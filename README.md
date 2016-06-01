# vue-drapload

这是基于vue的一个下拉刷新和加载更多的插件。


# 安装//Install

```npm
npm install vue-drapload --save
```

###ES6

```JavaScript
import vueDrapload from 'vue-drapload'
Vue.use(vueDrapload)
```

###CommonJS

```JavaScript
var vueDrapload =  require('vue-drapload');
Vue.use(vueDrapload)
```

###直接引用//Direct include

```JavaScript
<script src="../node_modules/vue_scroll/vue-drapload.js"></script>
```

# 使用方法//Usage

Use v-scroll to enable the infinite scroll, and use scroll-* attributes to define its options.

使用v-scroll进行滚动翻页,使用 scroll- * 属性来定义它的选项。

```HTML

<div class="app" v-scroll scroll-key="ascroll" scroll-initialize="true" scroll-down="down_a()" scroll-up="up_a()">
  <div>
    <div class="item" v-for="item in a">
      <h1 class="name">{{item.value}}</h1>
      <div class="desc">{{item.data.description}}</div>
      <div class="down">{{item.data.url}}</div>
      <div class="score">{{item.data.suggested_score}}</div>
    </div>
  </div>
</div>
```

```JavaScript
  var app = new Vue({
    el: 'body',
    data: function () {
      return { a: [], b: [] }
    },
    ready: function () {
      var me = this;
      me.$options.vue = me
    },
    /**
     * 加载数据
     * @param fn
     */
    loadListData: function (fn) {
      var me = this.vue;
      $.ajax({
        url: 'npm',
        data: {},
        type: 'GET',
        success: function (data) {
          fn(data.sections.packages)
        }
      });
    },
    methods: {
      down_a: function () {
        var me = this
        me.$options.loadListData(function (data) {
            me.a = me.a.concat(data)
            // 通过设置的key 方法下拉对象方法
            me.ascroll.resetload(true)
        });

      },
      up_a: function () {
        var me = this
        me.$options.loadListData(function (data) {
              me.a = data
              me.ascroll.resetload()
        });
      }
    }
  })
```




# 配置//Config

```JavaScript
  //现在使用默认配置。后面下拉刷新的各种状态会提供配置选项控制。
```

# 选项//Options

| scroll/Option | Description |
| ----- | ----- |
| scroll-key | 标准变量：该scroll唯一标示．// Number(default = 'scroll')： 你可以在vm对象中找到他 |
| scroll-up | 下拉刷新的时候回调该方法，存在该属性才会支持下拉刷新。否则不支持下拉刷新 |
| scroll-down |加载更多的时候调用该方法，存在该方法才会支持加载更多|
| scroll-initialize | 布尔(默认为false)：设定为true时将在页面加载完成后触发一次scroll-down 对应的方法．|

# 方法//Function

| scroll/Option | Description |
| ----- | ----- |
| resetload | 重置下拉刷新或者加载跟多状态。一般当你加载到数据后会调用该方法 |
| noData | 当加载更多数据（scroll-down对应方法）的时候没有数据后可以调用该方法。这时加载跟多组件状态会改为无数据状态 |

# License

MIT