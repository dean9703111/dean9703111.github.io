# [n8n 教學]用 Docker 在 Local 建立免費 & 無限次數使用的自動化工具

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202501/n8n-docker-local-setup/
- 發布：2025-01-14
- 主題：AI 實戰
- 系列：n8n 自動化
- 標籤：n8n、AI、Automatic、Workflow、Docker
- 摘要：n8n 是款能把你從重複的例行性任務中，拯救出來的自動化工具。 使用者可以透過視覺化的介面，用拖拉節點、設定參數的方式來建立符合自己需求的工作流。這篇文章會分享本地部署的詳細步驟。

n8n 是款能把你從重複的例行性任務中，拯救出來的自動化工具。

使用者可以透過視覺化的介面，用拖拉節點、設定參數的方式來建立符合自己需求的工作流。

他有[雲端](https://n8n.io/)與本地（local）的版本，考量到許多工作流程需要放上私鑰（ex: Google API Key、OpenAI API Key），以及雲端版本至少要付費 20 歐元（限制工作流程執行 2500 次）；所以筆者選擇了本地部署的方案，這篇文章會分享詳細的操作步驟。

## STEP 1: 安裝 Docker

前往 Docker 官網: [https://www.docker.com/](https://www.docker.com/)

根據自己的作業系統選擇對應的版本下載。

## STEP 2: 使用 docker-compose.yml 安裝 n8n

你可以直接 git clone [筆者的 GitHub 專案](https://github.com/dean9703111/n8n-google-sheet-exmaple)，或者建立一個 `n8n` 的資料夾，新增 `docker-compose.yml` 檔案。

```
volumes:
  n8n_storage:
services:
  n8n:
    image: n8nio/n8n:latest
    restart: always
    ports:
      - "127.0.0.1:5678:5678" # 根據實際需求設定
    volumes:
      - n8n_storage:/home/node/.n8n

```

貼上內容後，在終端機（Terminal）輸入 `docker compose up -d` 即可啟動

  如果要關閉，輸入 `docker cpose down` 即可

## STEP 3: 註冊 n8n 帳號

進入網址: [http://127.0.0.1:5678](http://127.0.0.1:5678/)

如果看到下面的帳號設定畫面就代表你 n8n 安裝成功了！

帳號設定完成後，你會看到下面的初始畫面。

這邊要特別提醒的是，`你一定要記得自己的 Email 跟 Password`，因為官方 Forgot Password 的功能只是擺飾用的。

到這一步，你就完成 n8n local 環境的初始化了！[下一篇文章將會向讀者分享 n8n Node（節點）的使用方法。](https://deanlin.net/blog/202501/n8n-core-nodes/)

**延伸閱讀**:

- [[n8n 教學]掌握重要節點 (Node) 使用情境：Set、Loop Over Items、If、Code、HTTP Request、Wait](https://deanlin.net/blog/202501/n8n-core-nodes/)
- [[n8n 教學]在 Google Sheet 自動建立 SEO 關鍵字矩陣](https://deanlin.net/blog/202501/n8n-seo-keyword-matrix/)

延伸閱讀：[AI 實戰](https://deanlin.net/categories/ai/)

**想將 AI 導入自己的職場與生活嗎？歡迎[購買筆者出版的新書](https://www.books.com.tw/products/0011004216)**，幫助您**在 AI 的浪潮中站穩腳步，並在未來的職場競爭中佔據優勢！**

  [https://www.books.com.tw/products/0011004216](https://www.books.com.tw/products/0011004216)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202501/n8n-docker-local-setup/
