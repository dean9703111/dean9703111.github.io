# Vue的基本常識

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202007/vue-basics/
- 發布：2020-07-10
- 主題：技術教學
- 標籤：Vue.js、Vuex
- 摘要：雖然用了一段時間的Vue，但自認對他的了解還不夠深刻，於是讀了一些大神文章，依照自己的理解做了整理筆記，希望大家都能吸收這些基本常識。

雖然用了一段時間的Vue，但自認對他的了解還不夠深刻，於是讀了一些大神文章，依照自己的理解做了整理筆記，希望大家都能吸收這些基本常識。

## SPA，single page application簡易說明

> 他會在一開始init web page時就把html, js, css這些需要的load完，因此在換頁時只需要load新頁面所需要的元件，基本上是用router-link的方式來完成換頁功能

### 優點：

- 使用者體驗良好，省去每次刷新頁面的等待，大幅減少loading相同元素的耗能
- 基於上一個原因，server的壓力也減少許多
- 前後端分離，前端專心處理渲染，後端專心處理數據

### 缺點：

- 第一次loading時間較長
- 因為是單頁瀏覽，所以不支援上一頁/下一頁功能
- SEO難度高，因為所有內容都在同一頁

## v-if與v-show的區別

- v-if是根據條件是否成立來決定渲染/銷毀
- v-show是無條件渲染，他就是一個css的display的功能

> 如果你使用場景偶爾才切換條件就使用v-if，可以減少資源浪費；但如果需要高頻率切換就使用v-show吧

## Vue parent component , child component特性

> Parent component可以透過props傳遞到child component，而child component如果想要傳遞變更事件及資料就需要用$emit來觸發parent的事件

如果你想要在child 變更props，請這樣操作：

這個props傳入一個initial value，child component會希望可以操作這個init value，如果想要做這個操作，你需要在child component裡面新增data的屬性，並給予props的initial value

```
props: ['initialValue'],
data: function () {
    return {
        counter: this.initialValue
    }
}

```

這個props傳入一個origin value，child component想要對他做一個換算或是變更，這樣建議用computed

```
props: ['size'],
computed: {
    normalizedSize: function () {
        return this.size.trim().toLowerCase()
    }
}

```

## Computed VS Watch

- Computed是用來計算的，有緩存的特性，只有裡面依賴的參數調整時才會觸發，且一定有計算完的回傳值
- Watch主要是觀察數據變更，比較常用來做一些消耗相對多resourcec或是非同步運算的事情(ex:call api)

## 給object array新的value，vue要如何偵測

> 因為javascript限制，下面兩個範例是vue無法偵測的，需要進行修正：

```
//vm.items[indexOfItem] = newValue
vm.$set(vm.items, indexOfItem, newValue)
//vm.items.length = newLength
vm.items.splice(newLength)

```

## Vue的生命週期

> Component初始化->屬性綁定&編譯templete->掛載dom->渲染更新…->銷毀

## Parent component & Child component life cycle

> 基本原則就是父親開始動作後兒子才能動作，兒子動作完畢後，父親才會正式停下來

### Component mounted

- 父 beforeCreate -> 父 created -> 父 beforeMount -> 子 beforeCreate -> 子 created -> 子 beforeMount -> 子 mounted -> 父 mounted

### Child component update

- 父 beforeUpdate -> 子 beforeUpdate -> 子 updated -> 父 updated

### Parent component update

- 父 beforeUpdate -> 父 updated

### Destroyed

- 父 beforeDestroy -> 子 beforeDestroy -> 子 destroyed -> 父 destroyed

## Vue的生命週期中可以做異步請求

> created、beforeMounted、mounted都可以，因為data已經存在，可以去做相關的操作，但建議在created階段操作：

- 可更早取得server端數據，減少頁面loading時間
- ssr不支援beforeMounted、mounted，放在created可保持程式邏輯一致性(如果前後端分離通常是在client端做渲染較不會有這個問題)

## 什麼階段可以對DOM操作

> 在mounted階段可以對DOM操作，這個階段已經完成整個頁面的component了

## Parent component如何監聽Child component的生命週期

1. 用$emit的方式

```
// Parent.vue
<Child @mounted=”doSomething”/>
// Child.vue
mounted() { this.$emit(“mounted”); }

```

2. 用@hook方式(較推薦)

```
// Parent.vue
<Child @hook:mounted=”doSomething” ></Child>
doSomething() {
console.log(‘父组件监听到 mounted 钩子函数 …’);
},
// Child.vue
mounted(){
console.log(‘子组件触发 mounted 钩子函数 …’);
},

```

## Keep-alive的作用

> keep-alive是 Vue 的原生元件，專門做為 Cache 的解決方案，避免頁面中的組件重新被渲染。[詳細可參考](https://medium.com/%E4%B8%80%E5%80%8B%E5%B0%8F%E5%B0%8F%E5%B7%A5%E7%A8%8B%E5%B8%AB%E7%9A%84%E9%9A%A8%E6%89%8B%E7%AD%86%E8%A8%98/vue-%E8%81%B0%E6%98%8E%E4%BD%BF%E7%94%A8-keep-alive-%E9%81%94%E5%88%B0-cache-%E7%9B%AE%E7%9A%84-691cfb3c8bf0)

## 為什麼component中的data是一個函數，然後return一個object，而new Vue卻可以直接是一個object

> 因為component是要用來重複使用的，如果data是一個object這樣component之間會資料錯亂，而當他是一個函數的時候就可以完成區隔
> 而new Vue則不會重複使用，所以沒有被引用的問題

```
// data
data() {
    return {
        message: “子组件”,
        childName:this.name
    }
}
// new Vue
new Vue({
    el: ‘#app’,
    router,
    template: ‘<App/>’,
    components: {App}
})

```

## v-model原理

> v-model主要是在input, select, textarea這些元素中建立雙向綁定的工具，他會在不同元素中拋出不一樣的事件：

- text 和 textarea 元素使用 value 屬性和 input 事件；
- checkbox 和 radio 使用 checked 屬性和 change 事件；
- select 字段将 value 作為 prop 並將 change 作為事件。

## Vue component間communicate 方式

- props / $emit 父子component通信
- [ref 與 $parent / $children](https://blog.csdn.net/zhouzuoluo/article/details/81097339) 父子component通信
- EventBus ($emit / $on) 你可以把它當成全域的事件，只要有監聽便可接收，但請在destory該component前也要把監聽事件destory掉
- [EventBus 事件總線](https://medium.com/@bebebobohaha/vue-eventbus-%E4%BA%8B%E4%BB%B6%E7%B8%BD%E7%B7%9A-9673e04f6a5f)
- Vuex，你可以把他想像成一個網站暫存的store，你可以更改及存取state

## Vuex 概念

> 核心為store，用來存取state；可使用mutation提交的方式來過改變state資料

## SSR優缺點

### 優點：

- 好的SEO，因為網頁已經再server side組裝完成，所以搜尋引擎能直接抓到渲染好的頁面
- 首頁加載快速，因為都由server side轉換完成了

### 缺點：

- 更多的開發條件限制：像是life cycle渲染階段只能用beforeCreate、created
- server負擔更重：因為都是server side作轉換，所以負擔更重，cache的設計更加顯得重要

## vue-router種類

> hash, history, abstract(default)

## MVVM是什麼

> 由Model-View-ViewModel組成

- Model: 由後端接口api return 回來的資料(ex: json data)

```
{ “url”: “/your/server/data/api”, “res”: { “success”: true, “name”: “IoveC”, “domain”: “www.cnblogs.com" } }

```

- View: 前端UI介面由HTML/CSS構建

```
<div id=”app”>
<p>{{message}}</p>
<button v-on:click=”showMessage()”>Click me</button>
</div>

```

- ViewModel: 前端開發人員根據api回傳的資料做二次封裝提供給View使用

```
var app = new Vue({
el: ‘#app’,
data: { // 用于描述视图状态
    message: ‘Hello Vue!’,
},
methods: { // 用于描述视图行为
    showMessage(){
        let vm = this;
        alert(vm.message);
    }
},
created(){
    let vm = this;
    // Ajax 获取 Model 层的数据
    ajax({
        url: ‘/your/server/data/api’,
        success(res){
            vm.message = res;
        }
    });
}})

```

## Virtual DOM優缺點

### 優點：

- 保證基礎效能
- 無須手動去調整DOM，增加開發效率
- 跨平台，如果SSR，WEEX

### 缺點：

- 無法極致優化，畢竟這些都是自動產生的

## Vue中Key的作用

> key是Vue中node節點的唯一值，透過他diff操作可更快速準確

## Vue可考慮優化項目

### coding部分

- v-if/v-show使用情境區分
- computed/watch使用情境區分
- v-for記得要有key，避免使用v-if(這部分應該交由server端就完成data的filter)
- 監聽的事件在component destroy時是否有銷毀
- Image lazy loading
- 第三方插件依照需求引入

### Webpack部分

- Webpack對圖片進行壓縮
- 共用component規劃
- 共用css設計

### web部分

- gzip
- CND
- Browser cache

參考資源：[https://juejin.im/post/5d59f2a451882549be53b170#comment](https://juejin.im/post/5d59f2a451882549be53b170#comment)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202007/vue-basics/
