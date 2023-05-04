---
title: cachelab2
date: 2023-05-03 23:27:15
tags: 
- 计算机系统结构
- cache
- 硬件
categories:
- 华科实验
---





## 实验要求



在trans.c中使用C语言编写一个实现矩阵转置的函数transpose_submit。即对于给定的矩阵`Am×n`，得到矩阵`Bn×m`，使得对于任意`0<=i<n、0<=j<m,有B[j][i]=A[i][j]`，其并且使函数调用过程中对cache的不命中数miss尽可能少。





### 相关知识

在进行转置的过程中，矩阵A和矩阵B在从内存加载到Cache中的时候会映射到同一个块中，从而导致当A和B元素的下标相同的时候会发生冲突。Cache会因此重复的加载块。为了避免这一冲突，我们尽量让已加载的数据先进行处理，从而最大的避免冲突。具体的做法为将矩阵进行分块处理，分块之后

* $32\times32$矩阵

  对于这样的矩阵，我们可以将其分为$8\times8$的矩阵进行处理，最大减少冲突。

* $64\times64$矩阵

  对于该矩阵，如果按上面的方法操作优化不足。现在考虑将矩阵划分成$4\times4$的大小。每一个元素的加载顺序如下：

  ```c
  //先将A[r],A[r+1],A[r+2]加载到内存中
  				v0 = A[r][c];
  				v1 = A[r+1][c];
  				v2 = A[r+2][c];
  				v3 = A[r+2][c+1];
  				v4 = A[r+2][c+2];
  				//加载B[c+3]
  				B[c+3][r] = A[r][c+3];
  				B[c+3][r+1] = A[r+1][c+3];
  				B[c+3][r+2] = A[r+2][c+3];  //此时A[r+2]的数据已经全部使用完毕
  				//加载B[c+2]
  				B[c+2][r] = A[r][c+2];
  				B[c+2][r+1] = A[r+1][c+2];
  				B[c+2][r+2] = v4;          //此时不必担心出现冲突
  				v4 = A[r+1][c+1];
  				//加载B[c+1]
  				B[c+1][r] = A[r][c+1];   //A[r]元素全部使用完毕
  				B[c+1][r+1] = v4;
  				B[c+1][r+2] = v3;
  				
  				B[c][r] = v0;
  				B[c][r+1] = v1;
  				B[c][r+2] = v2;
  				
  				B[c][r+3] = A[r+3][c];
  				B[c+1][r+3] = A[r+3][c+1];
  				B[c+2][r+3] = A[r+3][c+2];
  				v0 = A[r+3][c+3];          //A[r+3]元素使用完毕
  				
  				B[c+3][r+3] = v0;
  ```

  在该顺序下对于对角线上的元素每个$4\times4$的矩阵块只会造成一次多余的块的加载，从而大大的减少了miss的次数

* $61\times67$矩阵

  对于这样的矩阵，采用$32\times32$的策略就可以通过，但是要注意矩阵块大小无法被8整除，因此要检查是否超出边界。

具体代码如下：
```c
void transpose_submit(int M, int N, int A[N][M], int B[M][N])
{ 
int blockSize; 
int r, c; 
int temp = 0, d = 0; 
int v0,v1,v2,v3,v4; 
	
	if (N == 32)
	{
		blockSize = 8;
		for(int blockForCol = 0; blockForCol < N; blockForCol += 8)
		{
			for(int blockForRow = 0; blockForRow < N; blockForRow += 8)
			{
				for(r = blockForRow; r < blockForRow + 8; r++)
				{
					for(c = blockForCol; c < blockForCol + 8; c++)
					{
						
						if(r != c)
						{
							B[c][r] = A[r][c];
						}
						
						else 
						{
						//Store in temp instead of missing in B[j][i] to decrease misses
						temp = A[r][c];
						d = r;
						}
					}
					//为了防止相同的下标元素导致冲突，让对角线上的元素在块内所有元素被读取完之后再进行加载。
					if (blockForRow == blockForCol)	
					{
						B[d][d] = temp;
					}
				}
			}
		}
	}

	//block size设为4
	else if (N == 64)
	{	
 		blockSize = 4;
		for(r = 0; r < N; r += blockSize)
		{
			for(c = 0; c < M; c += blockSize)
			{
				//先将A[r],A[r+1],A[r+2]加载到内存中
				v0 = A[r][c];
				v1 = A[r+1][c];
				v2 = A[r+2][c];
				v3 = A[r+2][c+1];
				v4 = A[r+2][c+2];
				//加载B[c+3]
				B[c+3][r] = A[r][c+3];
				B[c+3][r+1] = A[r+1][c+3];
				B[c+3][r+2] = A[r+2][c+3];  //此时A[r+2]的数据已经全部使用完毕
				//加载B[c+2]
				B[c+2][r] = A[r][c+2];
				B[c+2][r+1] = A[r+1][c+2];
				B[c+2][r+2] = v4;          //此时不必担心出现冲突
				v4 = A[r+1][c+1];
				//加载B[c+1]
				B[c+1][r] = A[r][c+1];   //A[r]元素全部使用完毕
				B[c+1][r+1] = v4;
				B[c+1][r+2] = v3;
				
				B[c][r] = v0;
				B[c][r+1] = v1;
				B[c][r+2] = v2;
				
				B[c][r+3] = A[r+3][c];
				B[c+1][r+3] = A[r+3][c+1];
				B[c+2][r+3] = A[r+3][c+2];
				v0 = A[r+3][c+3];          //A[r+3]元素使用完毕
				
				B[c+3][r+3] = v0;
			}
		}
	}

	
	else 
	{
		blockSize = 8;
		
		for (int blockForCol = 0; blockForCol < M; blockForCol += blockSize)
		{
			for (int blockForRow = 0; blockForRow < N; blockForRow += blockSize)
			{	
				//不能被blocksize整除
				for(r = blockForRow; (r < N) && (r < blockForRow + blockSize); r++)
				{
					for(c = blockForCol; (c < M) && (c < blockForCol + blockSize); c++)
					{
						
						if (r != c)
						{
							B[c][r] = A[r][c];
						}
						
						
						else
						{
							temp = A[r][c];
							d = r;
						}
					}
					
					
					if(blockForRow == blockForCol) 
					{
						B[d][d] = temp;
					}
				}
			}
		}
	}
}
```



