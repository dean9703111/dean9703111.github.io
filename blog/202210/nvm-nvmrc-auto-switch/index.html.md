# 使用 nvm + .nvmrc 讓 Node.js 版本在不同的專案間自動切換

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202210/nvm-nvmrc-auto-switch/
- 發布：2022-10-26（更新：2023-05-05）
- 主題：技術教學
- 標籤：Node.js、nvm、Version Control、Zsh、Nvmrc
- 摘要：Node.js 版本更新速度極快，但未必所有套件都能相容於新的版本，這造成每個專案的 Node.js 版本不盡相同；所以今天要介紹如何透過 .nvmrc 搭配 zsh or bash script 讓你在專案間自動切換 Node.js 版本。

Node.js 版本更新速度極快，但未必所有套件都能相容於新的版本，這造成每個專案的 Node.js 版本不盡相同；即使 nvm 可以幫我們快速切換 Node.js 版本還是不太方便，所以今天要介紹如何透過 .nvmrc 搭配 zsh or bash script 讓你在專案間自動切換 Node.js 版本。

筆者原本選用「avn」這個套件，但它真的已經年久失修了無法使用[連結](https://github.com/wbyoung/avn/issues/109#issuecomment-945155272)

## STEP 1：建立兩個 Node.js 專案

在終端機複製貼上下方指令即可。

```
mkdir project-a
cd project-a
npm init -y
cd ..
mkdir project-b
cd project-b
npm init -y
cd ..

```

## STEP 2：安裝特定的 Node.js 版本以供測試

尚未 nvm 安裝的讀者，可以先透過如下指令安裝，更新的版本請參考[連結](https://github.com/nvm-sh/nvm#installing-and-updating)。

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.2/install.sh | bash

```

安裝完後建議重開終端機讓它生效，接著透過它安裝 Node.js 特定版本：

```
nvm install 12
nvm install 16

```

## STEP 3：在兩個 Node.js 專案中加入「.nvmrc」並指定 Node.js 版本

接下來我們就分別在兩個專案中新增「.nvmrc」的檔案，並個別指定 12、16 版本方便測試（讀者在終端機複製貼上下方指令即可）。

```
cd project-a
touch .nvmrc
echo "12" >> .nvmrc
cd ..
cd project-b
touch .nvmrc
echo "16" >> .nvmrc
cd ..

```

筆者為了方便 Demo 而指定大版本，「.nvmrc」檔案是能吃「x.y.z」版本號的。

## STEP 4：調整 zsh script

在終端機輸入 `vi ~/.zshrc`（每個人的路徑可能不同），到最後一行貼上下方 script：

```
# place this after nvm initialization!
autoload -U add-zsh-hook
load-nvmrc() {
  local node_version="$(nvm version)"
  local nvmrc_path="$(nvm_find_nvmrc)"
  if [ -n "$nvmrc_path" ]; then
    local nvmrc_node_version=$(nvm version "$(cat "${nvmrc_path}")")
    if [ "$nvmrc_node_version" = "N/A" ]; then
      nvm install
    elif [ "$nvmrc_node_version" != "$node_version" ]; then
      nvm use
    fi
  elif [ "$node_version" != "$(nvm version default)" ]; then
    echo "Reverting to nvm default version"
    nvm use default
  fi
}
add-zsh-hook chpwd load-nvmrc
load-nvmrc

```

儲存後再輸入 `source ~/.zshrc` 即可讓 script 生效。

這邊簡單說明 script 運行邏輯：

- 資料夾下**有**「.nvmrc」的檔案：若指定的 Node.js 版本已安裝就透過「nvm use」切換，假使該版本尚未安裝就會執行「nvm install」。
- 資料夾下**沒有**「.nvmrc」的檔案：切換回 Node.js 默認的版本。

唯一要注意的是，**記得把這段 script 放在 nvm 初始化後的位置**（或是直接放在最後面也可以）。

上面的 script 參考自 nvm 官網，如果讀者使用的是 bash script 請參考[連結](https://github.com/nvm-sh/nvm#bash)。

## STEP 5：實測是否有效

筆者在開頭故意切換到 Node.js 18 的版本，然後再依序切換到「project-a」以及「project-b」的資料夾，從下圖大家可以看到 Node.js 的版本都順利切換嚕！

希望這篇文章，有幫大家解決不同專案的 Node.js 版本切換之苦～

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202210/nvm-nvmrc-auto-switch/
