# PrivateGPT + llama 安裝教學 ── 用開源模型打造無需連網、有私人專屬知識庫的 AI

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202412/privategpt-llama-offline-rag/
- 發布：2024-12-06
- 主題：AI 實戰
- 標籤：PrivateGPT、Llama 3、AI、Local、RAG
- 摘要：儘管 ChatGPT、NotebookLM、Copilot 等 AI 工具非常強大，但許多公司在使用上仍有許多隱私疑慮。 所以今天想跟大家分享一款能離線或在內網使用的 AI 工具 ── PrivateGPT

## 用開源模型打造無需連網、有私人專屬知識庫的 AI ── PrivateGPT + llama 安裝教學

儘管 ChatGPT、NotebookLM、Copilot 等 AI 工具非常強大，但許多公司在使用上仍有許多隱私疑慮。

所以今天想跟大家分享一款能離線或在內網使用的 AI 工具 ── PrivateGPT，在搭配 llama 這個開源模型後，儘管他不像 ChatGPT 那麼學識淵博，但在基礎對話與文件檢索的能力上已經足夠日常工作使用。

開始閱讀前先提醒一下大家，這個工具的安裝有點複雜，建議具備相關程式背景知識，過程遇到非預期的錯誤實屬正常。

儘管[官網文件](https://python-poetry.org/docs/#installing-with-the-official-installer)很詳細，但總是有文件外的問題發生，做好心理準備後就可以開始了！

```
文章大綱

一、安裝前置環境（Python、Terminal、Git、VSCode）
二、安裝 PrivateGPT
三、安裝 Ollama
四、啟動 PrivateGPT
五、體驗離線與文件對話的功能
六、安裝過程可能會遇到的問題
七、總結：目前 RAG 會遇到的問題＆使用建議

```

## 一、安裝前置環境（Python、Terminal、Git、VSCode）

- **Python**：官方建議安裝 3.11 的版本，[官網](https://www.python.org/downloads/)
- **Terminal**：使用哪個終端機都可以，筆者是使用 [Oh My Zsh](https://ohmyz.sh/)
- **Git**：管理程式版本的工具，[官網](https://git-scm.com/downloads)
- **VSCode**：方便編輯相關設定檔案，[官網](https://code.visualstudio.com/)

> **筆者的電腦環境為**： `MacBook Pro，Apple M1 MAX 晶片，macOS 為 15.0.1`

## 二、安裝 PrivateGPT

**STEP 1**：用 `Git` 下載專案

```
git clone https://github.com/zylon-ai/private-gpt
cd private-gpt

```

**STEP 2**：安裝 `Poetry` 來下載相關資源

> 本文以 Mac 安裝步驟為主，Windows 可以參考[官網](https://python-poetry.org/docs/#installing-with-the-official-installer)

```
curl -sSL https://install.python-poetry.org | python3 -

```

將 Poetry 加入你的指令，並確認他安裝成功。

```
export PATH="/Users/lindingyuan/.local/bin:$PATH"
poetry --version

```

**STEP 3**：安裝 `make` 來跑 scripts

```
brew install make

```

## 三、安裝 Ollama

**STEP 1**：到[官網](https://ollama.com/)下載，並安裝到電腦（根據網路環境不同，可能要安裝一段時間）。

**STEP 2**：安裝完成後，請記得`關閉這個應用程式`，然後透過 Terminal 來執行 Ollama。

```
ollama serve

```

**STEP 3**：原本的 Terminal 不要關閉，再開一個 Terminal 來安裝 PrivateGPT 所需要的資源。

```
poetry install --extras "ui llms-ollama embeddings-ollama vector-stores-qdrant"

```

## 四、啟動 PrivateGPT

如果安裝一切順利，接下來執行下面的指令就可以體驗 PrivateGPT & ollama 的強大了！

```
PGPT_PROFILES=ollama make run

```

如果順利的話，透過連結: [http://localhost:8001](http://localhost:8001) 就可以訪問嚕！

目前預設使用的 llama 模型是 3.1，你可以進去專案文件，編輯 `settings-ollama.yaml` 的內容，將它改成最新的版本（筆者撰寫文章時，最新版本為 3.2）。

> 調整完要關閉重啟才會生效喔！

## 五、體驗離線與文件對話的功能

PrivateGPT 有幾種模式供大家使用：

- RAG: 從選擇的文件中取得答案。
- Search: 從選擇的文件中尋找資訊。
- Basic: 與使用的 LLM（ex: llama3.2）聊天，不會參考文件資訊。
- Summarize: 從選擇的文件提取摘要。

過去將公司的需求規格書、會議記錄上傳到雲端的 AI 平台會有資安方面的疑慮。

但現在 PrivateGPT 就解決了這個問題，這邊我上傳了 10 份會議記錄(用 AI 生成的模擬資料)，向大家展示他在 RAG 這個領域的表現。

筆者測試過多種情境後，體驗的結果是「尚可使用」，想擁有良好體驗需要再做很多人為的設計；目前跟 NotebookLM 相比，還有很大的差距。

> 關於 NotebookLM 可以[參考這篇文章](https://deanlin.net/blog/202411/notebooklm-workplace-uses/)

另外 llama3.2 的基礎對話能力其實已經表現得挺不錯了。

## 六、安裝過程可能會遇到的問題

最後分享筆者在安裝過程中遇到的問題。

**問題**：執行 `make run` 時出現「xcrun: error: invalid active developer path (/Library/Developer/CommandLineTools)…」錯誤

**解決方案**：執行 `xcode-select –install` 便可安裝。

安裝過程有點久，請耐心等候…

## 七、總結：目前 RAG 會遇到的問題＆使用建議

今年年初，筆者分享過 [GPT4All](https://deanlin.net/blog/202401/gpt4all-offline-ai/) 這款 AI 工具；體驗上 GPT4All 較適合個人使用，而 PrivateGPT 則適合企業。

而他們是否足夠聰明，很大程度是跟你選擇的 Model 有正相關，因為能接受的 max_new_tokens、context_window 極限值不同。

下面分享幾個目前 RAG 工具會有的問題：

- **不懂裝懂**：可能這份文件中明明缺少這些資訊，但他卻煞有其事的給出資訊，你可以透過：「如果不確定答案，直接告訴我你不知道」的 Prompt 減少這類錯誤。
- **沒有抓到正確的答案**：可能明明有答案，但 AI 對內容的權重排序錯誤導致答案錯誤（比如他以為目錄就是解答，但實際上應該到章節內找答案），可以嘗試縮小檢索範圍來提升準確率。
- **回答不全面**：你可能需要 A、B、C 文件的綜合資訊，但他給的內容遺漏許多細節，這邊會建議每個文檔分開詢問。
- **答案錯誤或相互矛盾**：如果來源品質本身就不好，那就不要期待他能給出多好的答案；建議先做資料清洗，以維持來源品質。

其實 NotebookLM 官方有表明[不會將個人資料用於訓練 NotebookLM](https://web.archive.org/web/20241118233308/https://support.google.com/notebooklm/answer/14275965)，因此企業除非有很強的 AI 人才了解如何優化模型提高輸出品質，否則 NotebookLM 也許是目前的最優解。

**如果你想更全面的掌握生成式 AI 工具，歡迎購買筆者出版的新書，他將幫助您在 AI 的浪潮中站穩腳步，並在未來的職場競爭中佔據優勢！**

  [https://www.books.com.tw/products/0011004216](https://www.books.com.tw/products/0011004216)

也歡迎 Follow 筆者的部落格，我會持續分享最新 AI 工具，目前已有超過 80 篇 AI 相關文章等你來看！

延伸閱讀：[AI 實戰](https://deanlin.net/categories/ai/)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202412/privategpt-llama-offline-rag/
