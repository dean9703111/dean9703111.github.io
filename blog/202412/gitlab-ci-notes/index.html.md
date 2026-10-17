# GitLab CI 實戰（踩坑）筆記

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202412/gitlab-ci-notes/
- 發布：2024-12-02（更新：2024-12-04）
- 主題：技術教學
- 標籤：GitLab、GitLab CI、Dockerfile Example、YAML、Example
- 摘要：GitLab CI 是 GitLab 的內建工具，每次 Push 都會觸發一次腳本執行，內容通常涵蓋測試、編譯、部署等一系列自定義內容。 這篇文章會分享設計上要注意的細節，以及實戰過程踩過的坑。

GitLab CI 是 GitLab 的內建工具，每次 Push 都會觸發一次腳本執行，內容通常涵蓋測試、編譯、部署等一系列自定義內容。

這篇文章會分享設計上要注意的細節，以及實戰過程踩過的坑，不會特別說明基礎資訊，想更深入理解的朋友可以參考[官網文件](https://docs.gitlab.com/ee/ci/)；如果讀者有更好的方案，或對名詞有更精準的定義，歡迎留言分享。

```
文章大綱

 GitLab CI 資料夾結構設計
 在 .gitlab/.default.gitlab-ci.yml 定義要執行 pipeline 的 Branchs
 根據專案需求定義對應的 Stages
 想在不同 stage 傳遞資料時，artifacts 跟 cache 的使用時機
 讓 stage 下的 job 並行執行 or 順序執行
 notify 階段建議要有的資訊
 Dockerfile 多階段構建
 其他的坑（套件安裝、相容性、單元測試）

```

## GitLab CI 資料夾結構設計

```
Project
├── .gitlab-ci.yml
└── .gitlab
    ├── stages
    │   ├── 1.xxx.yml
    │   └── 2.xxx.yml
    ├── .default.gitlab-ci.yml
    └── docker-bake.hcl

```

下面說明一下資料夾結構下各檔案要負責的任務：

- **.gitlab-ci.yml**: 定義有要執行的 stages，以及參考的 yml 檔案（ex: `.gitlab/.default.gitlab-ci.yml`）。
- **.gitlab**: 存放 GitLab 設定檔的資料夾。
- **.gitlab/stages**: 存放各 stage 的資料夾，讓邏輯更容易維護。
- **.gitlab/stages/xxx.yml**: 定義 stage 下各 jobs 要執行的任務。
- **.gitlab/.default.gitlab-ci.yml**: 用來定義默認的設定，避免重複性的工作（ex: 設定通用的 `variables`、定義要執行 pipeline 的 branchs）。
- **.gitlab/docker-bake.hcl**: 定義 Build Docker Image 的資訊（ex: 支援多平台、傳遞參數）。

## 在 .gitlab/.default.gitlab-ci.yml 定義要執行 pipeline 的 Branchs

僅管我們期待每次 Push 一版都會觸發 pipeline，但有時也會有一些例外狀況（ex: 需要高頻率 Push 測試一些特殊功能）。

以下面的設計來說，就是僅允許「merge_request_event」以及「release/、hotfix/、feature/*」，還有 Default Branch（ex: master、develop）會觸發 pipeline。

```
workflow:
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
    - if: '$CI_COMMIT_BRANCH =~ /^release\/.*$/'
    - if: '$CI_COMMIT_BRANCH =~ /^hotfix\/.*$/'
    - if: '$CI_COMMIT_BRANCH =~ /^feature\/.*$/'
    - if: '$CI_COMMIT_BRANCH == $CI_DEFAULT_BRANCH'

```

## 根據專案需求定義對應的 Stages

筆者習慣設計下面 5 個 Stages:

- **init**: 在這個階段做套件安裝的任務（ex: npm install）。
- **analysis**: 如果 init 順利才會進入這個階段，會在這裡進行單元測試(test)、Coding Style 檢查(lint)，並將對應的 report 保存到 artifacts 當中。
- **build_image**: 如果前面都通過才會 Build Docker Image，並將 $TARGET_IMAGE:$TAG 保存到 artifacts/tags.txt 當中（要在 notify 階段使用）。
- **notify**: 我將它設定為 always 執行，用來傳送 pipeline 成功/失敗的訊息到對應的 Chanel（ex: Zoom、Teams、Line）。
- **deploy**: 根據實際需求設計部署流程。

## 想在不同 stage 傳遞資料時，artifacts 跟 cache 的使用時機

上個 stage 做完的事情，在下個 stage 是不會知道細節的。

想要傳遞資訊就需要透過 artifacts 或 cache，而這兩者的用途不太一樣：

- **artifacts**: 記錄這次 pipeline 獨有的資訊，像是 test report、image_id 這類的，建議要設定對應的 expire_in 時間，否則資料會永久保存消耗硬碟空間。

```
analysis:
  artifacts:
    paths:
      - reports
    expire_in: 1 day

```

- **cache**: 這個專案經常使用的資料，比如 node_modules 這類保存套件的資料夾。

```
analysis:
  cache:
    key: "${CI_PROJECT_PATH_SLUG}"
    paths:
      - node_modules

```

## 讓 stage 下的 job 並行執行 or 順序執行

如果沒有特別設計，stage 下的 job 默認都是並行執行的（比如 unit test & lint 可以同時進行）；如果想要順序執行，可以透過 needs 來設定，範例如下：

```
job1:
  stage: build
  script:
    - echo "Executing job1"
job2:
  stage: build
  script:
    - echo "Executing job2"
  needs:
    - job1  # 等待 job1 完成後執行

```

## notify 階段建議要有的資訊

GitLab CI 通常要跑幾分鐘的時間（通常跟 unit test 多寡有關），所以要設計 notify 的機制來傳遞資訊，建議包含的內容如下：

- **project**: $PROJECT_NAME，方便了解執行的專案。
- **branch**: $CI_COMMIT_REF_NAME，了解哪個 branch 更新了。
- **result**: 會包含成功與失敗的詳細資訊。
- **pipeline_link**: $PIPELINE_LINK，出問題時點擊連結可以看到最直接的資訊。
- **image_id**: 從 artifacts 裡面讀取。

## Dockerfile 多階段構建

下面用一個範例來說明如何用多階段構建（multi-stage build）來減少 Docker Image 大小並提高效率（`COPY –from=builder` 的作用是從第一階段複製文件到第二階段）。

```
# 第一階段：使用 Node.js 安裝套件，並進行打包
FROM node:23.3-alpine as builder
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm install
COPY . .
RUN npm run build

# 第二階段：使用 Nginx 部署靜態文件
FROM nginx:alpine3.20
COPY --from=builder /app/dist /usr/share/nginx/html
WORKDIR /usr/share/nginx/html
COPY ./build/nginx/default.conf /etc/nginx/conf.d/default.conf

```

透過這個方案，最終的 Image 只有靜態文件跟 Nginx，不包含 Node.js 開發工具；不但減少 Image 大小，還減少了潛在攻擊面向。

## 其他的坑（套件安裝、相容性、單元測試）

- **套件無法順利安裝**: 如果套件是經由私有庫來安裝的，就需要設定對應的網路環境；如果在 Dockerfile 裡面安裝，請記得將 `.npmrc` 這類設定 COPY 到對應的位置。
- **qemu-user-static 相容性錯誤**: 有些專案需要指定對應的 platform 才能順利 build 出來（Dockerfile from 的 image 可能僅支持 arm 或 amd）。
- **單元測試錯誤突然暴增**: 如果在 local 環境測試起來正常，但在 GitLab CI 時出現錯誤，那通常是「非同步」的問題，可以先用 `await` 拉長等待時間，並搭配 log 來釐清狀況。

有段時間沒發純技術的文章了，希望能幫助到有相關需求的朋友們，也歡迎大家分享自己在 GitLab CI 的使用經驗。

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202412/gitlab-ci-notes/
