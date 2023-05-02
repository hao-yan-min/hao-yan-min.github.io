---
title: Vue
date: 2023-05-01 23:48:03
tags:
- Vue
- 网页设计
- 前端学习
categories:
- 前端学习
---

### 导入Vue

使用语句

```html
<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
```

将Vue导入文件中。



### 创建绑定Vue实例

在js中创建Vue实例

```html
<script>
        var app = new Vue({
            el: "#app",
            data: {
                msg: "Hello World!"
            }
        })
    </script>
```

el为挂载点，表示该Vue的生效范围，其中"#app"表示生效的组件的id为"app"。





### Vue指令



#### v-text

![image-20230101170617149](/images/image-20230101170617149.png)

段落中的内容会被表达式替换



#### v-html

v-html指令和v-text指令相比，可以将vue中的字符串以html的格式进行渲染。





#### v-on



v-on指令可以将元素与事件进行绑定



html事件

| 属性                                                         | 值     | 描述                                           |
| :----------------------------------------------------------- | :----- | :--------------------------------------------- |
| [onclick](https://www.w3school.com.cn/tags/event_onclick.asp) | script | 元素上发生鼠标点击时触发。                     |
| [ondblclick](https://www.w3school.com.cn/tags/event_ondblclick.asp) | script | 元素上发生鼠标双击时触发。                     |
| ondrag                                                       | script | 元素被拖动时运行的脚本。                       |
| ondragend                                                    | script | 在拖动操作末端运行的脚本。                     |
| ondragenter                                                  | script | 当元素元素已被拖动到有效拖放区域时运行的脚本。 |
| ondragleave                                                  | script | 当元素离开有效拖放目标时运行的脚本。           |
| ondragover                                                   | script | 当元素在有效拖放目标上正在被拖动时运行的脚本。 |
| ondragstart                                                  | script | 在拖动操作开端运行的脚本。                     |
| ondrop                                                       | script | 当被拖元素正在被拖放时运行的脚本。             |
| [onmousedown](https://www.w3school.com.cn/tags/event_onmousedown.asp) | script | 当元素上按下鼠标按钮时触发。                   |
| [onmousemove](https://www.w3school.com.cn/tags/event_onmousemove.asp) | script | 当鼠标指针移动到元素上时触发。                 |
| [onmouseout](https://www.w3school.com.cn/tags/event_onmouseout.asp) | script | 当鼠标指针移出元素时触发。                     |
| [onmouseover](https://www.w3school.com.cn/tags/event_onmouseover.asp) | script | 当鼠标指针移动到元素上时触发。                 |
| [onmouseup](https://www.w3school.com.cn/tags/event_onmouseup.asp) | script | 当在元素上释放鼠标按钮时触发。                 |
| onmousewheel                                                 | script | 当鼠标滚轮正在被滚动时运行的脚本。             |
| onscroll                                                     | script | 当元素滚动条被滚动时运行的脚本。               |

绑定事件的基本语法如图

![image-20230101171907823](/images/image-20230101171907823.png)

当绑定的事件不传参的时候，函数默认有一个传入的参数event，包含了所有的事件信息。其中e.target为触发了事件的组件。

当需要传入参数的时候，为了将事件传入，可以使用$event作为参数之一传入。



![image-20230103181121230](/images/image-20230103181121230.png)



### 按键修饰符

![image-20230104120759345](/images/image-20230104120759345.png)



#### 事件修饰符



![image-20230103181602959](/images/image-20230103181602959.png)



#### v-bind

v-bind可以用于绑定元素的属性

![image-20230102131405893](/images/image-20230102131405893.png)



#### v-for

![image-20230102131617651](/images/image-20230102131617651.png)

使用v-for进行列表渲染，具体语法和微信小程序类似

```html
<li v-for="(item,index) in list">内容是{{item.content}}</li>
```



### v-model

v-model可以实现数据的双向绑定，即当绑定的元素改变时，Vue中的值也会改变。

![image-20230104121557050](/images/image-20230104121557050.png)

![image-20230104122044579](/images/image-20230104122044579.png)

### js获取元素和绑定事件

使用document.getElementById()可以获取某个元素，可以根据获得的该元素在js中改变元素的属性



### 网页初始化函数

![image-20230102141838661](/images/image-20230102141838661.png)

类似微信小程序中onload函数，是在页面加载的时候执行的





### Webpack

Webpack的使用方法为运行脚本，具体指令为'npm run dev'如果脚本的名称为dev。Webpack的默认输入文件为src/index.js；输出路径为dist/main.js。如果要改变，就在Webpack.config.js中修改配置

![image-20230103132704930](/images/image-20230103132704930.png)



### 监听器



监听器可以监视某一数值的变化，让程序员可以在数据发生变化的时候进行相应的操作，具体的定义方式如下：

```html
 watch: {
                msg(newval, old) {
                    console.log(newval, old);
                }
            }
```

为了让监听器在一开始就运行，可以将侦听器定义为一个对象，对象名为监视对象，添加immediate属性为true。具体代码如下：

```html
watch: {
                msg: {
                    handler(newval, old) {
                        console.log(newval)
                    },
                    immediate: true
                }
            }
```

当对象的其中一个属性发生变化时，侦听器不会被触发，为了侦听此类变化，可以将侦听器设置为对象并将其deep属性设置为true。

```html
watch: {
                msg: {
                    handler(newval, old) {
                        console.log(newval.info)
                    },
                    immediate: true,
                    deep: true
                }
            }
```







### 使用Vue-cli创建项目

在控制台中输入

```bash
vue create project-name
```

就可以创建项目。

### Vue组件

vue组件由三部分构成，分别是&lt;template&gt;  &lt;style&gt;以及&lt;script&gt; 组成。在main.js中引入相应组件并使用render函数进行渲染。具体代码如下：

```vue
import Vue from 'vue'
import App from './App.vue'
Vue.config.productionTip = false

new Vue({
    render: h => h(App),
}).$mount('#app')
```

其中$mount('#app')的作用和定义在Vue构造函数中的el:'#app'相同。



* 在vue的template部分中，定义了组件包含的模板部件

* 在vue的style中包含了组件的样式表

* 在vue的script中包含了组件脚本，默认的语法为

  ```vue
  <script>
  export default {
      data(){
          return{
              msg:"hello"
          }
      }
  
  }
  </script>
  ```

  其中的data默认为函数，需要定义的数据放在return的元组中。

  除此之外，还有methods用于定义方法，还有components用于注册使用的组件。

* <b>组件的注册和使用</b>

  * 组件在使用之前，要在script中提前import，并在Vue的methods元素中注册，具体代码如下：

    ```vue
    import lis from '@/components/elementlist.vue'
    export default {
        data(){
            return{
                msg:"hello"
            }
        },
        
       
        components:{
            lis
        }
    
    }
    </script>
    ```

    此时注册的组件为私有组件。如果希望在全局进行注册，则应该在main.js中引入该组件并在Vue.component('name',component)

  * 组件的props属性

    为了让组件能够实现自定义其中的某些参数，需要声明某些属性，使用props声明，具体代码如下：

    ```Vue
    
    ```

    