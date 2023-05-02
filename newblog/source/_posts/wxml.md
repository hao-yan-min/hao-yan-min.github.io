---
title: wxml
date: 2023-05-02 00:08:30
tags:
- 微信小程序
- 前端学习
---





# wxml

## wxml负责布局 

### view 

&lt;view>  相当与**标签**，是用来划分文档内大段的

```html
<view class="container">
   <view>A</view> 
</view>
```

其中的class表示需要应用的样式。其中样式的编写在wxss中完成。在.wxss中使用.container view{} 来制定该样式下所有的的view的基本样式。使用nth-child(1)来为应用了container内的样式的第一个view增加特质。text-align为view的文本居中

```css
.container view{
  width:100px; <!--px为长度单位-->
  text-align: center
}
.container view:nth-child(1){
  background-color: red;
}
```

若要规定container本身的样式，则需要在wxss中进行设置

```css
.container{
border: solid 1px red;
}
```

### scroll-view 

使用<scroll-view class="container" scroll-y> 可以设置滚动效果并且为沿y轴滚动。当container的大小容不下所有的标签组件时可以看到滚动效果

1. <swiper> 和<swiper-item>可以生成滑块效果。<swiper>下的每一项都是<swiper-item>

   可以在<swiper-item>下添加<view>模块，模块的样式“item"在wxss中编写。indicator-dots可以添加圆点，autoplay可以让滑块自动播放，interval可以设置自动播放的时间。circular使滑块平滑播放。

   ```html
   <swiper class="swiper-container" indicator-dots autoplay interval="3000" circular>
   <swiper-item>
   <view class="item">A</view>
   </swiper-item>
   <!--第一个-->
   <swiper-item>
   <view class="item">B</view>
   </swiper-item>
   <swiper-item>
   <view class="item">C</view>
   </swiper-item>
   </swiper>
   ```

   在wxss中，如果要改变swiper-item的组件的样式，可以使用swiper-item:nth-child(1) .item{}对组件进行修改。

   ```css
   .swiper-container{
     height: 150px;
   }
   .item{
     height: 100%;
     line-height: 150px;
     text-align: center;
   }
   swiper-item:nth-child(1) .item{
     background-color: red;
   }
   swiper-item:nth-child(2) .item{
     background-color: lightblue;
   }
   swiper-item:nth-child(3) .item{
     background-color: lightgreen;
   }
   ```

   ==swiper-item在设置的时候前面没有**.**而且.item前面有空格==

2. <text>组件可以用来生成文本

3. <bottom>组件可以生成按钮 。type="primary" type="warning"可以改变按钮的颜色

4. <image> 组件可以插入图片。src="\image\1.png"表示图片的路径；mode=""为图片的样式。heightFix为保持图片的高不变，改变组件的宽以适应图片。widthFix类似。在wxss中以image{}改变配置

   ## 小程序开发的API

   * 事件监听API

   * 同步API

   * 异步API

     ## 数据绑定

​    在page的js文件的data:中声明变量，在wxml文件中可以使用``{{}}``进行调用

```css
data: {
       info: 'hello world',
       imasrc: '/image/1.6267827695216963E9.jpg'
  },
```

```html
<view>{{info}}</view>
<image src="{{imasrc}}" mode="widthFix"></image>
```

## 事件绑定

小程序中常用事件有

|  类型  |  绑定方式  |
| :----: | :--------: |
|  tap   |  bindtap   |
| input  | bindinput  |
| change | bindchange |

```html
<button type="primary" bindtap="myTapHandler">按钮</button>
```

其中myTapHandler()函数在js文件中定义

```javascript
myTapHandler(e)
  {
    console.log(e)
  },
```

==如果要通过函数改变数据的值，则可以通过setData()函数== 注意调用类内的数据时要加上this.data

```js
Page({

  /**
   * 页面的初始数据
   */
  data: {
       count: 1
  },
  myTapHandler(e)
  {
    console.log(e)
  },
  myTapHandler1(e)
  {
    this.setData({
      count: this.data.count + 1
    })
  }
})
```

如果需要传递参数，则在wxml中进行定义, 使用 data-**="" 的格式进行传参，可以使用``{{}}``的语法

```html
<button type="primary" bindtap="myTapHandler1" data-info="{{5}}">按钮</button>
```

在调用的时候使用event.target.dataset找到参数

```js
myTapHandler1(event)
  {
    this.setData({
      count: this.data.count + 1
    })
    console.log(event.target.dataset.info)
  }
```

在tap事件中，event.detail包含触碰的位置信息。在input事件中，detail.value为输入的内容，placeholder为输入的提示

```html
<input bindinput="myinput" placeholder="请输入内容"></input>
```



<b>在input中文本框显示的内容为其value，可以通过改变value改变显示的内容</b>

```js
myinput(e){
    console.log(e.detail.value)
  },
```

### form组件

表单。将组件内的用户输入的[switch](https://developers.weixin.qq.com/miniprogram/dev/component/switch.html) [input](https://developers.weixin.qq.com/miniprogram/dev/component/input.html) [checkbox](https://developers.weixin.qq.com/miniprogram/dev/component/checkbox.html) [slider](https://developers.weixin.qq.com/miniprogram/dev/component/slider.html) [radio](https://developers.weixin.qq.com/miniprogram/dev/component/radio.html) [picker](https://developers.weixin.qq.com/miniprogram/dev/component/picker.html) 提交。

当点击 [form](https://developers.weixin.qq.com/miniprogram/dev/component/form.html) 表单中 form-type 为 submit 的 [button](https://developers.weixin.qq.com/miniprogram/dev/component/button.html) 组件时，会将表单组件中的 value 值进行提交，需要在表单组件中加上 name 来作为 key。

## 属性说明

| 属性                  | 类型        | 默认值 | 必填 | 说明                                                         |
| :-------------------- | :---------- | :----- | :--- | :----------------------------------------------------------- |
| report-submit         | boolean     | false  | 否   | 是否返回 formId 用于发送[模板消息](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/template-message.html) |
| report-submit-timeout | number      | 0      | 否   | 等待一段时间（毫秒数）以确认 formId 是否生效。如果未指定这个参数，formId 有很小的概率是无效的（如遇到网络失败的情况）。指定这个参数将可以检测 formId 是否有效，以这个参数的时间作为这项检测的超时时间。如果失败，将返回 requestFormId:fail 开头的 formId |
| bindsubmit            | eventhandle |        | 否   | 携带 form 中的数据触发 submit 事件，event.detail = {value : {'name': 'value'} , formId: ''} |
| bindreset             | eventhandle |        | 否   | 表单重置时会触发 reset 事件                                  |

```html
<!--点击提交后会执行的函数-->
<form bindsubmit="bntSub">
    <input name="value" placeholder="输入"></input>
    <button form-type="submit">提交</button>
    <button form-type="reset">重置</button>        
</button>    
</button>
</form>
```

其中input组件的name属性决定了该输入在表单中的属性名称

在”bntSub“中可以调用这些信息。信息存储在event.detail.value中，用设定的索引名称去引用

## 条件渲染

在小程序中，使用``wx:if="{{condition}}"``的方式选择是否渲染模块，类似python，可以使用elif语句

```html
<block wx:if="{{count<3}}">
  <button type="primary" bindtap="myTapHandler1" data-info="{{5}}">按钮</button>
  <input bindinput="myinput" ></input>
</block>
<block wx:elif="{{count===3}}">
 <text>点击次数达到三次</text>
 <button type="primary" bindtap="myTapHandler1" data-info="{{4}}">点击</button>
</block>
<block wx:else>
 <text>超过三次了</text>
</block>

```

block为块，在编译后不需要被渲染

## 列表渲染

列表渲染的基本语法是``wx:for="{{array}}"``其中每项索引为index，内容为item。如果数组内存着键值，则可以用键值进行遍历。语法为wx:key="keyname"

```html
<view wx:for="{{array}}">
索引为{{index}},内容为{{item}}
</view>
```

## 单选框

```html
<radio-group class="radio-group" bindchange="radioChange">
  <label class="radio" wx:for="{{items}}">
    <radio value="{{item.name}}" checked="{{item.checked}}" />
    {{item.value}}
  </label>
</radio-group>
```

其中radio-group组件中可以包含多个radio选项，可以用来捕捉change事件

## 弹窗设置

弹窗组件为modal。 hidden为是否显示的属性，bindconfirm为按确认键后绑定的发生事件。bindcancel为按取消键后的绑定事件

```html
<modal hidden="{{modalhidden}}" bindconfirm="myTapHandler1" bindcancel="myTapHandler2">
<image src="/image/passpart.cc679602.png" mode="widthFix"></image>
</modal>
```

validateNumber()函数可以验证输入是否为数字

## 定时函数

使用setinterval(function(){},timeout)可以设置一个定时函数，函数每隔timeout毫秒执行一次function，在function中无法使用this，因此要在外面设置that=this来代替使用。如果想要停止interval，则需要调用clearinterval函数

# 微信云服务



## 数据库的查询

使用==wx.cloud.database==函数对数据库进行引用。假设有一个数据库的环境名为“test"，则可以用如下代码获取引用

```js
const db=wx.cloud.database(
{
    env: "test"
})
//为了获取集合，可以使用collection函数，doc可以筛选id
const todos=db.collection("todos")
todos.get.doc("_id")({
    success:res=>{
        console.log(res.data)
    }
})
//也可以使用链式调用
todos.get().then(res=>{}).then(res=>{}).end(err=>{})
//如果要筛选查询的数据，需要用到where函数
todos.where({
    author:"jack"
    progress: db.command.gt(30)
}).get().then(res=>{})
```

常用的查询指令：

| 查询指令 | 说明                 |
| :------- | :------------------- |
| eq       | 等于                 |
| neq      | 不等于               |
| lt       | 小于                 |
| lte      | 小于或等于           |
| gt       | 大于                 |
| gte      | 大于或等于           |
| in       | 字段值在给定数组中   |
| nin      | 字段值不在给定数组中 |

使用where().get()时返回的res.data为满足条件的数组

**如果需要随机返回几条满足条件的记录** ：可以使用

```js
const List = db.collection("name").aggregate().sample(size:1).end()//返回的对象为一个aggregate，数据储存在List.list里面
```

#### aggregate.lookup()

使用lookup函数可以跨表查询。具体的使用方法为：

```js
db.collection('fav').aggregate()
			.lookup({
				from: 'store',
				localField: 'fav_id',
				foreignField: '_id',
				as: 'storedetail',
			})
			.match({
				_openid: event.userInfo.openId
			})
			.end()
```

<b>避坑提示：使用云函数的时候，直接返回整个查询结果，不要用promise返回res</b>



## 数据库的插入

### 插入一条数据  

使用db.collection("todos").add()函数可以增加一条数据

```js
db.collection('todos').add({
  // data 字段表示需新增的 JSON 数据
  data: {
    description: "learn cloud database",
    due: new Date("2018-09-01"),
    tags: [
      "cloud",
      "database"
    ],
    location: new db.Geo.Point(113, 23),
    done: false
  }
})
.then(res => {
  console.log(res)
})
//或者用success函数
db.collection('todos').add({
  // data 字段表示需新增的 JSON 数据
  data: {
    // _id: 'todo-identifiant-aleatoire', // 可选自定义 _id，在此处场景下用数据库自动分配的就可以了
    description: "learn cloud database",
    due: new Date("2018-09-01"),
    tags: [
      "cloud",
      "database"
    ],
    // 为待办事项添加一个地理位置（113°E，23°N）
    location: new db.Geo.Point(113, 23),
    done: false
  },
  success: function(res) {
    // res 是一个对象，其中有 _id 字段标记刚创建的记录的 id
    console.log(res)
  }
})
```

**tips**:使用wx.showLoading函数可以生成加载图像

```js
wx.showLoading({
    title:"Loading...",
    mask:true
})

//结束加载图片
wx.hideLoading()
```



### 插入一个表单

使用form组件生成一个表单之后在form的bindsubmit绑定的函数进行表单提交

```html
<form bindsubmit="mytap">
<input name="num1" style="border: 1px solid grey;"></input>
<input name="op" style="border: 1px solid grey;"></input>
<input name="num2" style="border: 1px solid grey;"></input>
<button type="primary" form-type="submit">提交</button>
<button form-type="reset">重置</button>
</form>
```

```js
mytap(e)
  {
    const demo=db.collection("demolist")
    var{num1,op,num2}=e.detail.value
    demo.add({
      data:{
        id:1,
        num1:num1,
        op:op,
        num2:num2
      }
    }).then(res=>{
      console.log(e)
    })
  }
```



### 更新数据

  使用update函数可以实现局部更新

```js
db.collection('todos').doc('todo-identifiant-aleatoire').update({
  // data 传入需要局部更新的数据
  data: {
    // 表示将 done 字段置为 true
    done: true
  },
  success: function(res) {
    console.log(res.data)
  }
})
```

除了用指定值更新字段外，数据库 API 还提供了一系列的更新指令用于执行更复杂的更新操作，更新指令可以通过 `db.command` 取得：

| 更新指令 | 说明                                   |
| :------- | :------------------------------------- |
| set      | 设置字段为指定值                       |
| remove   | 删除字段                               |
| inc      | 原子自增字段值                         |
| mul      | 原子自乘字段值                         |
| push     | 如字段值为数组，往数组尾部增加指定值   |
| pop      | 如字段值为数组，从数组尾部删除一个元素 |
| shift    | 如字段值为数组，从数组头部删除一个元素 |
| unshift  | 如字段值为数组，往数组头部增加指定值   |

```js
const _ = db.command
db.collection('todos').doc('todo-identifiant-aleatoire').update({
  data: {
    style: _.set({
      color: 'blue'
    })
  },
  success: function(res) {
    console.log(res.data)
  }
})
```

如果需要更新多个数据，需在 Server 端进行操作（云函数），在 `where` 语句后同样的调用 `update` 方法即可，比如将所有未完待办事项的进度加 10%：

```js
// 使用了 async await 语法
const cloud = require('wx-server-sdk')
const db = cloud.database()
const _ = db.command

exports.main = async (event, context) => {
  try {
    return await db.collection('todos').where({
      done: false
    })
    .update({
      data: {
        progress: _.inc(10)
      },
    })
  } catch(e) {
    console.error(e)
  }
}
```

#### 使用command.push指令为数据库的数组增加元素

| 属性     | 类型        | 默认值 | 必填 | 说明                             |
| :------- | :---------- | :----- | :--- | :------------------------------- |
| each     | Array.<any> |        | 是   | 要插入的所有元素                 |
| position | number      |        | 否   | 从哪个位置开始插入，不填则是尾部 |
| sort     | number      |        | 否   | 对结果数组排序                   |
| slice    | number      |        | 否   | 限制结果数组长度                 |

```js
const _ = db.command
db.collection('todos').doc('doc-id').update({
  data: {
    tags: _.push(['mini-program', 'cloud'])
  }
})
```



### 监视database的变化

监听集合中符合查询条件的数据的更新事件。使用 `collection.watch`函数

| 属性     | 类型     | 默认值 | 必填 | 说明                                                         |
| :------- | :------- | :----- | :--- | :----------------------------------------------------------- |
| onChange | function |        | 是   | 成功回调，回调传入的参数 snapshot 是变更快照，snapshot 定义见下方 |
| onError  | function |        | 是   | 失败回调                                                     |

| 属性  | 类型     | 说明                                                       |
| :---- | :------- | :--------------------------------------------------------- |
| close | function | 关闭监听，无需参数，返回 Promise，会在关闭完成时 `resolve` |

#### snapshot 说明

| 字段       | 类型          | 说明                                                 |
| :--------- | :------------ | :--------------------------------------------------- |
| docChanges | ChangeEvent[] | 更新事件数组                                         |
| docs       | object[]      | 数据快照，表示此更新事件发生后查询语句对应的查询结果 |
| type       | string        | 快照类型，仅在第一次初始化数据时有值为 init          |
| id         | number        | 变更事件 id                                          |

#### ChangeEvent 说明

| 字段          | 类型     | 说明                                                         |
| :------------ | :------- | :----------------------------------------------------------- |
| id            | number   | 更新事件 id                                                  |
| queueType     | string   | 列表更新类型，表示更新事件对监听列表的影响，枚举值，定义见 QueueType |
| dataType      | string   | 数据更新类型，表示记录的具体更新类型，枚举值，定义见 DataType |
| docId         | string   | 更新的记录 id                                                |
| doc           | object   | 更新的完整记录                                               |
| updatedFields | object   | 所有更新的字段及字段更新后的值，key 为更新的字段路径，value 为字段更新后的值，仅在 `update` 操作时有此信息 |
| removedFields | string[] | 所有被删除的字段，仅在 `update` 操作时有此信息               |

```js
var player=db.collection("rooms").doc(docid).watch({
     onError:error=>{console.log(error)},
     onChange: snapshot=>{
       if(snapshot.docChanges[0].dataType=="update"&&snapshot.docs[0].peoplenum==2)
       {
          console.log("匹配成功")
          player.close()
          this.communi()
       }
     }
   })	
```



## 云函数的创建

```js
const cloud = require('wx-server-sdk')

cloud.init({ env: cloud.DYNAMIC_CURRENT_ENV }) // 使用当前云环境
const db=cloud.database()
// 云函数入口函数
exports.main = async (event, context) => {
  db.collection(event.collection).doc(event.docid).update({
    data:{
      peoplenum:event.peoplenum,
      enable:false
    },
  })
}
```

## wx.createSelectorQuery()

在js页面进行组件的交互

# [微信小程序官方文档](https://developers.weixin.qq.com/miniprogram/dev/wxcloud/reference-sdk-api/database/aggregate/Aggregate.lookup.html)





