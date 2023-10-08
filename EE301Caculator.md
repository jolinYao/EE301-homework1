### EE301|Make a WeChat Caculator

- #### Introduction

  This blog mainly introduces how I designed a **Wechat mini program calculator**, it can achieve basic operations, including addition, subtraction, multiplication, division, power and trigonometric functions, in the next will introduce some related background and design process and code, and finally the product display.

  **Link to the finished project code：**[jolinYao/EE301-homework1: EE301 | homework1 WeChat Calculator (github.com)](https://github.com/jolinYao/EE301-homework1)

- #### Personal information

  | The Link Your Class                                  | [2301-MUSE社区-CSDN社区云](https://bbs.csdn.net/forums/ssynkqtd-04) |
  | ---------------------------------------------------- | :----------------------------------------------------------: |
  | **The Link** **of Requirement of This Assignment**** | **[Software Engineering Practice First Assignment-CSDN社区](https://bbs.csdn.net/topics/617332156)** |
  | **The Aim of This Assignment**                       |              **Wechat mini program calculator**              |
  | **MU STU ID and FZU STU ID**                         |                    **21125422_832101225**                    |

- #### PSP Table

  | **Personal Software Process Stages**    | Estimated Time（minutes） | Actual Time（minutes） |
  | :-------------------------------------- | :------------------------ | :--------------------- |
  | **Planning**                            | **20**                    | **25**                 |
  | • Estimate                              | 20                        | 25                     |
  | **Development**                         | **415**                   | **515**                |
  | • Analysis                              | 30                        | 30                     |
  | • Design Spec                           | 5                         | 5                      |
  | • Design Review                         | 10                        | 10                     |
  | • Coding Standard                       | 40                        | 40                     |
  | • Design                                | 60                        | 80                     |
  | • Coding                                | 180                       | 240                    |
  | • Code Review                           | 60                        | 80                     |
  | • Test                                  | 30                        | 30                     |
  | **Reporting**                           | **100**                   | **125**                |
  | • Test Repor                            | 60                        | 75                     |
  | • Size Measurement                      | 10                        | 20                     |
  | • Postmortem & Process Improvement Plan | 30                        | 30                     |
  | **Sum**                                 | **535**                   | **665**                |

- #### Thoughts before doing the project

  在开始之前有几个小问题：

  1. 怎么开发一个小程序？（用什么环境、什么软件）

     > 1. 申请账号
     >
     > 2. 安装开发者工具
     >
     >    为了帮助开发者简单和高效地开发和调试微信小程序，我们在原有的公众号网页调试工具的基础上，推出了全新的 [微信开发者工具](https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html)，集成了公众号网页调试和小程序调试两种开发模式。
     >
     >    - 使用公众号网页调试，开发者可以调试微信网页授权和微信JS-SDK [详情](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421141115)
     >    - 使用小程序调试，开发者可以完成小程序的 API 和页面的开发调试、代码查看和编辑、小程序预览和发布等功能。

  2. 小程序代码构成是什么？
     通过查看我可以发现小程序的后缀分为4种：
     `.json`、`.wxml`、`.wxss`和`.js`

     1. `.json` 后缀的 `JSON` 配置文件
     2. `.wxml` 后缀的 `WXML` 模板文件----------对标网页编程`HTML`(描述当前这个页面的结构)
     3. `.wxss` 后缀的 `WXSS` 样式文件----------对标网页编程`css`(描述页面的样子)
     4. `.js` 后缀的 `JS` 脚本逻辑文件(处理这个页面和用户的交互)

     > - ##### JSON 配置
     >
     >   JSON 是一种数据格式，并不是编程语言，在小程序中，JSON扮演的静态配置的角色。
     >
     >   我们可以看到在项目的根目录有一个 `app.json` 和 `project.config.json`，此外在 `pages/logs` 目录下还有一个 `logs.json`
     >
     >   - 小程序配置app.json文件
     >
     >     `app.json` 是当前小程序的全局配置，包括了小程序的所有页面路径、界面表现、网络超时时间、底部 tab 等
     >     其中：
     >     `pages`字段 —— 用于描述当前小程序所有页面路径，这是为了让微信客户端知道当前你的小程序页面定义在哪个目录
     >     `window`字段 —— 定义小程序所有页面的顶部背景颜色，文字颜色定义等
     >
     >   - 工具配置project.config.json
     >     在工具上做的任何配置都会写入到这个文件，当你重新安装工具或者换电脑工作时，你只要载入同一个项目的代码包，开发者工具就自动会帮你恢复到当时你开发项目时的个性化配置
     >
     >   - 页面配置page.json
     >     `page.json` 其实用来表示 pages/logs 目录下的 `logs.json` 这类和小程序页面相关的配置
     >     可以独立定义每个页面的一些属性，例如刚刚说的顶部颜色、是否允许下拉刷新等
     >
     >   - **注意：！！！**
     >
     >     *JSON文件都是被包裹在一个大括号中 {}，通过key-value的方式来表达数据。JSON的Key必须包裹在一个双引号中，在实践中，编写 JSON 的时候，忘了给 Key 值加双引号或者是把双引号写成单引号是常见错误。*
     >
     >     *JSON的值只能是以下几种数据格式，其他任何格式都会触发报错，例如 JavaScript 中的 undefined。*
     >
     >     1. *数字，包含浮点数和整数*
     >     2. *字符串，需要包裹在双引号中*
     >     3. *Bool值，true 或者 false*
     >     4. *数组，需要包裹在方括号中 []*
     >     5. *对象，需要包裹在大括号中 {}*
     >     6. *Null*
     >
     >     *还需要注意的是 JSON 文件中无法使用注释，试图添加注释将会引发报错*
     >
     > - **WXML模板**
     >   `WXML` 由标签、属性等等构成
     >   HTML 的时候，经常会用到的标签是 `div`, `p`, `span`，开发者在写一个页面的时候可以根据这些基础的标签组合出不一样的组件，例如日历、弹窗等等; 小程序的 `WXML` 用的标签是 `view`, `button`, `text` 等等，这些标签就是小程序给开发者包装好的基本能力，还提供了地图、视频、音频等等组件能力
     >
     > - **WXSS样式**
     >
     >   `WXSS` 具有 `CSS` 大部分的特性，小程序在 `WXSS` 也做了一些扩充和修改。
     >
     >   1. 新增了尺寸单位。在写 `CSS` 样式时，开发者需要考虑到手机设备的屏幕会有不同的宽度和设备像素比，采用一些技巧来换算一些像素单位。`WXSS` 在底层支持新的尺寸单位 `rpx` ，开发者可以免去换算的烦恼，只要交给小程序底层来换算即可，由于换算采用的浮点数运算，所以运算结果会和预期结果有一点点偏差。
     >   2. 提供了全局的样式和局部样式。和前边 `app.json`, `page.json` 的概念相同，你可以写一个 `app.wxss` 作为全局样式，会作用于当前小程序的所有页面，局部页面样式 `page.wxss` 仅对当前页面生效。
     >   3. 此外 `WXSS` 仅支持部分 `CSS` 选择器
     >
     > - **JS逻辑交互**
     >   小程序里边，我们就通过编写 `JS` 脚本文件来处理用户的操作

  3. 怎么实现计算器功能？

     > 将在接下来的过程中展示

- #### Design Process

  **目标：**

  1. Basic requirement: Implement addition, subtraction, multiplication, division, and clear functions.
  2. Advanced requirement: Implement functionality for exponentiation, trigonometric functions, and more

  **过程：**

  1. 写计算器的界面（标题、背景颜色、字体、是否允许下拉刷…）在`.wxml`文件中
  2. 整个界面的布局进行调整（四个按钮横向摆放、按钮的宽度、文体的居中…）在`.wxss`文件中
  3. 每个按钮添加一个事件，然后开始写逻辑层的代码，也就是我们点击之后整个界面会发生什么变化。 在文件`.js`文件中
  4. 设置清除和回退的功能按钮以及等于号的逻辑结构代码编写
  5. 写按钮按下之后会出现的效果

  **流程图：**
  ![697f5d08e8eb8e10b2c0818afec6d59](img/697f5d08e8eb8e10b2c0818afec6d59.png)

- #### Code description

In programming the calculator applet, most of the code are programming in the `Index.js`, `Index. WXML`, `Index.WXSS`, so I mainly show the three file code.
**index.js**:

```js
import * as util from './calculatorDep/util'
import {Checker} from './calculatorDep/BtnCheck'
import {calculate} from './calculatorDep/calculator_compiler'
const app = getApp()
const log = console.log
var checker=new Checker()
var PageWidth,PageHeight
var variblesDic={Ans:'0',X:'0',Y:'0',A:'0',B:'0',C:'0',D:'0',E:'0',F:'0'}
var mvX/**/,scrollLeft=0/**/,angle=true,charWidth=16.494791666666668

Page({
  data: {
    spaceHeight:0,
    answer:'',inputs:'',
    isHistoryShow:false,
    outClick:true, valsetHide:true, 
    mvOffset:0,autoLeftOffset:0,cursor_offset:0,
    fbtns:null,nbtns:null,
    varibles:null,curVar:{ key:'Ans',value:'0'},
    backgroundImg:null
  },
  //侧边栏隐藏
  pageClick(){ this.setData({outClick:true}) },
  //弹窗变量赋值
  valsetbtn(){
    this.setData({valsetHide:false})
  },
  valset(value){
    this.updateVaribles({value: parseFloat(value.detail)})
  },
  //函数滑动翻页
  moving:function(e){  mvX=e.detail.x; } ,
  autoMv:function(){
    var mvWidth=2*PageWidth
    var cretia=mvWidth/20; var mid=mvX+mvWidth/2
    if(this.data.mvOffset==0) this.setData({mvOffset:mvX<-cretia?-mvWidth/2:0})
    else this.setData({mvOffset:mid>cretia?0:-mvWidth/2})
  },
  //光标输入交互
  scrollListen:function(e){ scrollLeft=e.detail.scrollLeft },
  cursorJump:function(e){
    if(e.detail.x-.05*PageWidth>this.data.inputs.length*charWidth) return
    var scrollRight=this.data.inputs.length*charWidth-scrollLeft-.9*PageWidth
    var charCnt=Math.round((.95*PageWidth-e.detail.x+scrollRight)/charWidth)
    this.setData({cursor_offset:charWidth*(this.data.inputs.length-checker.justifyCursorCharcnt(this.data.inputs.length-charCnt))})
  },
  //更新变量列表
  updateVaribles(curVar){
    if(curVar==undefined)
      curVar=this.data.curVar
    else {
      if(!curVar.key) curVar.key=this.data.curVar.key
      variblesDic[curVar.key] = curVar.value
    }
    this.setData({varibles:Object.keys(variblesDic).map(key=>{
      return {key,value:variblesDic[key]}
    }),curVar:curVar})
  },
  button:function(e){
    var that=this
    function updateCursor(btn){
      if(checker.input(btn))
        that.setData({inputs:checker.toString(),cursor_offset:checker.justifyCursorCharcnt(-1,'right')*charWidth})
      else 
        wx.showToast({ title: '算式错误', icon: 'none', duration:300 });
    }
    var btn=e.currentTarget.dataset.info
    if(btn.type=='FUNC'){
      if(btn.value=='=') {
        variblesDic.Ans=parseFloat(calculate(checker.toString(false),angle,variblesDic).toPrecision(15))
        this.setData({answer:variblesDic.Ans})
        if(this.data.curVar.key=='Ans')
          this.updateVaribles({value:variblesDic.Ans})
      }
      if(btn.value=='DEL') {
        if(!checker.input(btn))wx.showToast({ title: '算式错误', icon: 'none', duration:300 });
        else this.setData({inputs:checker.toString()})
      }
      if(btn.value=='AC') {
        updateCursor(btn)
        this.setData({answer:'0'})
      }
      if(btn.value=='^2') {
        if(checker.input({value:'^'   ,type:'op'  ,text:'^'   })) {
          checker.input({value:'2',type:'num',text:'2'})
          var res=checker.toString()
          this.setData({inputs:res})//先刷新文本在移动文本框
          this.setData({autoLeftOffset:(res.length+1)*charWidth})
        }
      }
      if(btn.value=='角度制'||btn.value=='弧度制') {
        angle=!angle;
        this.data.nbtns[3][0]=angle?{value:'角度制',type:'FUNC',text:'角度制'}:{value:'弧度制',type:'FUNC',text:'弧度制'};
        this.setData({nbtns:this.data.nbtns})
      }
      if(btn.value=='保存'){
        this.updateVaribles({value:this.data.answer})
      }
    }
    else if(checker.input(btn)){
      var res=checker.toString()
      this.setData({inputs:res})//先刷新文本在移动文本框
      this.setData({autoLeftOffset:(res.length+1)*charWidth})
    }
    else wx.showToast({ title: '非法输入', icon: 'none', duration:300 });
    this.updateVaribles()
  },
  //历史记录操作
  historyShow(){
    this.setData({isHistoryShow:!this.data.isHistoryShow})
  },
  historyChoose(e){
    this.setData({curVar:e.currentTarget.dataset.info,isHistoryShow:false})
  },
  historyInput(e){
    var Var=e.currentTarget.dataset.info
    checker.input({value:Var.key,type:'var'})
    this.setData({isHistoryShow:false,inputs:checker.toString()})
  },
  onLoad: function () {
    PageWidth=wx.getSystemInfoSync().windowWidth
    PageHeight=wx.getSystemInfoSync().windowHeight;
    this.setData({spaceHeight:79-130*PageWidth/PageHeight})
    this.setData({nbtns:util.nBtns,fbtns:util.fBtns})
    this.setData({varibles:Object.keys(variblesDic).map(key=>{
      return {key,value:variblesDic[key]}
    })})
  },
  onShow(){
    if(app.globalData.backgroundImg!=null && this.data.backgroundImg!=app.globalData.backgroundImg) 
      this.setData({backgroundImg:app.globalData.backgroundImg})
    if(this.data.backgroundImg==null) this.setData({backgroundImg:'../../asset/images/back2.jpg'})
  }
})

```

**index.wxml**:

```html
<view class="calculator" bind:tap='pageClick'>
  <image class="bg" src="{{backgroundImg}}" mode='aspectFill'/>
  <view class="mask"></view>
  <view class="body">
    <view class="nav">
      <view class="icon"></view>
      <view class="text">科学</view>
    </view>
    <view class="space" style="height:{{spaceHeight*5/12}}vh"></view> 
    <view class="screen">
      <view class="answer">{{answer}}</view>
      <view class="space" style="height:{{spaceHeight/6}}vh"></view> 
      <scroll-view class="input-scroll" scroll-x="{{true}}" scroll-left="{{autoLeftOffset}}" bindscroll="scrollListen" bind:tap='cursorJump'>
        <view class='input-cbox' >
          <view class='input-content'> {{inputs}}</view>
          <view class='input-cursor_layer' style="right:{{cursor_offset}}px"></view>
        </view>
      </scroll-view>
    </view>
    
    <view class="space" style="height:{{spaceHeight*5/12}}vh"></view> 
    <view class="toolbar">
        <view class="container">
          <view class="back"></view>
          <view class="drop-menu" style="overflow: {{isHistoryShow?'visible':'hidden'}};">
            <view class="drop-btn" id="history" hover-class='drop-btn-hover' hover-stay-time='0' data-info='{{curVar}}' bindtap="historyShow" bind:longpress="historyInput">
              <view class="text">{{curVar.key}}:{{curVar.value}}</view>
              <view class="icon"><image src="../../asset/images/dropArrow.png" mode='aspectFit'/></view>
            </view>
            <view class="drop-box">
              <view class="drop-menuItem" wx:for="{{varibles}}" hover-class='drop-item-hover' hover-stay-time='0' data-info='{{item}}' bindtap='historyChoose' bind:longpress="historyInput">{{item.key}}:{{item.value}}</view>
            </view>
          </view>
          <view class="valSetBar">
            <view class="valSetBtn" bind:tap="valsetbtn" hover-class='valSetBtn-hover' hover-stay-time='0' ><view class="text">赋值</view></view>
          </view>
        </view>
    </view>
    <view class="keyboard">
      <!-- 函数 -->
      <movable-area>
        <movable-view damping="{{100}}" inertia='{{true}}' friction="1" class="mv" direction="horizontal" bindtouchend="autoMv" bindchange="moving" x="{{mvOffset}}">
          <view class="grids" wx:for="{{fbtns}}">
            <view class="grid" wx:for="{{item}}" wx:for-item="sitem" data-info='{{sitem}}' bind:tap='button'><view hover-class='grid-hover' hover-stay-time='0'>{{sitem.text}}</view></view>
          </view>
        </movable-view>
      </movable-area>
      <!-- 键盘 -->
      <view class="grids" wx:for='{{nbtns}}'>
        <view class="grid" wx:for="{{item}}" wx:for-item="sitem" data-info='{{sitem}}' bind:tap='button'><view hover-class='grid-hover' hover-stay-time='0'>{{sitem.text}}</view></view>
      </view>
    </view>
  </view>
</view>

<sideMenu outClick='{{outClick}}'  curItemIdx='{{0}}'/>
<inputModel bind:param='valset' hide="{{valsetHide}}"></inputModel>


```

**index.wxss:**

```css
@font-face{
  font-family: consola;
  src:url("https://636c-cloud-gga5i-1303216693.tcb.qcloud.la/fonts/consola.ttf?sign=9fea6f5a9f7e9e27213d6d384a01176e&t=1614311663")
}
.calculator{
  width: 100vw; height: 100vh;
  position: relative;
  z-index: 0;
  transition-property: all;
  transition: 150ms;
  transition-timing-function: ease-in-out;
}
.bg{
  position: absolute;
  top: 0; bottom: 0;left: 0; right: 0;
  width: 100vw; height: 100vh;
  filter: blur(40rpx);
  z-index: 1;
}
.mask{
  position: absolute;
  top: 0; bottom: 0;left: 0; right: 0;
  width: 100vw; height: 100vh;
  background: #c1c1c1;
  opacity: 0.7;
  z-index: 2;
}
.body{
  position: absolute;
  width: 100vw; height: 100vh;
  display: flex; flex-direction: column;
  z-index: 3;
}

.nav{
  width: 100vw; height: 6vh;
  display: flex;
}
.nav>.icon{
  width: 6vh; height: 6vh;
  position: relative;
  content: '';
}
.icon-hover{
  background-color: rgba(172, 172, 172, 0.7);
}
.nav>.text{
  width: 15%; height: 6vh;
  font-weight: bold;
  font-size: 40rpx;
  line-height: 6vh;
  text-align: center;
}

.screen{
  width: 100vw; height: 15vh;
  display: flex; flex-direction: column;
  align-items: center;
}

.screen>.answer{
  width: 90vw; height: 5vh;
  font-size: 40rpx; color: #5c5c5c;
  line-height: 5vh;
  text-align: right;
  font-family: consola;
}
.screen>.expression{
  width: 90vw; height: 10vh;
  font-size: 60rpx; font-weight: bold;
  line-height: 10vh;
  text-align: right;
}
.input-scroll{
  width: 90vw; height: 10vh;
  position: relative; display: flex; justify-content: flex-end;
}
.input-cbox{
  width: max-content; height: 10vh;
  position: relative; z-index: 3;
  overflow: visible;
}
.input-content{
  height: 10vh; right: 0;
  /* position: absolute; z-index: 4; */
  font-size: 30px; font-weight: bold; text-align: right; line-height: 10vh;
  font-family: consola;
  white-space: nowrap;
}
.input-cursor_layer{
  width: 1.5px; height: 5vh; top: 2.5vh;
  background: #666;
  position: absolute; z-index: 2;
  animation: twinkling 1s infinite ; 
}
@-webkit-keyframes twinkling{ 
  0%  { opacity: 0;} 
  100%{ opacity: 1;} 
} 
.toolbar{
  width: 100vw; height: 12vw;
  margin-top: 1vw; margin-bottom: 1vw;
  display: flex;
  position: relative; z-index: 3;
}
.toolbar>.container{
  position: relative;
  z-index: 4;
}
.container>.back{
  background: #ddd7d7; opacity: 0.5;
  position: absolute; top: 0; right: 0; bottom: 0; left: 0;
  z-index: 0;
}
.drop-menu{
  width: 35vw; height: 12vw;
  position: absolute; z-index: 2;
  margin-top: .5vw; margin-bottom: .5vw; margin-left: 2vw;
  display: flex; flex-direction: column;
}
.drop-btn{
  width: 33vw; height: 11vw;
  position: relative; z-index: 4;
  display: flex;
}
.drop-btn>.text{
  width: 23vw; height: 11vw;
  margin-left: 2vw;
  line-height: 11vw; text-align: left;
  overflow: hidden;
  text-overflow: clip;
  white-space: nowrap; 
  font-weight: bold;
  font-family: consola;
}
.drop-btn>.icon{
  width: 8vw; height: 11vw;
  position: relative;
}
.drop-btn>.icon image{
  width: 4vw; height: 4vw;
  position: absolute;
  top: 3.5vw; left: 2vw;
}
.drop-btn-hover{
  width: 34vw;
  border: .5rpx solid #636160;
}
.drop-box{
  width: 34vw; height: auto;
  z-index: 4;
  background: #c2c4c4;
  margin-top: 10rpx;
  padding: .5vw;
}
.drop-menuItem{
  width: 32vw-20rpx; height: 11vw;
  margin: .5vw; padding-left: 20rpx;
  font-size: 30rpx; line-height: 11vw; text-align: left; font-weight: bold;
  background: #edeeee;
  overflow: hidden;
  text-overflow: ellipsis;
}
.drop-item-hover{
  background: #d2d2d2;
}
.valSetBar{
  width: 25vw; height: 12vw; right: .5vw;
  position: absolute; z-index: 2;
  margin-top: .5vw; margin-bottom: .5vw; margin-right: .5vw;
  display: flex; flex-direction: column;
}
.valSetBar>.valSetBtn{
  width: 23vw; height: 11vw;
  position: relative; z-index: 4;
  display: flex; justify-content: center;
}
.valSetBtn>.text{
  width: 23vw; height: 11vw;
  line-height: 11vw; text-align: center; font-size: large;
  overflow: hidden;
  text-overflow: clip;
  white-space: nowrap; 
  font-weight: bold;
  font-family: consola;
}
.valSetBtn-hover{
  width: 22vw;
  border: 1rpx solid #636160;
}
.keyboard{
  width: 100vw; height: auto;
  position: relative; z-index: 1;
}
movable-area{
  width: 100vw; height: 40vw;
}
movable-area movable-view{
  width: 200vw; height: auto;
  display: flex; flex-direction: column;
}
.grids{
  display: flex;
}
.grid{
  width:20vw; height:20vw;
  align-items: center; justify-content: center;
}
.grid>view{
  width: 19.5vw; height: 19.5vw;
  background:#fafafa; opacity: 0.5;
  line-height: 19.5vw; text-align: center;
  font-weight: bold; font-size: 30rpx;
}
.grid-hover{
  background: #000; 
  border: .5rpx solid #636160;
}

```

- #### Display

  <video src="20231008_165012.mp4"></video>

- #### Summary

This is the first time to contact the small program, learned a lot of new knowledge (wechat small program structure, user interaction, interface Settings, etc.). However, the function of this calculator is not very perfect at present, and we hope to further improve it in the future to achieve functions such as trigonometric functions.