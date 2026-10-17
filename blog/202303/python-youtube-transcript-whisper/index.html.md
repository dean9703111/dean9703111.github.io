# 進階應用！詢問 ChatGPT 如何用 Python 將 Youtube 影片轉換成逐字稿（OpenAI 的 whisper）

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202303/python-youtube-transcript-whisper/
- 發布：2023-03-31
- 主題：AI 實戰
- 系列：YouTube 影片轉逐字稿
- 標籤：ChatGPT、OpenAI、Whisper、YouTube、Python
- 摘要：上一篇文章中，筆者靠 ChatGPT 成功將 Youtube 影片轉換成音檔（mp3），在這篇文章中，我會分享如何用 OpenAI 提供的 whisper api 將 音檔轉成逐字稿。

[上一篇文章中](https://deanlin.net/blog/202303/python-youtube-to-mp3-with-chatgpt/)，筆者靠 ChatGPT 成功將 Youtube 影片轉換成音檔（mp3），在這篇文章中，我會分享如何用 OpenAI 提供的 whisper api 將 音檔轉成逐字稿。

> 如果沒有程式基礎的朋友，建議先從[上一篇文章](https://deanlin.net/blog/202303/python-youtube-to-mp3-with-chatgpt/)開始閱讀。

> 提醒大家，不要未經授權下載有版權保護的影片喔！

目前的 ChatGPT 資料截止於 2021 年 9 月，如果用到最近才新增的技術，我們還是要先到官網查看文件。

OpenAI 技術文件：[https://platform.openai.com/docs/guides/speech-to-text](https://platform.openai.com/docs/guides/speech-to-text)

儘管 ChatGPT 不知道未來世界的資料，但我們還是可以請他幫我們將程式重組。

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
import openai
audio_file= open("/path/to/file/audio.mp3", "rb")
transcript = openai.Audio.transcribe("whisper-1", audio_file)

```

有了 ChatGPT 的提示後，我們可以按照下面的步驟操作。

**SETP 1**：安裝新套件：`pip3 install openai`。

> 如果上一篇文章沒有安裝到，請記得順便安裝不然會有錯誤訊息喔：`pip install pydub pytube`

**SETP 2**：建立一個檔案`youtube-to-text.py`，然後貼上 ChatGPT 提供的程式。

```
from pytube import YouTube
from pydub import AudioSegment
import openai

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

# 將音檔轉換為 WAV 格式
wav_file = audio_file.export("path/to/save/directory/audio.wav", format="wav")

# 使用 OpenAI 的 Audio API 將音檔轉成文字
openai.api_key = "YOUR_API_KEY"
with open("path/to/save/directory/audio.wav", "rb") as f:
    transcript = openai.Audio.transcribe("whisper-1", f)

# 印出轉換後的文字
print(transcript)

```

> 我們原本是將 Youtube 轉換成 mp3 檔，而這次 ChatGPT 改寫的過程中，把它轉換成了 wav 檔；並且對我們提供的範例程式做了一定程度的改寫（ex：告訴我們要自己填入”YOUR_API_KEY”）。

**SETP 3**：請先去 [https://platform.openai.com/account/api-keys](https://platform.openai.com/account/api-keys) 申請自己的 API Key，將 YOUR_API_KEY 替換成自己的。

> **小提醒：**將 API Key 放入環境變數的作法更好，不過這篇文章以 Demo 目的為主，讀者切記不要把自己的 API Key 洩露出去了。

**SETP 4**：輸入 `python youtube-to-text.py` 執行程式，結果遇到了錯誤如下：

看不懂錯誤訊息沒關係，直接貼到 ChatGPT。

原來是檔案大小的問題，這邊筆者將 10 秒的時間拉長到 100 秒，避免聲音切割過度。

```
from pytube import YouTube
from pydub import AudioSegment
import openai

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

# 切割音檔成多個小檔案
chunk_size = 100 * 1000  # 100 秒
chunks = [audio_file[i:i+chunk_size] for i in range(0, len(audio_file), chunk_size)]

# 使用 OpenAI 的 Audio API 將每個小檔案轉成文字，然後合併在一起
openai.api_key = "YOUR_API_KEY"
transcript = ""
for chunk in chunks:
    with chunk.export("temp.wav", format="wav") as f:
        result = openai.Audio.transcribe("whisper-1", f)
        transcript += result["text"]

# 印出轉換後的文字
print(transcript)

```

再輸入一次 `python youtube-to-text.py` 執行程式，接著就順利取得逐字稿啦！！！！

  大家可以發現裡面的文字不是百分百正確，品質還是需要自己把關的喔！

## 結論：要懂得組合應用

相比於上一篇，這篇文章的門檻比較高，像是多了：

1. 前往 OpenAI 官網，取得所需技術文件
2. 需要一定的英文閱讀能力（雖然你也可以請 ChatGPT 幫你翻譯）
3. 讓原有程式基礎上，疊加新應用的提問技巧

不過儘管我們取得了逐字稿，但這份逐字稿真的有點醜，閱讀起來有點困難。

這邊我們可以直接把他丟到 ChatGPT，請他幫忙分段落、整理摘要。

😀：「幫我改錯字、分段、加標點符號，讓內容更通順：`[逐字稿]`」

  大家可以發現裡面的文字不是百分百正確，品質還是需要自己把關的喔！

😀：「幫我擷取 300 字摘要：`[逐字稿]`」

> 摘要的內容有點赤裸 😅，不過當年筆者參加比賽的確就是為了出書。

不過手動貼到 ChatGPT 還是有點麻煩，[下一篇筆者就來分享如何透過 OpenAI 的 GPT-3.5 模型](https://deanlin.net/blog/202304/youtube-summary-gpt-whisper-api/)，幫我們整理逐字稿內容吧！

延伸閱讀：[讓我們一起掌握 AI 帶來的紅利](https://dean-lin.medium.com/list/ddcc9c53a4ac)

**想將 AI 導入自己的職場與生活嗎？歡迎[購買筆者出版的新書](https://www.books.com.tw/products/0011004216)**，幫助您**在 AI 的浪潮中站穩腳步，並在未來的職場競爭中佔據優勢！**

  [https://www.books.com.tw/products/0011004216](https://www.books.com.tw/products/0011004216)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202303/python-youtube-transcript-whisper/
