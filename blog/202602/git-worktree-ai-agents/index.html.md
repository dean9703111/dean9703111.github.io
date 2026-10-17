# 【Git Worktree 實戰】指揮 AI Agents 同時運作，搭配 Agent Skills 建立自動化協作體系

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202602/git-worktree-ai-agents/
- 發布：2026-02-24
- 主題：AI 實戰
- 標籤：Git Worktree、AI Agent、Agent Skills、Vibe Coding、Code Review
- 摘要：最近與一群工程師朋友聚餐時，我們聊到公司導入 AI 加速開發後，遇到的各種問題。

最近與一群工程師朋友聚餐時，我們聊到公司導入 AI 加速開發後，遇到的各種問題。

其中討論最熱烈的點，就是大家 **Code Review 的負擔大幅增加**，畢竟 AI 寫程式的速度，實在太快了。

**在過去，一天完成既定工作量就是好員工；但現在有了 AI 的幫助，有些公司會認為，一天要完成 3 倍工作量才算達到 KPI。**

而這，就衍生了一系列的問題：

### ① 缺乏深度思考

因為 KPI 大幅上升，大家為了追上進度，就會犧牲對需求的深度思考，或者說，直接讓 AI 代替思考。

### ② 懶得 Code Review

完成的功能增加，就會導致功能合併到主線時的 Code Review 負擔，以往會認真思考別人的設計是否合理，但如果要看的份量太多，很多人就放棄理解直接合併。

### ③ 最終導致的結果

團隊集體責任感下降、專案理解力降低、程式碼品質降低、上線後錯誤增加。

切記，我們雖然可以讓 AI 自己檢查、修改、寫測試程式，但這只是確保產品的底線。

假使用 AI 開發過中大型專案，就會理解全權交給 AI 有多麼的不切實際。

**AI 產生的程式碼很多，但這不是你不檢查的原因，我們應該去想怎麼利用 AI 去優化檢查流程。**

別忘記，當你對 AI 產出的內容，按下同意的那一刻，所有的錯誤必須由人類來承擔。

所以今天這篇文章，我要跟大家分享如何用 Git Worktree 搭配 Agent Skills，來解決前面提到的痛點：

### ① 爲什麼版本控制很重要

我會先展示一個經典的錯誤案例，讓大家更直觀的理解，如果沒有基礎的版本控制概念，專案會遭遇哪些災難。

### ② 完成前置作業

接著帶大家了解 Git Worktree 基礎操作，並安裝 pnpm 減少磁碟空間浪費，以及分享有哪些好用的擴充套件能增加我們操作效率。

### ③ Agent Skills：透過 Git Worktree 拆分任務、設計 SPEC

然後就要進入實作階段，這邊會先提出一個複雜功能，以此觸發 Agent Skills 執行 Git Worktree 拆分任務，讓每個功能都有自己的工作區，這樣 AI Agent 就可以同時執行。

### ④ Agent Skills：拆分 Commit，讓邏輯可被追蹤

在確認功能符合預期後，我會請 AI 根據調整檔案分階段的 commit，讓邏輯在未來可以被追蹤。

### ⑤ 降低 Code Review 負擔

把修改推送上去後，我們要透過 Pull Request 將變更的內容合併到主分支，為了讓 Code Review 的人快速理解，我們可以請 AI 根據變更生成 PR 模板。

如果你是第一次接觸 Agent Skills，我過去有拍一隻影片分享安裝、使用、製作的技巧：[https://youtu.be/xe00zJEtuMo](https://youtu.be/xe00zJEtuMo)

**如果你剛接觸 Vibe Coding ，或是進入職場不久，相信這隻影片會讓你了解業界在導入 AI 開發時，遇到的實際狀況。**

完整的 Demo，可以觀看 YT 喔 👉

對 AI 應用感興趣的朋友，歡迎按下訂閱按鈕，未來我計畫分享一系列 AI 工具的應用教學。

[YouTube 影片：YouTube 影片](https://www.youtube.com/watch?v=1VuLTQidbPM)

## ① 缺乏版控概念，是災難的起點

現在輸入簡單的指令，AI 就可以根據需求完成功能，甚至一口氣完成 10 個功能都沒有問題。

從表面上來看，效率倍增，但實際上有非常多的隱患，這邊展示一個實際案例讓大家更能理解：

  參考 PR: [https://github.com/deancourse/git-worktree-demo/pull/1](https://github.com/deancourse/git-worktree-demo/pull/1)

在團隊合作時，如果想把自己的修改合併回主線，會先開一個 Pull Request，讓其他人幫你檢查程式碼、討論是否需要修改，確認沒問題後再合併。

以這個 Pull Request 舉例，標題寫著「新增 i18n 多語系、淺色深色主題設計、常見 QA 問答區…」等多個功能，感覺做了很多事。

但從專案管理的角度來看，其實每個功能都應該對應到不同的 feature branch，不要在一個分支中放多個功能。

把視角切換到「Files changed」，你會對我說的話更有體會，像這個 Pull Request 涵蓋了 18 個檔案，有近千多行的程式變更；從 Code Review 的角度來看，你根本無法了解變更的功能要對應到哪些檔案。

再來，我們看一下「Commits」的分頁，你會發現改了這麼多功能，竟然只有一個 commit，而且訊息也非常的簡單。

這會導致未來發生問題時，我們無法追溯當時的設計邏輯，完全沒有參考價值。

**其實這邊所展示的問題，在過去就已經存在了，但在使用 AI 加速開發後，問題被成倍放大。**

因此接下來要跟大家分享，如何透過 Git Worktree 與 Agent Skills 來解決這些痛點。

## ② 建立 Git Worktree 環境：Git + pnpm + Extensions

為了方便大家可以跟著影片操作，我在 GitHub 有上傳一個練習用的專案，歡迎大家路過按個 Star：

  [https://github.com/deancourse/git-worktree-demo](https://github.com/deancourse/git-worktree-demo)

這邊我建議大家點擊 fork，將專案複製到自己的帳號下：

然後點擊 Code，從裡面複製專案網址：

接著打開你熟悉的程式碼編輯器，這邊我以 Google 最近推出的 Antigravity 來示範，這是 Google 最新推出的 AI 程式碼編輯

目前登入 Google 帳號就可以使用，但如果付費升級到 Pro 會有更高的使用額度：

  [https://antigravity.google/](https://antigravity.google/)

但如果你熟悉的是 Cursor 或 Claude 等其他 AI 工具也沒關係，因為以 Agent Skills 的設定來說，只有路徑稍微不同而已。

好的，我們再把注意力轉回 Antigravity，點擊 `Clone Repository`，貼上剛剛複製的網址來下載專案：

接下來要先跟大家分享，安裝哪些工具可以更舒適的使用 Git Worktree 功能：

### ➤ 確認 Git 版本

首先，我們要確認電腦的 Git 版本。

提醒一下大家，因為這隻影片會使用到 Node.js 與 Git 等技術，如果你是第一次接觸，我有專門錄一隻影片分享詳細的操作步驟喔！

[https://youtu.be/5DEV_K0JauQ](https://youtu.be/5DEV_K0JauQ)

[安裝好 Git 後](https://git-scm.com/)，點擊上方的 Toggle Panel 開啟終端機，輸入指令 `git -v` 確認有正確安裝。

另外提醒安裝過的朋友，**如果你的版本低於 2.30，建議升級到最新版本，這樣 Git Worktree 功能會較為完整。**

### ➤ 安裝 pnpm

因為 Git Worktree 會建立多個工作目錄，而 Node.js 專案的 node_modules 動輒上百 MB。

如果用 `npm` 安裝會消耗大量硬碟資源，因此我推薦大家使用 `pnpm` 這款套件。

在終端機輸入 `npm install -g pnpm` 便可完成全局安裝。

pnpm 會將所有套件存放在電腦的一個共用儲存區，各專案的 node_modules 只是透過連結指向同一份套件。

雖然每個 Worktree 需要執行 pnpm install，但不會重新下載套件，非常適合搭配 Git Worktree 進行並行開發。

### ➤ 安裝 Extensions

然後我們把視角轉到左側選單，點擊「Extensions」的分頁，這邊推薦大家安裝兩款外掛。

第一款是「Git Worktree Manager」，他能讓你透過介面輕鬆的建立、切換、清理 Worktree。

第二款是「GitLens」，安裝好後，你把滑鼠移到對應的程式碼位置時，可以直接看到 Commit 的歷史。

不過這邊要提醒一下大家，這些外掛大部分是由第三方開發的，安裝前請自行評估安全性。

### ➤ 了解 Git Worktree 工作模式

做好前置作業後，我們先在終端機輸入 `pnpm install` 安裝套件。

安裝完成後，輸入 `pnpm run dev` 便可啟動專案，下面就會有一個「localhost:3000」的網址。

確認專案可以順利啟動後，我們回到 Antigravity，點擊左側剛剛安裝好的「Git Worktree Manager」，然後點擊這個資料夾的符號來新增 Worktree。

因為我們要從現在的主分支，切換到其他分支，所以選擇第一個「Create new branch」

至於分支的名稱就打 `test`，主要是先讓大家了解 Git Worktree 概念；至於目錄，使用系統自動生成的就好了，最後點擊「Create」，就會直接把工作區的內容複製一份過去。

此時，你就會看到 test 出現在 Worktree List 當中：

點擊後便會開啟一個新的工作區，打開終端機後，要在輸入 `pnpm install` 來安裝套件；**你會發現套件幾乎在瞬間安裝完畢，打開 node_modules 資料夾，你會發現有箭頭圖示「↩」。**

代表只是建立資料夾的連結，而非重新下載套件，這就是 pnpm 的運作方式，可以幫我們節省硬碟空間。

然後我們嘗試輸入 `pnpm run dev` 啟動專案，這次的網址變成了「localhost:3001」，點擊後，網頁也可以順利呈現。

這邊的示範，是想要讓大家了解**每個 Worktree 都是獨立的工作目錄，所以可以同時跑不同的 dev server 跟 port**。

當然，也代表我們可以**同時指揮多個 AI Agent 並行開發不同功能。**

也跟大家說明一下，如何把 Worktree 刪除；在刪除前，請先把剛剛啟動的專案關閉。

然後回到 Git Worktree Manager 的介面，點擊右鍵，在 Worktree Action 當中選擇 Remove Worktree，就可以移除了。

## ③ Agent Skill：自動建立分支與開發規格

前面有說到，不同的功能應該使用不同的 feature branch 來開發；但具體怎麼拆分功能，以及 branch 的名稱如何命名，我們可以直接設計一個 Agent Skill 來幫我們搞定。

如果你是 Fork 我的專案，在根目錄下方會有一個「.agent」的資料夾，我有放一些 Skills 讓大家跟著影片一起體驗：

像這邊我設計了一個 `git-worktree-design` 技能，把 Markdown 點開來後，跟大家說明一下設計的邏輯：

- 首先會檢查目前專案分支的狀況，確認既有 Worktree，以及是否有沒提交的變更。
- 然後將複雜的需求拆成獨立的 feature branch 以及參考的規格後，用表格預覽，使用者確認後才會執行。
- 接下來就會根據 Skill 指定的格式建立 Worktree。
- 並順便安裝好每個 Worktree 的套件。
- 然後新增這個 feature 要執行的 SPEC，方便後續 AI Agent 的開發。
- 完成以上任務後，會再用表格像我們展示完成的結果。

了解 Skill 的設計邏輯後，我們在右側與 AI 的對話視窗中，輸入指令

```
採用 Worktree，新增淺色深色主題設計、常見 QA 問答區、Cookie 同意彈窗

```

此時，你會發現 AI 判斷需要使用 Git Worktree 這個 Skill，並初步規劃了每個 Worktree 要參考的規格：

確認沒問題後，就可以讓 AI 幫我們執行下去，稍等一段時間後，AI 說他已經幫我們完成了對應的設計。

打開左側 Git Worktree Manager 的分頁，可以看到看到新增的 Worktree：

這邊點擊其中一個 Worktree，你可以看到 AI 有根據我們的指示，用 pnpm 來安裝套件，所以 node_modules 下面資料夾有這個箭頭符號。

並且根目錄的地方有存放給「git-Worktree-spec.md」這個給 AI Agent 參考的規格，確保執行的方向。

## ④ 指揮多個 AI Agents 同時執行

但讓 AI 參考規格開發前，跟大家分享一個小技巧。先開啟與 AI 對話的視窗，點擊右上方的「…」選擇 Customizations，選擇 Workflows：

我有設計一個自動化流程，這樣只要輸入快捷鍵，就可以讓 AI 知道讀取規格、執行實作、驗收結果的步驟：

這邊我們先點開其中一個 Worktree，輸入「/」，選擇剛剛的那個 workflow，點擊「Enter」他就會開始執行了：

以往我們開發功能因為在同一個工作區，所以需要一個完成後才能執行下一個。

但用 Git Worktree 的好處，就是可以多個 AI Agent 同時執行。這邊我也把另外兩個 Worktree 打開，輸入「/」讓他們根據 workflow 來執行。

但如果如果要在多個視窗切換，來觀察不同 AI Agent 執行的進度有點麻煩。

所以我們可以點擊上方的「Open Agent Manager」，這樣就能知道 Agent 執行的過程是否需要我們介入。

像這邊轉圈圈就是代表執行中，而稍等一段時間後，就會變成鈴鐺提醒的符號。

稍等一段時間後，AI 就在不同的工作區，完成了各自的功能，這邊我們也可以從 SPEC 的修改中，看到他已經自我驗證了功能符合需求。

但 AI 完成後，人類一定也要自行確認結果是否符合期待，這邊我先開啟各個 worktree 的瀏覽器。

把所有服務都啟動後，也剛好趁這個機會，跟大家說明不同功能分 feature 開發的好處

首先是我們人類驗證功能會比較輕鬆，因為要確認的面向單一

- 像 Cookie 的部分，就是下方在使用者進入時，會有彈窗提示

- 而淺色深色主題這塊，則是上方多了一個按鈕，讓使用者自由切換主題

- 至於常見問答，也有根據主題新增了對應的區塊

再確認修改符合預期後，先把 Agent Manager 關閉，我們要把 Worktree 的變更推送上去。

比如我們來打開淺色深色主題的 Worktree，以往我們可能是人工手打，或是點擊「Generate」讓 AI 自動產生。

但人工手打不僅耗時，每個人的撰寫習慣也各不相同；但假使點擊 Generate 讓 AI 自動產生，他生成的訊息有時候是英文，有時是中文，而且長短很隨機。

所以 commit 這塊，我也有設計一個 Skill，讓 AI 根據我的指引生成。

我們先回到專案的目錄，點擊「.agent」資料夾，在 skills 底下有一個「git-smart-commit」的技能：

他的任務就是把雜亂的變更，拆分成有意義的 commit；**畢竟請 AI 輔助開發時，很可能一口氣改了很多個檔案，如果全部放在一個 commit，那這個 commit 就跟沒寫一樣。**

這邊我們把左側的選單切換到 Source Control，你就會看到這個淺色深色主題有修改到多個檔案

這邊我就在對話窗輸入

```
新增 commit

```

稍等一段時間，會看到 AI 根據變更的檔案，生成對應的 commit 訊息：

commit 送出後，記得要點擊 Publish Branch 才會將檔案上傳喔！

完成後可以把這個工作區關閉，另外兩個 Worktree 也是一樣操作即可。

都 Publish 之後，我們回到 GitHub 網頁，此時你會看到下拉選單中有剛剛上傳的 branch：

隨便點擊一個，就可以看到剛剛撰寫的 commit 訊息：

但目前的 feature 都在不同的 branch，所以接下來會帶大家透過 Pull Reqeust，將功能合併到主線。

## ⑤ 好的 Pull Request，讓 Code Review 變輕鬆

這邊先點擊上方的「Pull Requests」：

選擇「New Pull Requests」：

如果你是 fork 我的專案，這邊要特別注意，base 要改成自己的帳號，然後 compare 要選擇想合併進來的 feature 分支：

建立 PR 後，你會看到要填寫標題與描述，根據我的工作經驗，PR 是決定專案品質的重要環節：

因為他是讓團隊成員 Code Review 時，理解你到底做了些什麼的文件

**如果隨便寫寫，那審核的人通常也是隨意看看；畢竟直接讀程式碼，去理解功能為何這樣設計，真的太消耗腦力了。**

我知道很多人嫌寫 PR 很麻煩，所以我也設計了一個 Skill，用來生成 Pull Request 的草稿。

這邊我們把視角切換到 Antigravity，這個 Skill 一樣是放在「.agent/skills」底下，名稱叫做「git-pr-description」。

這邊跟大家簡述這個 Skill 的設計

- 首先，預設合併的目標為 master 這個主線分支，如果你是用 main/develop 等其他的分支請自行調整。
- 接下來就會把當前的分支與目標分支進行比對，讀取差異的檔案與 commit 訊息，這邊也再次跟大家提醒 commit 訊息是很重要的，你不想寫，至少要讓 AI 幫你寫吧。
- 而後就是分析變更的內容，然後生成 Pull Request 的 Title 與 Description。
- 其中 Description 的部分，我是要求他參考「pr-template」的這個範例檔來撰寫，如果大家想把這個 Skill 導入目前的工作流，可以改寫這個檔案。

講了這麼多，我直接示範給大家看比較快；一樣打開 Agent Manager，然後再各自的 Agent 輸入。

```
撰寫 PR

```

稍等一段時間後，他就會根據 Skill 提供的格式生成 Pull Request 的草稿了：

這邊我們把它複製到 GitHub 後，我強烈建議大家至少要跟著測試案例執行過一遍。

如果是前端專案的話，建議附上對應的圖片，這樣可以減少別人 Code Review 時的負擔，同時也確認了功能符合預期。

如果專案是自己開發的，那就直接點擊 Merge 合併就好。

接下來回到 Antigravity，合併好的 Worktree，我個人會建議直接點擊右鍵，在 Worktree Action 當中選擇 Remove Worktree 將他移除，避免 Worktree 太多管理困難。

至於另外兩個 Worktree 也是一樣操作，先建立 Pull Request，接著用 AI 生成對應的 Review 草稿，確認沒問題後合併回主線，完成這一切後記得移除 Worktree。

但有一點要提醒一下大家，因為 Worktree AI Agent 同時開發，所以開發當下不會知道彼此的變更

**所以把功能合併回主線時，可能出現衝突**；像常見問答跟淺色深色的分支在合併時，都遇到了這個問題：

點擊「Resolve conflicts」可以看到發生衝突的位置，但我個人並不建議在網頁上調整，因為你無法得知調整後的實際狀況：

**我建議回到程式碼編輯器上操作，本地解決完衝突，確認結果符合預期後再更新上去。**

這邊先回到 Antigravity 的頁面，我以切換淺色深色主題這個分支來做示範。

首先，我們要打開終端機，執行 `git fetch origin` 拉取遠端最新資料 然後再執行 `git merge origin/master` 來同步主線的更新。

執行後，你就會發現左側 Source Control 這個地方，在 Merge Changes 有一個衝突的檔案：

打開後就會看到產生衝突的位置，衝突通常分成「語法衝突」跟「邏輯衝突」。

邏輯衝突較難處理，因為要考量到相容性或是改寫程式，這個之後有機會透過一個主題專門分享。

而現在看到的這個是調整 CSS 網頁外型的語法衝突；只是括號位置撞到了而已，所以我選擇 Accept Current Change。

這邊也跟大家提一下前面安裝 Gitlens 外掛的好處，像這邊滑鼠移到程式碼時，你就可以看到當時是誰更新了這段程式、更新的目的是什麼：

處理完衝突後，按下「+」，然後輸入 commit

```
chore: sync with master

```

這是與主線同步的意思，然後點擊「Sync Changes」推送上去。

此時回到 GitHub，刷新一下網頁，就會發現可以順利合併嚕！

等所有 feature 都合併到主線後，我們可以在 master 的這個地方輸入 `git pull`，拉回變更。

然後輸入 `pnpm run dev`，來確認合併後的內容是否符合預期。

這邊把視角切到網頁，我們可以看到第一次進入時，下方會有 Cookie 的詢問視窗，上面可以切換淺色、深色主題，往下滑一點，常見問題的段落也有順利顯示。

如果文章看到這邊，先給自己拍拍手，然後別忘記幫忙點個讚。

假使你有跟著教學做到這個步驟，歡迎留言，讓我知道觀眾裡面有狠人，因為這個主題真的不容易。

## 結語：人，才是 AI 的瓶頸

完成這隻影片後，我不禁有個特別的感悟。

因為這隻影片不管是專案範例、Commit 訊息、Agent Skills，甚至連 Pull Request 都是 AI 生成的。

我從頭到尾一行程式都沒有寫，主要任務就是去 Review AI 給的建議、執行的成果。

我想未來絕大多數產品，背後也都是 AI 在進行開發與迭代。

但這並不代表 AI 可以取代工程師，雖然現在人人都可以寫程式；**但「團隊溝通、功能擴充、系統維護、問題解決」都是需要經驗的。**

就拿今天的影片來舉例好了，我是因為觀察到 Code Review 的速度，已經跟不上 AI 寫程式的速度。

當人成為 AI 的瓶頸時，就要去分析如何降低門檻。

比如不同功能要建立各自的分支，以減少 Code Review 的認知負擔。又或者完成功能後，請 AI 分析程式碼分段 commit，並撰寫 Pull Request 草稿，讓團隊協作的資訊有一致性。

**雖然每一步都是 AI 在執行，但如果沒有業界的實戰經驗，大家對這些工具如何與 AI 協作是沒有概念的。**

但這些才是真正的痛點，只要設計好就能提升個人與團隊的工作效率。

當然，我相信一定還有很多更好的方案，歡迎大家留言分享。

最後提醒大家，不管 AI 多強大，總需要有個人按下同意按鈕。

當你按下去的那刻，責任就歸屬於你，畢竟 AI 還不能幫人背鍋。

如果這篇文章對你有幫助，可以順手按個讚，並把文章分享給對 AI 主題感興趣的朋友們。

  [https://www.tenlong.com.tw/products/9786264142083](https://www.tenlong.com.tw/products/9786264142083)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202602/git-worktree-ai-agents/
