# 在 Local 導入 Whisper 套件，用 Python 免費將 Youtube 影片轉換成逐字稿！

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202304/local-whisper-youtube-transcript/
- 發布：2023-04-01
- 主題：AI 實戰
- 系列：YouTube 影片轉逐字稿
- 標籤：ChatGPT、Whisper、OpenAI、YouTube、Python
- 摘要：如果每次轉逐字稿都使用線上的 Whisper API 會快速消耗 OpenAI 的 Token，要是時常有轉長影片的需求，那可能得花不少錢在上面。 所以這篇文章筆者要向大家介紹如何在 Local 環境導入 Whisper 套件的方案，免費將 Youtube 影片轉換成逐字稿！

如果每次轉逐字稿都使用線上的 Whisper API 會快速消耗 OpenAI 的 Token，要是時常有轉長影片的需求，那可能得花不少錢在上面（雖然他真的很方便）。

所以這篇文章筆者要向大家介紹如何在 Local 環境導入 Whisper 套件的方案，免費將 Youtube 影片轉換成逐字稿！

> 提醒大家，不要未經授權下載有版權保護的影片喔！

> **小提醒**：雖然原則上按照步驟就能順利完成，但每個人的電腦環境會有些許差異，遇到特殊狀況時，請自行排除（或是問 ChatGPT）。

## 觀看 Whisper 官方文件

GitHub 官網：[https://github.com/openai/whisper](https://github.com/openai/whisper)

  有興趣的朋友可以看圖片，來了解這款套件的訓練方式，這邊筆者就不再贅述。

## 安裝 Whisper 套件

**STEP 1**：首先要特別注意的，就是目前這款套件是使用「Python 3.9.9」的版本，如果你安裝的是最新版的 Python，那後續連基礎的安裝就會失敗。

安裝 Python 3.9 版本：`brew install [python@3.9](mailto:python@3.9)`

**STEP 2**：如果你在 local 環境已經安裝了其他版本的 Python，就需要切換到 3.9 的版本才能夠順利執行後續安裝，建議直接改 alias 比較快。

```
vi ~/.zshrc
alias python='python3.9'
alias pip='pip3.9

```

**STEP 3**：前面兩個步驟完成後，接下來安裝「通常」就會順利很多。

```
pip install torch torchvision torchaudio
pip install ffmpeg-python
pip install -U openai-whisper

# windows (https://chocolatey.org/)可使用 choco install ffmpeg
brew install ffmpeg
pip install setuptools-rust

```

> 如果想了解每個套件負責的任務是什麼，可以參考官網：[https://github.com/openai/whisper#setup](https://github.com/openai/whisper#setup)

**STEP 4**：確認是否安裝成功。

在終端機輸入指令：`whisper –help`，如果出現下面的畫面就代表你安裝成功嚕！

## 用 Whisper 套件將 Youtube 影片轉換成逐字稿

筆者在[前面的文章](https://deanlin.net/blog/202303/python-youtube-to-mp3-with-chatgpt/)，已經完成將 Youtube 影片轉換成音檔的程式，接下來我們可以請 ChatGPT 幫我們做程式組合的任務。

> 請自行安裝 pytube、pydub 的套件。

😀：「下面是我用 Python 寫的 Youtube 影片轉成音檔的程式，我想要在這個基礎上擴充功能，我在後續的對話中會提出自己的需求，請問你可以幫忙嗎？

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

😀：「我找到了一個可以將 mp3 檔案轉成文字檔的套件，官方範例如下，我要如何將他與我現在的程式結合呢？

```
import whisper

model = whisper.load_model("base")
result = model.transcribe("audio.mp3")
print(result["text"])

```

我們在前面已經完成相關的套件安裝了，接下來直接嘗試看看 ChatGPT 組合後的程式是否靠譜。

```
from pytube import YouTube
from pydub import AudioSegment
import whisper

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

# 讀取 MP3 檔案，轉成文字檔案
model = whisper.load_model("base")
result = model.transcribe("path/to/save/directory/audio.mp3")
print(result["text"])

```

**SETP 1**：建立一個`youtube-local-whisper.py`的檔案，貼上 ChatGPT 的程式。

**SETP 2**：在終端機輸入`python youtube-local-whisper.py`，然後稍等一段時間…就可以見證奇蹟啦！

  儘管有成功轉換逐字稿，但這個品質真的有點悲劇。

## 提升轉出的逐字稿品質

在程式中，預設的 model 是 base 模型，實際上他有提供這些模型供我們選擇：「tiny/base/small/medium/large」

不過越往上，對電腦的需求越高，花費時間越多，我們可以先看一下官方提供的對照表：

除了 model 的選擇外，另一個是「語言」，如果一開始有設定好也能提升辨識的準確度（ex：中文是「zh」）。

> 有興趣的朋友可以看一下套件目前提供的語言列表：[https://github.com/openai/whisper/blob/main/whisper/tokenizer.py](https://github.com/openai/whisper/blob/main/whisper/tokenizer.py)

下面是筆者將語言調整成「zh」，並選擇「medium」這個 Model 優化的程式（除非你電腦性能超級優秀，否則選擇 large 會等到天荒地老）：

```
from pytube import YouTube
from pydub import AudioSegment
import whisper

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

# 讀取 MP3 檔案，轉成文字檔案
model = whisper.load_model("medium")
result = model.transcribe("path/to/save/directory/audio.mp3", language='zh')
print(result["text"])

```

這次輸出的結果總算是比較接近預期的了（真的要等一段時間喔…）。

如果覺得看起來眼花，那就交給 ChatGPT 幫你整理一下吧！

😀：「幫我改錯字、分段、加標點符號，讓內容更通順：[逐字稿]」

  大家可以發現裡面的文字不是百分百正確，品質還是需要自己把關的喔！

## 結論：優化還是需要一定的專業水平

如果是一個 0 基礎的人，我會更建議使用筆者的[另外一篇文章](https://deanlin.net/blog/202303/python-youtube-transcript-whisper/)，來實作 Youtube 轉逐字稿的功能。

因為在 Local 環境導入 Whisper 真的還是有一些技術門檻，光是環境設定的部分，就可能讓沒經驗的人經歷「困惑 ➝ 絕望 ➝ 投降」的 3 個階段；不過，如果能順利設定完成，那將給自己帶來巨大的成就感。

儘管初步的程式，我們可以靠 ChatGPT 幫我們組合出來；但因為 Whisper 畢竟是一款新的套件，如果想要對他優化，還是需要具備閱讀技術文件的能力。

有了 AI 輔助，工程師就能把更多的精力放在優化而非基礎建設上，相信在不久的將來，M 型化會越來越明顯。

延伸閱讀：[AI 實戰](https://deanlin.net/categories/ai/)

**想將 AI 導入自己的職場與生活嗎？歡迎[購買筆者出版的新書](https://www.books.com.tw/products/0011004216)**，幫助您**在 AI 的浪潮中站穩腳步，並在未來的職場競爭中佔據優勢！**

  [https://www.books.com.tw/products/0011004216](https://www.books.com.tw/products/0011004216)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202304/local-whisper-youtube-transcript/
