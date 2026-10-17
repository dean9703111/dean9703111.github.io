# 0 基礎！讓 ChatGPT 手把手教你用 Python 將 Youtube 影片轉成音檔（mp3）

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202303/python-youtube-to-mp3-with-chatgpt/
- 發布：2023-03-29
- 主題：AI 實戰
- 系列：YouTube 影片轉逐字稿
- 標籤：ChatGPT、Python、YouTube、MP3、Tutorial
- 摘要：筆者真的嚇到了🫢，沒想到我真的做到一行程式碼都沒寫，以菜鳥的語氣跟 ChatGPT 對話，就能完成一個用 Python 將 Youtube 影片轉成音檔（mp3）的小專案。

筆者真的嚇到了🫢，沒想到我真的做到一行程式碼都沒寫，以菜鳥的語氣跟 ChatGPT 對話，就能完成一個用 Python 將 Youtube 影片轉成音檔（mp3）的小專案。

在這篇文章中，我附上與 ChatGPT 的完整對話，大家只要複製貼上就可以嘗試看看嚕！

> Python 環境安裝的部分我就不贅述了，筆者使用的是 Python3；不過有設定 alias name，讓 python3/pip3 的指令對應 python/pip。

😀：「我使用的程式語言是 Python，他有辦法抓取 Youtube 影片的音擋嗎？」

> ChatGPT 這邊也有提醒大家，不要未經授權下載有版權保護的影片喔！

```
from pytube import YouTube

# 設定要下載的影片 URL
url = "https://www.youtube.com/watch?v=VIDEO_ID"

# 建立 YouTube 物件
yt = YouTube(url)

# 取得影片的音軌
audio_stream = yt.streams.filter(only_audio=True).first()

# 下載音軌到指定的路徑
audio_stream.download(output_path="path/to/save/directory", filename="audio")

```

**SETP 1**：建立一個`youtube-test.py`的檔案，貼上 ChatGPT 的程式。

**SETP 2**：如果要執行程式，請安裝套件：`pip install pytube`。

**SETP 3**：將 url 替換成自己的：[https://www.youtube.com/watch?v=IvOdTbIYZs4](https://www.youtube.com/watch?v=IvOdTbIYZs4%22%EF%BC%88%E9%80%99%E6%98%AF%E7%AD%86%E8%80%85%E5%88%86%E4%BA%AB%E5%87%BA%E6%9B%B8%E5%BF%83%E5%BE%97%E7%9A%84%E5%BD%B1%E7%89%87%EF%BC%89%E3%80%82)

> 這是筆者分享出書心得的影片。

**SETP 4**：在終端機輸入`python youtube-test.py`執行它，你會發現他自動建立了資料夾路徑，裡面存放著「audio」的檔案。

😀：「有順利在「path/to/save/directory」這個路徑下產生「audio」的檔案了，但他無法打開，這個真的是音擋嗎？」

> 請安裝套件：`pip install pydub`

😀：「如何與上一段程式結合呢？」

```
Copy code
from pytube import YouTube
from pydub import AudioSegment

# 設定要下載的影片 URL
url = "https://www.youtube.com/watch?v=VIDEO_ID"

# 建立 YouTube 物件
yt = YouTube(url)

# 取得影片的音軌
audio_stream = yt.streams.filter(only_audio=True).first()

# 下載音軌到指定的路徑
audio_stream.download(output_path="path/to/save/directory", filename="audio")

# 讀取下載下來的音檔
audio_file = AudioSegment.from_file("path/to/save/directory/audio")

# 檢查音檔是否可以正常解碼
print(audio_file)

```

在終端機輸入`python youtube-test.py`，結果冒出一堆錯誤訊息。

把錯誤的結果貼到 ChatGPT，他是會幫你解答的。

> 若未安裝 homebrew 的朋友也可以詢問 ChatGPT 如何安裝。

在終端機輸入`brew install ffmpeg`，之後等他安裝完成（頗久的…）

再輸入一次`python youtube-test.py`，從終端機來看，天下太平了！

然後我們把 ChatGPT 的建議加入最後一行，讓他轉成 mp3 檔案。

```
from pytube import YouTube
from pydub import AudioSegment

# 設定要下載的影片 URL
url = "https://www.youtube.com/watch?v=IvOdTbIYZs4"

# 建立 YouTube 物件
yt = YouTube(url)

# 取得影片的音軌
audio_stream = yt.streams.filter(only_audio=True).first()

# 下載音軌到指定的路徑
audio_stream.download(output_path="path/to/save/directory", filename="audio")

# 讀取下載下來的音檔
audio_file = AudioSegment.from_file("path/to/save/directory/audio")

# 檢查音檔是否可以正常解碼
print(audio_file)

# 將音檔轉換為 MP3 格式
mp3_file = audio_file.export("path/to/save/directory/audio.mp3", format="mp3")

```

此時你在終端機執行`python youtube-test.py` 會有一種感動！這個 mp3 擋真的在播放 Youtube 影片的音檔了…太神了…

## 結語：讓不會程式的人更願意嘗試

寫程式是一條非常艱辛的路，中間會遇到非常多的險阻與挫折。

就像上面展示的範例一樣，我們很難一次就得到正確的結果，但在發生錯誤時，過往會需要花費大量的時間除錯、找文件。

這些失敗的經歷會使人成長，但同時也會消耗熱情。

而 ChatGPT 的出現，很大程度地減緩了這塊的痛苦，讓我們更容易上手程式。

> 不過這對程式教育產業來說，可說是巨大的衝擊，因為人們多了一個全新的選擇（而且還不賴）。
> 下一篇就來分享[如何搭配 OpenAI 的 whisper api 將音檔（mp3）轉換成文字吧](https://deanlin.net/blog/202303/python-youtube-transcript-whisper/)！

延伸閱讀：[讓我們一起掌握 AI 帶來的紅利](https://dean-lin.medium.com/list/ddcc9c53a4ac)

**想將 AI 導入自己的職場與生活嗎？歡迎[購買筆者出版的新書](https://www.books.com.tw/products/0011004216)**，幫助您**在 AI 的浪潮中站穩腳步，並在未來的職場競爭中佔據優勢！**

  [https://www.books.com.tw/products/0011004216](https://www.books.com.tw/products/0011004216)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202303/python-youtube-to-mp3-with-chatgpt/
