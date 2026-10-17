# [GPT-SoVITS]用 AI 快速複製你的聲音，搭配 Colab 免費入門

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202402/gpt-sovits-voice-cloning/
- 發布：2024-02-09（更新：2025-08-17）
- 主題：AI 實戰
- 標籤：GPT SoVITS、AI、AI Voice Generator、AI Voice Cloning、Colab
- 摘要：用過 GPT-SoVITS 後，我感覺到善良真的是一種選擇。 在這之前，筆者嘗試過 clone-voice、MockingBird 等複製聲音的 AI 工具。但他們英文的表現尚可，中文的結果真的慘不忍睹。儘管 GPT-SoVITS 在中文領域尚有進步空間，但已經足夠驚艷了。

**注意：本專案因這兩年相依套件改版太多，故無法順利啟用，有興趣與時間的朋友們，建議[參考官方文件](https://github.com/RVC-Boss/GPT-SoVITS)，目前 Colab 執行會遇到許多特殊狀況，如果有順利解決的朋友，歡迎留言分享您的經驗，相信會給更多人帶來幫助，感謝大家。**

用過 GPT-SoVITS 後，我感覺到善良真的是一種選擇。

在這之前，筆者嘗試過 clone-voice、MockingBird 等複製聲音的 AI 工具。但他們英文的表現尚可，中文的結果真的慘不忍睹。

儘管 GPT-SoVITS 在中文領域尚有進步空間，但已經足夠驚艷了（而且開源免費，上手難度低）。

**【免責聲明】：教學只是示範一種思路，沒有任何不良引導，請勿未經許可使用他人音訊資源用於個人學習外其他目的，使用軟體需遵守軟體相關條款協議。**

小提醒：本篇文章的操作時間約 30 分鐘，若對這個主題感興趣，建議先保存下來，或 [Follow](https://dean-lin.medium.com/subscribe) 我的部落格。

```
文章大綱

STEP 1: 安裝 GPT-SoVITS
STEP 2: 提供訓練的聲音素材
STEP 3: 檢查語音識別是否準確
STEP 4: 將訓練的資料格式化
STEP 5: 微調訓練
STEP 6: 推理，輸入文字讓 AI 生成聲音
 結語

```

## STEP 1: 安裝 GPT-SoVITS

考量到讀者所使用的作業系統不同，本篇文章會使用 Colab 來做示範，讓大家透過 Google 雲端資源輕鬆入門。

但如果你還是想要在自己的電腦安裝：

- **Windows**: 可以直接下載[官方整合包](https://huggingface.co/lj1995/GPT-SoVITS-windows-package/resolve/main/GPT-SoVITS-beta.7z?download=true)，解壓縮後點擊 go-webui.bat 即可啟動。
- **Mac**: 儘管官網文件中有 Mac 版本的教學，但坑多到讓人懷疑人生，就算改用 Docker 還是會遇到一堆問題，因此建議你直接使用 Colab 操作。

下面連結是筆者調整後，實測可直接運行的版本： [https://colab.research.google.com/drive/1Yff7mRQgKeqXJ4oB8DyT_onigVIBo48l?usp=sharing](https://colab.research.google.com/drive/1Yff7mRQgKeqXJ4oB8DyT_onigVIBo48l?usp=sharing)

2025.01.09 更新：

筆者 Github 的 GPT-SoVITS 部分套件已經被 Python 視為過舊，因此有附上官方最新版本的 Github 連結：git clone [https://github.com/RVC-Boss/GPT-SoVITS](https://github.com/RVC-Boss/GPT-SoVITS) ，但他們更新頻率高，實際能否跑需要一些運氣（囧）

  小提醒：Colab 需要有 Google 帳號才能使用，並且每天有用量限制，如果用量到極限的話，可以考慮付費升級（或換一個 Google 帳號操作）。

如果順利看到上圖畫面，點擊「執行階段 → 全部執行」就會開始下載、安裝程式。

過程中會跳出各種警告、授權的警示窗，大家可以自行評估風險；不過原則上要全部允許才能執行，不然根本無法與 Google Drive 溝通。

等資源下載完後，將「啟動程式」的面板展開，你會在下方看到「Running on public URL: xxx」的網址，點擊後就會進入網頁的操作畫面。

如果能看到下面的網頁，恭喜你！安裝成功了！

## STEP 2: 提供訓練的聲音素材

我們需要準備一段聲音素材給 AI 訓練，建議符合以下規格：

- 1~2 分鐘的聲音素材（多一些更好）
- 目前僅支援「中文」訓練
- 僅接受「.wav」檔
- 盡可能提供無背景噪音的聲音

準備好對應的素材後，我們把視角拉回 Colab，在左側的工作區點擊「drive → MyDrive」，在自己的雲端硬碟新增一個「voice_files」的資料夾，並在下面建立兩個資料夾：

- raw: 原始聲音
- slicer: 分割後的聲音

然後我們就可以把準備好的聲音檔拖拉到「raw」這個資料夾底下。

接著我們複製聲音檔 & 分割聲音的資料夾路徑

- /content/drive/MyDrive/voice_files/raw
- /content/drive/MyDrive/voice_files/slicer

把路徑貼到 GPT-SoVITS 網頁上後，點擊「Start audio slicer」，等待「Audio slicer output log」出現「切割结束」

切割完成的文件就會在「slicer」資料夾下。

完成後把分割聲音的資料夾路徑(/content/drive/MyDrive/voice_files/slicer)貼到 GPT-SoVITS 的「Batch ASR (Chinese only) input folder」下。接著點擊「Start batch ASR」，等「ASR output log」出現「ASR任务完成」即可（需要一段時間）。

完成的檔案在「GPT-SoVITS」的「output/asr_opt」資料夾下。

我們把他移動到 voice_files 路徑下方便後續操作。

## STEP 3: 檢查語音識別是否準確

複製 slicer_list 的路徑（/content/drive/MyDrive/voice_files/asr_opt/slicer.list）貼到「.list annotation file path」，並勾選「Open labelling WebUI」。

我們接下來要為語音識別好的文件打上標記，回到 Colab，打開新的 Public 連結（原本 GPT_SoVITS 的畫面不要關掉）。

在這個界面中，你可以檢查語音識別是否正確，並對其優化。

## STEP 4: 將訓練的資料格式化

回到一開始的 GPT-SoVITS 網頁，點擊「1-GPT-SoVITS-TTS」填上 Model 名稱。

接著在「Text labelling file」放上打標檔案的路徑（/content/drive/MyDrive/voice_files/asr_opt/slicer.list），並於「Audio dataset folder」放上音頻切割後的資料夾路徑（/content/drive/MyDrive/voice_files/slicer）。

點擊「Start one-click formatting」，等待右側出現「一键三连进程结束」訊息。

## STEP 5: 微調訓練

點擊「1B-Fine-tuned traning」，依序點擊「Start SoVITS training」、「Start GPT traing」，這個階段也需要耐心等待。

完成的當案會出現在「GPT_weights」和「SoVITS_weights」資料夾，建議下載一份，畢竟走到這一步很累啊（目前儲存的資料夾再重新連結後會刷新消失，如果無法在時間內下載完，下次連線時從 STEP 5 開始就好）。

## STEP 6: 推理，輸入文字讓 AI 生成聲音

終於到最後一步了，進入「1C-inference」分頁，點擊「refreshing model paths」後，就會看到剛剛訓練好的模型。

接著點擊「Open TTS inference WEBUI」。

回到 Colab 點擊 public 連結。

就可以透過網頁輸入文字讓 AI 生成聲音了，下圖是對介面的簡易說明。

生成好的音頻可以直接下載，大家可以點擊試聽看跟我原本的聲音有幾成相似。

[YouTube 影片：AI複製自己的聲音](https://www.youtube.com/watch?v=V9CoMR94q_I)

下面放上我真實的聲音來比對，我個人覺得音色非常接近。

[YouTube 影片：自己的聲音](https://www.youtube.com/watch?v=_QutJpFlV7w)

## 結語

如果有人能跟著我的文章做一遍，那你真的是一個狠人，因為這篇教學連我自己都寫到快崩潰了。

其實我一直猶豫是否要發布這篇教學文，因為過去曾有詐騙集團靠 AI 複製聲音來語音詐騙。

但後來想了想，只要有心，這項技術並沒有那麼難；讓更多人知道這項技術的存在，也許還能減少大家被詐騙的可能性。

技術本身沒有善惡之分，存粹看用的人如何操作；像這個工具對有錄影片需求的朋友來說就是一大福音，只要準備好文字稿，就能請 AI 用我們的聲音來表達。

如果你跟筆者一樣容易講話卡螺絲，但又有錄不露臉教學影片的需求，相信善用工具能大幅提升你的工作效率。

延伸閱讀：[讓我們一起掌握 AI 帶來的紅利](https://dean-lin.medium.com/list/ddcc9c53a4ac)

**想將 AI 導入自己的職場與生活嗎？歡迎[購買筆者出版的新書](https://www.books.com.tw/products/0011004216)**，幫助您**在 AI 的浪潮中站穩腳步，並在未來的職場競爭中佔據優勢！**

  [https://www.books.com.tw/products/0011004216](https://www.books.com.tw/products/0011004216)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202402/gpt-sovits-voice-cloning/
