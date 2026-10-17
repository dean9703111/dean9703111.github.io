# Redmine Plugins & Themes 介紹：那些我成功安裝的外掛

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202103/redmine-plugins-and-themes/
- 發布：2021-03-02
- 主題：團隊合作
- 系列：Redmine 專案管理
- 標籤：Redmine、Plugins、Themes、啟用模板方式、安裝套件
- 摘要：Redmine 的擴充性相當強，但是擴充性強是一回事，你能不能成功安裝是另外一回事；Redmine 版本不斷更新的同時，有許多的 plugins 也成為時代的眼淚，今天就來分享幾個我成功擴充並且實用的幾個 plugins & Theme。

Redmine 的擴充性相當強，但是擴充性強是一回事，你能不能成功安裝是另外一回事；Redmine 版本不斷更新的同時，有許多的 plugins 也成為時代的眼淚，這篇文章將分享幾個我成功擴充並且實用的幾個 plugins & Theme。

  這篇文章會介紹上面成功安裝的 plugins

```
一、Redmine Dashboard plugin
二、Redmine Checklists plugin (Light version)
三、Redmine Issues Tree plugin
四、Redmine Issue Templates plugin
五、Redmine Issue View Columns
六、Redmine Lightbox 2
七、Redmine Mentions
八、Redmine A1 theme
九、備註

```

### 提供安裝套件時的參數給大家參考：

> rvm version：1.29.12
> ruby version：2.6.6
> Redmine version：4.1.1
> 這篇文章是介紹套件為主，如果你想知道怎麼安裝可以[參考『Redmine 超詳細安裝教學』的 STEP 9](https://deanlin.net/blog/202102/redmine-installation-ubuntu/)

## 一、Redmine Dashboard plugin

> Resource：[https://github.com/jgraichen/redmine_dashboard](https://github.com/jgraichen/redmine_dashboard)
> 推薦程度：⭐⭐⭐⭐⭐

這是一款必須安裝的套件！透過 **看板的呈現 讓**使用者更能全盤掌握專案的進度、利用** Filter 條件**篩選出想要的議題、直覺的 **拖拉議題** 改變狀態

  卡片版

  精簡版

  可以做進度更新、指派任務的動作

## 二、Redmine Checklists plugin (Light version)

> Resource：[https://www.redmineup.com/pages/plugins/checklists](https://www.redmineup.com/pages/plugins/checklists)
> 推薦程度：⭐⭐⭐⭐⭐

有時一個議題內容比較複雜的時候，除了新增子議題外，你也可以用 Checklist 的方式來提醒自己有哪些任務需要完成。

另外**對於測試人員來說這個 Checklist 的功能是非常有幫助的**，可以用來幫助他們紀錄已經測試過的項目及更精準的追蹤測試進度。

**備註**：套件可設定除非 **Checklist 全部打勾 **才能關閉議題

## 三、Redmine Issues Tree plugin

> Resource：[https://github.com/Loriowar/redmine_issues_tree](https://github.com/Loriowar/redmine_issues_tree)
> 推薦程度：⭐⭐⭐⭐

這個套件在一個專案議題數量很多的時候幫助很大，透過樹狀圖的顯示每個議題之間的關係讓使用者找到所需資源

**Before**：在系統預設的畫面會顯得有點混亂

**After**：樹狀圖檢視降低閱讀壓力

## 四、Redmine Issue Templates plugin

> Resource：[https://github.com/Loriowar/redmine_issues_tree](https://github.com/agileware-jp/redmine_issue_templates)
> 推薦程度：⭐⭐⭐⭐⭐

專案進行時最害怕開議題時每個人的風格都不一樣，並且對於議題的描述不夠精準忘東忘西；本套件就是為了減少這部分的溝通成本，**讓每個參與者都必須依照特定格式來新增議題/筆記**。

  管理者可以決定問題樣板的**作用範圍**

  使用者可以**依照專案**去新增樣板，並設定是否要成為**預設值**

  使用者可以去調整樣板的**優先順序**

  這樣新增議題時便會有**統一的格式**

## 五、Redmine Issue View Columns

> Resource：[https://github.com/kenan3008/redmine_issue_view_columns](https://github.com/kenan3008/redmine_issue_view_columns)
> 推薦程度：⭐⭐⭐⭐

這個套件是設定議題下面**子議題要呈現哪些欄位**的 Plugin

**Before**：原本呈現的資訊不符合使用者的實際需求，且有些欄位不清楚作用

**After**：自訂想要呈現的欄位，欄位功能清晰

## 六、Redmine Lightbox 2

> Resource：[https://github.com/paginagmbh/redmine_lightbox2](https://github.com/paginagmbh/redmine_lightbox2)
> 推薦程度：⭐⭐⭐

有時議題會附加一些照片、PDF 做為參考資料，原本預設這些附加檔案都需要下載或是開新分頁才能閱讀，安裝這個外掛後就能透過**預覽檢視照片、PDF**

## 七、Redmine Mentions

> Resource：[https://github.com/arkhitech/redmine_mentions](https://github.com/arkhitech/redmine_mentions)
> 推薦程度：⭐⭐⭐⭐⭐

我們在**撰寫議題描述/筆記**時常常需要提到某些人(會議決策者、功能開發人員…)，讓這些人了解目前議題執行的進度，或是紀錄他們說過的話。

加入這個套件之後可以透過 `@someone_name` 的方式標記對方

如果希望被標記時也會收到信件，要調整帳號設定如下

  會收到長這個樣子的信件

## 八、Redmine A1 theme

> Resource：[https://www.redmineup.com/pages/zh/themes](https://www.redmineup.com/pages/zh/themes)
> 推薦程度：⭐⭐⭐⭐⭐

如果覺得系統預設的主題(Theme)太醜，可以到[這個網站](https://www.redmineup.com/pages/zh/themes)上找自己喜歡的主題

我這次選擇的是 [A1主題](https://www.redmineup.com/pages/themes/a1) ，如果要**更換主題需要由管理者做設定喔**！

## 九、備註

今天所操作的這些 plugins **有些是默認全域作用、有些是針對專案作用**，如果你發現 Plugin 安裝成功但是沒看到功能時請依照下面的步驟打開 Plugin：

### STEP 1：進入專案清單

### STEP 2：選擇你要設定的專案

### STEP 3：設定專案

### STEP 4：滑到下面選擇啟用模組並儲存

```
以下是我整理 Redmine 的相關資源，想更進一步了解的朋友可以參考看看：
👍 Redmine 超詳細安裝教學：Redmine 4.1.1 on Ubuntu 16.04
👍 如何用 Redmine 做專案管理＆體驗心得
👍 用 Redmine 做專案管理 — 管理者不可不知的設定
👍 用 Redmine 做專案管理 — 一般用戶不可不知的基礎功能
👍 Redmine 教育訓練 — 依角色權限說明

```

### 🙂 讀完這篇文章後

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202103/redmine-plugins-and-themes/
