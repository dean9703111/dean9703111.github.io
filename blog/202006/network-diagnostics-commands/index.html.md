# 網路環境檢查

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202006/network-diagnostics-commands/
- 發布：2020-06-15
- 主題：技術教學
- 標籤：Nmap、Ping、Traceroute、網路
- 摘要：在這裡會說明幾個我常使用的檢查方式

> 以下指令皆在Terminal(終端機)操作

## Ping xxx.xxx.xxx.xxx 最基礎的一種

這是檢察網路環境最方便的指令，無論windows、mac還是linux都可以在不用安裝任何套件的狀態下使用他

**主要功能是拿來測試主機是否有連通內網外網，或是開通特定主機的通道**

  你可以直接ping某個Domain name

  或是ping特定的ip

個人比較常用的是加上ping次數的限定

```
基礎指令如下：
-c n
n為要ping的次數
ex: ping -c 10 192.168.120.206

```

  這是只跑一次的範例

## Nmap 常用來確認防火牆以及哪些port有打開

這是一個非常棒的網路診斷工具，如果你的系統尚未安裝可下指令如下

```
sudo apt-get install nmap

```

下面介紹我常用的指令：

1. 掃描主機是否有開機，並且開啟哪些port

```
nmap www.hinet.net

```

2. 如果你要得到該主機更詳細的資訊可加上參數 -v

```
nmap -v www.hinet.net

```

3. 確認防火牆開啟的狀況(這個指令需要進入sudo高級權限)

```
nmap -sA scanme.nmap.org

```

4. 常用到的應該是確認該port是否開啟

```
nmap -p 80 google.com

```

## traceroute 主要是為了用來追蹤路徑

你可以理解為這個指令就是列出從你電腦到目標網段所走過的路，主要是為了檢查在哪個環節斷掉

```
traceroute google.com

```

下面的幾個參數有興趣的可以自己嘗試看看喔

```
traceroute -p 6888 google.com
探測包使用的基本UDP埠設置6888
traceroute -r google.com
繞過正常的路由表，直接發送到網路相連的主機

```

> 希望這篇文章提供的方法，可以幫助你在查找問題時更加順利～

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202006/network-diagnostics-commands/
