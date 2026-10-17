# 手把手帶你在 MacOS 安裝 Redis ＆Another Redis Desktop Manager

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202108/install-redis-on-macos/
- 發布：2021-08-11（更新：2023-05-05）
- 主題：技術教學
- 標籤：macOS、Redis、Redis GUI、Redis 安裝
- 摘要：本篇文章除了帶你安裝 Redis，更分享一個超好用的 Redis GUI 神器；整個操作流程不超過 10 分鐘，想入門 Redis 看這篇文章就對啦！

本篇文章除了帶你安裝 Redis，更分享一個超好用的 Redis GUI 神器；整個操作流程不超過 10 分鐘，想入門 Redis 看這篇文章就對啦！

## 一、安裝並啟動 Redis

**SETP 1**：打開終端機輸入下面指令安裝 Homebrew（若安裝過請忽略）

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

```

**SETP 2**：使用 Homebrew 安裝 Redis

```
brew install redis

```

**SETP 3**：啟動 Redis

啟動 Server 終端機執行指令 `redis-server`

開一個 Terminal 啟動 Client 終端機執行指令 `redis-cli` （如果沒開新的 Terminal 直接做是不行的喔！）

**SETP 3**：測試 Redis 基礎功能

```
ping
set foo bar
get foo

```

如果操作結果跟上圖一樣就恭喜你安裝成功嚕～

## 二、安裝及使用 Another Redis Desktop Manager

**SETP 1**：使用 Homebrew 安裝

```
brew install --cask another-redis-desktop-manager

```

**SETP 2**：安裝後可以在應用程式列表看到他，但如果你的作業系統是 MacOS 10.15 以上，打開程式時會遇到如下錯誤

**SETP 3**：看到錯誤別慌張，打開「系統偏好設定」，選擇「安全性與隱私權」

**SETP 4**：在「一般」的分頁下，「強制打開」這個 APP

**SETP 5**：在系統的警告下，鼓氣勇氣「打開」他吧！

**SETP 6**：打開後迎面而來的是簡潔有力的畫面，點擊左上角「New Connection」建立新連線

**SETP 7**：直接使用默認的設定即可，按下「OK」

**SETP 8**：最後進入 DB 確認剛剛建立的 key-value(foo-bar) 是否存在

如果 redis 沒有選擇 DB，預設都是 0

大功告成嚕！恭喜大家踏出學習 Redis 的第一步～

**參考資源：**

- [macOS 上安裝 Redis](https://www.ioa.tw/macOS/Redis.html)
- [Another Redis Desktop Manager](https://github.com/qishibo/AnotherRedisDesktopManager)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202108/install-redis-on-macos/
