# 微信小程序wepy框架入门教程-底部导航栏效果（四）

### 文档

*   [wepy快速指南](https://tencent.github.io/wepy/document.html)
*   [小程序框架wepy开发文档](https://www.w3cschool.cn/qdwzc/)
*   [wepy开源](https://github.com/Tencent/wepy)
*   [wepy官方文档](https://tencent.github.io/wepy/)
* * *
wepy是腾讯自己开发的框架，作用主要是提高开发者的开发效率，采用了类似使用了vue的代码书写风格，开发者可以熟练的上手小程序开发。


先上效果图
![](https://upload-images.jianshu.io/upload_images/5640239-9d960f29485c4a81.gif?imageMogr2/auto-orient/strip)

1：新建四个界面
在components
底下新建四个wepy文件
分别是
message 消息列表界面
```
<template>
  <view class="me">
   消息列表界面
  </view>
</template>
<script>
  import wepy from 'wepy';
  export default class Me extends wepy.component {
    components = {
    }
    methods = {
    };
  }
</script>
```
contact联系人界面
```
<template>
  <view class="me">
    联系人界面
  </view>
</template>
<script>
  import wepy from 'wepy';
  export default class Me extends wepy.component {
    components = {
    }
    methods = {
    };
  }
</script>

```
discovery发现界面
```
<template>
  <view class="me">
   发现界面
  </view>
</template>
<script>
  import wepy from 'wepy';
  export default class Me extends wepy.component {
    components = {
    }
    methods = {
    };
  }
</script>

```
me我的主页
```
<template>
  <view class="me">
   我的主页
  </view>
</template>
<script>
  import wepy from 'wepy';
  export default class Me extends wepy.component {
    components = {
    }
    methods = {
    };
  }
</script>

```
![](https://upload-images.jianshu.io/upload_images/5640239-74dd14292544d110.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


2:编写index界面代码
```
<style type="scss">
  .tab_item {
    height: 100%;
  }
  page, .body{
    height: 100%;
    font-family: '\5FAE\8F6F\96C5\9ED1', arial;
    background-color: #f0eff5;
  }
</style>
<template>
  <view class="body">
    <view class="tab_item tab_message" hidden="{{currentTab != 0}}">
      <message />
    </view>
    <view class="tab_item tab_contact" hidden="{{currentTab != 1}}">
      <contact />
    </view>
    <view class="tab_item tab_discovery" hidden="{{currentTab != 2}}">
      <discovery />
    </view>
    <view class="tab_item tab_me" hidden="{{currentTab != 3}}">
      <me />
    </view>

    <tab :active.sync="currentTab" />
    <toast />
  </view>
</template>

<script>
  import wepy from 'wepy';
  import Message from '../components/message';
  import Discovery from '../components/discovery';
  import Contact from '../components/contact';
  import Me from '../components/me';
  import Tab from '../components/tab';
  import Toast from 'wepy-com-toast';

  export default class Index extends wepy.page {
    config = {
      'navigationBarTitleText': 'wepy - 微信',
      'navigationBarTextStyle': 'white',
      'navigationBarBackgroundColor': '#3b3a40'
    };

    components = {
      message: Message,
      discovery: Discovery,
      me: Me,
      contact: Contact,
      tab: Tab,
      toast: Toast
    };

    data = {
      currentTab: 0
    };

    methods = {
    };

    onShow() {
      this.currentTab = 0;
      this.$invoke('message', 'loadMessage');
    }

    showToast(name) {
      let promise = this.$invoke('toast', 'show', {
        title: `${name}`,
        img: 'https://raw.githubusercontent.com/kiinlam/wetoast/master/images/star.png'
      });

      promise.then((d) => {
        console.log('toast done');
      });
    }
  }
</script>

```
![](https://upload-images.jianshu.io/upload_images/5640239-11c292cdd60ea691.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
3：新建images文件夹，准备图标
![](https://upload-images.jianshu.io/upload_images/5640239-78f3312a6676f4cc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
4:在components底下新建tab.wpy
编写代码
```
<style type="scss">
  $fontcolor: #7b7b7b;
  $activecolor: #13b113;
  .tab {
    color: $fontcolor;
    position: fixed;
    bottom: 0;
    height: 100rpx;
    width: 100%;
    border-top: 1px solid #dad9d6;
    background-color: #f7f7f7;
    font-size: 24rpx;
    white-space: nowrap;
    .tab_item {
      &.active {
        color: $activecolor;
      }
      display: inline-block;
      width: 25%;
      text-align: center;
    }
    .icon {
      width: 60rpx;
      height: 60rpx;
      display: block;
      margin: auto;
    }
    .title {
      padding-top: 6rpx;
      display: block;
    }
  }
</style>
<template>
  <view class="tab">
    <view class="tab_item tab_message{{active == 0 ? ' active' : ''}}" @tap="change(0)">
      <image class="icon" src="../images/message{{active == 0 ? '_active' : ''}}.png"></image>
      <text class="title">微信</text>
    </view>
    <view class="tab_item tab_contact{{active == 1 ? ' active' : ''}}" @tap="change(1)">
      <image class="icon" src="../images/contact{{active == 1 ? '_active' : ''}}.png"></image>
      <text class="title">通讯录</text>
    </view>
    <view class="tab_item tab_discovery{{active == 2 ? ' active' : ''}}" @tap="change(2)">
      <image class="icon" src="../images/discovery{{active == 2 ? '_active' : ''}}.png"></image>
      <text class="title">发现</text>
    </view>
    <view class="tab_item tab_me{{active == 3 ? ' active' : ''}}" @tap="change(3)">
      <image class="icon" src="../images/me{{active == 3 ? '_active' : ''}}.png"></image>
      <text class="title">我</text>
    </view>
  </view>
</template>
<script>
  import wepy from 'wepy';
  export default class Tab extends wepy.component {
    props = {
      active: {
        twoWay: true
      }
    };
    data = {
    };
    methods = {
      change (idx, evt) {
        this.active = +idx;
      }
    };
    onLoad () {
    }
  }
</script>

```
![](https://upload-images.jianshu.io/upload_images/5640239-88af4e4ff56f0cf3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

5：一切准备就绪，编译
![](https://upload-images.jianshu.io/upload_images/5640239-29920994830bbf0c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

6：打开微信开发者工具，查看效果（开篇已经给出动态图效果）
![](https://upload-images.jianshu.io/upload_images/5640239-fb1137a051bd90bf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)













