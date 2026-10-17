# 5 分鐘帶你搞懂 Vue、Vuex、Vuetify、Nuxt.js 之間的關係

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202104/vue-vuex-vuetify-nuxt-explained/
- 發布：2021-04-01（更新：2023-05-05）
- 主題：技術教學
- 標籤：Vue.js、Vuex、Vuetify、Nuxt.js、前端框架
- 摘要：有了框架後做什麼都很方便，但因為太方便導致有時連工具負責哪一塊都功能有點霧颯颯，如果你搞不太懂 Vue、Veux、Vuetify、Nuxt.js 之間的關係、具體各自負責什麼事情的話可以參考這篇文章釐清自己的觀念喔！

有了框架後做什麼都很方便，但因為太方便導致有時連工具負責哪一塊都功能有點霧颯颯，如果你搞不太懂 Vue、Veux、Vuetify、Nuxt.js 之間的關係、具體各自負責什麼事情的話可以參考這篇文章釐清自己的觀念喔！

```
Vue.js
➤ 過去只有 Javascript、HTML、CSS 時有什麼問題？
➤ 框架的優勢
Vuex
➤ Vue過去傳值時面臨什麼問題？
➤ 如何解決這個問題？
Vuetify
➤ 提供前端工程師豐富的 MD component
➤ 強大的社群支持並穩定更新
➤ 自由決定導入 component
Nuxt.js
➤ Vue.js 的應用框架
➤ SSR 伺服器端渲染

```

## Vue.js

一個優秀的 Javascript 框架

### 過去只有 Javascript、HTML、CSS 時有什麼問題？

- **難以維護**：專案結構龐大時 Javascript、HTML、CSS 維護難度會成幾何式上升，各式各樣的邏輯都集中在 Javascript 裡面導致程式可讀性極低
- **效能低落**：如果 Javascript 頻繁操作 DOM 元素時會不斷觸發 Reflow & Repaint 的流程，頁面重複渲染的過程會消耗大量效能

### 框架的優勢

- **資料與 UI 分離**：過去 Javascript 要動態的去更新 DOM 來塞入資料，但框架透過資料雙向綁定，工程師可以專注處理資料的邏輯面
- **模組化的 UI**：一個網站會有許多重複出現的元素(按鈕、表單、表格…)，在框架中可以這些常用的元素設計成 component，要使用時再根據實際需求填入資料及參數即可，大幅提升工程師理解與維護的能力。
- **更好的效能**：採取 **Virtual DOM** 概念，當資料變動時先計算好要變動的地方，以此抵銷無意義的變更，並且重複使用已存在的 DOM 元素

## Vuex

### Vue過去傳值時面臨什麼問題？

過去 Vue 裡面 component 的參數傳遞都是採用 `emit`跟 `props` ，但如果 component 的關係並非 parent/child 而是像下圖想要將 Component B 的資訊傳到 Component C ，這種操作不但需要很多 `emit`跟 `props` 還會造成程式碼閱讀困難

### 如何解決這個問題？

根據上面的問題，官方推出專門用來『狀態管理』的工具，你可以把它想像成 Vue 裡面的全域變數，將狀態(state)全部放在倉庫(store)來進行管理。

**更新 state**：在 **Vuex** 裡面，儲存狀態的為 **State**，component 需要更動 **State **時，需要透過 **Actions **發出一個 **Commit** 去呼叫 **Mutations**，再由 **Mutations **去更改 **State**。

**取得 state**：而 component 的資料更新可以在 **compute** 裡面使用 **mapGetters** 使用 **getters** 的 function 或是用 **mapState** 直接取用 state 的資料

## Vuetify

### 提供前端工程師豐富的 MD component

你可以把 Vuetify 當成 Vue 的 Material Design framework，他不但省去了你自己手工打造 UI component 的時間，還為每個 component 提供許多可以自由定義的 property 讓版型更加靈活。

### 強大的社群支持並穩定更新

截至今天在 Github 上有高達 30K 的星星，如果你在開發上遇到問題在他們 [discord社區](https://discord.com/invite/s93b7Fv)上發問也能很快得到解答。

官方每個禮拜都會穩定更新版本，相比於其他 Vue 的 framework 是非常穩定的

  image from:[https://vuetifyjs.com/en/introduction/why-vuetify/#why-vuetify3f](https://vuetifyjs.com/en/introduction/why-vuetify/#why-vuetify3f)

### 自由決定導入 component

官方稱為 **Auto treeshaking**，在使用 Vuetify 時你可能覺得他非常『肥』，裡面包了一堆你專案中用不到 component，但因為有 **Auto treeshaking **的機制所以只會導入你有使用的 component

## Nuxt.js

### Vue.js 的應用框架

把 webpack、vue loader、vuex、router 的設定都集中在一起，減少使用者的配置成本

### SSR 伺服器端渲染

因為由 server 端組成 HTML 後回傳，所以 Google 在 SEO 解析時可以很好的分析 DOM 元素，藉此提升網站的 SEO 水平

Nuxt.js 也是能做 SPA(無SSR)、產生靜態網頁

**參考資源**： [Vuex 是什麼? 怎麼用?](https://medium.com/itsems-frontend/vue-vuex1-state-mutations-364163b3acac)、[前端框架的功能與優點](https://tw.alphacamp.co/blog/why-use-front-end-framework)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202104/vue-vuex-vuetify-nuxt-explained/
