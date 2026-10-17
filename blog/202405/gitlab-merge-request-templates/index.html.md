# [Gitlab] 建立 Merge Request Templates，讓團隊合作更加順利！

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202405/gitlab-merge-request-templates/
- 發布：2024-05-11（更新：2024-05-17）
- 主題：團隊合作
- 標籤：GitLab、Merge Request、Template、Project Management、Teamwork
- 摘要：在多人協作的開發過程中，我們通常會需要透過 Merge Request 來進行 Code Review。 但這時會遇到一個問題：「每個人對專案的認知有差異，且文字表達能力不同。」 這會導致 Code Review 的人需要花更多的時間來理解程式，甚至浪費寶貴的時間在溝通上。

在多人協作的開發過程中，我們通常會需要透過 Merge Request 來進行 Code Review。

但這時會遇到一個問題：「每個人對專案的認知有差異，且文字表達能力不同。」

這會導致 Code Review 的人需要花更多的時間來理解程式，甚至浪費寶貴的時間在溝通上；有些交代不清楚的 Merge Request 如果被合併了，也容易在未來發現 Bug 時難以釐清問題。

而解決上面問題的方式也很簡單：「建立 Template（模板）」

如果大家都按照模板撰寫，就能保證每個 MR 都有一定的品質。

## Merge Request 要有哪些資訊？

筆者個人建議一個好的 Merge Request 要包含以下元素：

- **概要 & 背景**：簡短描述這次 Merge Request 的目的，讓 Code Review 的人可以快速理解前因後果。
- **相關資訊**：放上專案需求規格連結、關聯的 Project/branch、會議紀錄等資訊。
- **變更內容**：說明新增/修改/移除了什麼內容，如果邏輯複雜可以簡述演算法。
- **測試步驟**：描述如何驗證這次的變更，包括需要運行的測試和預期結果。

大家可以參考我的 Merge Request Template 格式（Markdown 語法）：

```
# 概要 & 背景

# 相關資訊

- 需求規格:
- 關聯的 Project/branch:
- 任務 Ticket/會議紀錄:

# 變更內容

- [新增] xxx
- [修改] yyy
- [移除] zzz

# 測試步驟

### 測試項目 1

- STEP 1:
- STEP 2:
- STEP 3:

預期結果:

```

英文版：

```
# Summary & Background

# Relevant Information

- Requirement Specifications:
- Associated Project/Branch:
- Task Ticket/Meeting Notes:

# Changes

- [Added] xxx
- [Modified] yyy
- [Removed] zzz

# Testing Steps

### Test Case 1

- STEP 1:
- STEP 2:
- STEP 3:

Expected Result:

```

## 在 Gitlab 建立 Merge Request Templates 的步驟

### STEP 1: 選擇 Project default branch，建立存放 Templates 的資料夾

我們要先在根目錄下建立 `.gitlab/merge_request_templates/` 的資料夾。

**小提醒**：一定要在「default branch（ex: main、develop）」下建立，否則是沒有效的。

### STEP 2: 在資料夾下建立「.md」的 Templates

你可以直接複製筆者在前面提供的 Markdown Template，這邊我將檔案命名為「merge.md」。

這邊我開一個測試的 branch 來做 Merge request，大家可以看到在 Description 旁邊有一個下拉選單，裡面的 merge 就是我們剛剛新增的模板。

選擇後就能看到剛剛建立的 Merge request Template：

相信一個好的 Merge request Template 能提高專案的品質，並加速團隊的協作流程，減少不必要的溝通成本。

延伸閱讀：[軟體工程](https://dean-lin.medium.com/list/7f3c9b6e2442)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202405/gitlab-merge-request-templates/
