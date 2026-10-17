# 只要一張自拍照就能讓 AI 換臉整段影片！Mac M1 在本機安裝 Roop 教學

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202402/roop-face-swap-mac/
- 發布：2024-02-09
- 主題：AI 實戰
- 標籤：Roop、Face Swap、macOS、Apple Silicon、AI
- 摘要：原本我打算放上 Google Colab，但發現免費版執行的失敗率高得嚇人；因此寫了一篇 Mac AppleSilicon 的踩坑文件，讓大家可以輕鬆體驗這款 AI 換臉的工具（在自己電腦執行，無論效率還是便利性都高太多了）。

原本我打算放上 Google Colab，但發現免費版執行失敗率高得嚇人；因此寫了一篇 Mac AppleSilicon 的踩坑文件，讓大家可以輕鬆體驗這款 AI 換臉工具（在自己電腦執行，無論效率還是便利性都高太多了）。

文章開始前，我先總結一下體驗心得：「在 AI 的時代下，眼見不足為憑；這類換臉工具在使用上已經沒有技術壁壘，即使 0 基礎的小白也能在短時間上手。」

**【免責聲明】：教學只是示範一種思路，沒有任何不良引導，請勿將工具用於個人學習外其他目的，使用軟體需遵守軟體相關條款協議，[別忘記小玉已經在監獄了](https://web.archive.org/web/20231229190446/https://udn.com/news/story/7321/7672903)。**

  把我的臉合成到葉問上

> **小提醒**：Windows 要安裝的東西可以[參考這篇官方文章](https://github.com/s0md3v/roop/wiki/1.3-Setup-Windows)，其實跟 Mac 差不多。

## STEP1: 安裝 Miniconda

進入官網選擇與自己電腦相容的 pkg 檔下載: [https://docs.conda.io/en/latest/miniconda.html](https://docs.conda.io/en/latest/miniconda.html)

安裝完成後進入終端機（Terminal），輸入 `conda config –set auto_activate_base true` 初始化 Conda（若要關閉輸入 `conda config –set auto_activate_base false` 即可）。

再次打開終端機時，命名行前會有（base）

## STEP2: 下載 Roop 專案 & 安裝相關套件

進入終端機後，輸入 `git clone [https://github.com/s0md3v/roop](https://github.com/s0md3v/roop)` 下載專案。

下載完成後進入專案目錄，將下面的內容貼到「requirements.txt」文件（這些是要安裝的套件）。

```
--extra-index-url https://download.pytorch.org/whl/cu118

numpy==1.23.5
opencv-python==4.7.0.72
onnx==1.14.0
insightface==0.7.3
psutil==5.9.5
tk==0.1.0
pillow==9.5.0
customtkinter==5.1.3
torch==2.0.0
onnxruntime==1.15.0; sys_platform == 'darwin' and platform_machine != 'arm64'
onnxruntime-silicon==1.13.1; sys_platform == 'darwin' and platform_machine == 'arm64'
onnxruntime-gpu==1.15.0; sys_platform != 'darwin'
tensorflow==2.13.0rc1; sys_platform == 'darwin'
tensorflow==2.12.0; sys_platform != 'darwin'
opennsfw2==0.10.2
protobuf==4.23.2
tqdm==4.65.0

```

然後在終端機進入 roop 資料夾，輸入 `pip install -r requirements.txt` 安裝相關套件。

接著可以通過 [homebrew](https://brew.sh/zh-tw/) 安裝 ffmpeg。

`brew install ffmpeg`

## STEP3: 執行 roof 換臉程式

在 roof 資料夾下執行 `python run.py`，等個幾秒鐘就會有如下的圖形介面出現（第一次執行會需要下載 500 多 MB 的資源，需等待一段時間），操作非常簡單，主要功能可以參考下圖說明。

不過大多數人應該第一次執行時不會那麼順利，像筆者就遇到 tk 的錯誤如下：

後面用 `brew search tk` 來搜尋，才發現 `[python-tk@3.10](mailto:python-tk@3.10)` 沒裝，執行 `brew install [python-tk@3.10](mailto:python-tk@3.10)` 安裝後就正常了。

接著又遇到 torch、torchvision 相容性問題，這部分我看考他們的[版本對照表](https://github.com/pytorch/vision#installation)重裝就正常了

```
pip uninstall torch torchvision
pip install torch==2.0 torchvision==0.15

```

## STEP4: 加強出圖效果與品質

作者表示 [Apple Silicon 支援 CoreML 加速](https://github.com/s0md3v/roop/wiki/2.-Acceleration#coreml-apple)，執行下面指令安裝對應套件。

```
pip uninstall onnxruntime onnxruntime-silicon
pip install onnxruntime-silicon==1.13.1

```

之後執行換臉程式時結尾加上「 — execution-provider coreml」即可加速，另外想增加品質可以在結尾加上「 — frame-processor face_swapper face_enhancer」即可，**完整啟動換臉的指令如下**：

`python run.py –frame-processor face_swapper face_enhancer –execution-provider coreml`

## STEP5: 看看換臉後的圖片與影片品質

這邊我先拿自己的照片換到葉問的電影照看看。

  圖片來源：葉問，僅作為教育及研究使用

其實我覺得效果是不錯的，會有違和感是因為我太胖了，跟身體不太吻合。

下面我再拿自己的照片跟韓團影片合成看看，不知道會不會一秒變歐巴（因為影片中有兩個人，所以我將 Many Faces 打開）。

  影片來源: [Levitating / dsomeb Choreography & Dance](https://www.youtube.com/watch?v=Ly-Wm-hKoYU&ab_channel=dsomeb)

其實我覺得結果的違和感不大（自己在說），不過因為我提供的照片有眼鏡，所以有些畫面會閃。

以上就是今天 AI 換臉的教學分享，筆者的電腦為 Mac M1 Max 32GB RAM，實測下來建議選擇 15 秒內的影片來做換臉，超過的話 RAM 可能會撐不住（超過可透過分段生成處理）。

> 最後提醒一下大家，這個工具真的很強大，門檻也很低；我們可以拿來學習、娛樂，但千萬不要拿來做犯法的事情。

延伸閱讀：[AI 實戰](https://deanlin.net/categories/ai/)

**想將 AI 導入自己的職場與生活嗎？歡迎[購買筆者出版的新書](https://www.books.com.tw/products/0011004216)**，幫助您**在 AI 的浪潮中站穩腳步，並在未來的職場競爭中佔據優勢！**

  [https://www.books.com.tw/products/0011004216](https://www.books.com.tw/products/0011004216)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202402/roop-face-swap-mac/
