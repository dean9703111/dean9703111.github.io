# 嫌Google Form太醜？那就把他當後台吧XD

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202006/google-form-as-backend/
- 發布：2020-06-05（更新：2023-05-05）
- 主題：技術教學
- 標籤：Google Forms Tutorial、Google Sheets、HTML5 Development、GitHub、jQuery
- 摘要：Google Form & Google sheet & HTML & github 結合應用

為什麼撰寫這篇文章

常常使用Google Form當問卷的你是否覺得Google Form有點太過制式化，或者說讓人覺得用Google Form有點誠意不足

但自己撰寫網頁問卷還要花錢買(租)一台Server放網頁跟建立資料庫，才能讓大家進來這個網頁填寫問卷，我覺得這樣一點都不划算，因為我只是想要讓這個問卷看起來有質感，不是Google Form而已XD

## 於是我就誕生一個想法

為什麼不要讓Google Form乖乖當後台就好，我放一個靜態網頁到Github上面就能省去過去在Server上面的花費

文章目標

- 如何把Google Form的功能抽出來在一般靜態網頁使用
- 可全客製化自己心目中漂亮的表單(內文使用Bootstrap 4為範例)
- 讓你了解撰寫網頁會遇到的問題(謹以本範例為說明)

**本文章使用到的技術：**

- 前端:static web(Bootstrap 4)
- 後端:Google Form
- 資料庫:Google Sheets

Demo區讓你搶先體驗

[DEMO網頁](https://dean9703111.github.io/customer_feedback_form/index.html) (看完文章後你可以自己客製化一個)

[Google Form](https://docs.google.com/forms/d/e/1FAIpQLSeijRuLDQE62gbapDYgd_3pbmg7GyWY1CWPVeaEK-TiLDKzPw/viewform) (他其實才是執行動作的本體)

[Google Sheets](https://docs.google.com/spreadsheets/d/1aZyvHnajaVjdNBaUT1Md-Y9nYW8O_rdPnMmN5WFrdO4/edit?usp=sharing) (資料最終儲存的地方)

專案原始碼

[Github Source code](https://github.com/dean9703111/customer_feedback_form) (如果實作上遇到問題可以參考)

## STEP 1 / 7: 建立Google Form表單

先建立一個[Google Form](https://www.google.com.tw/intl/zh-TW/forms/about/)，選取個人的即可：

您可以按造自己的需求建立一個簡單的表單：

## STEP 2 / 7: 抽出Google Form表單裡面的元素

建立好表單後進入預覽模式

接下來進入網頁的開發者模式(按下F12)，選擇Elements檢視網頁元素，然後搜尋”entry”，這些name的標籤是之後會使用的參數欄位，請把他們複製起來

2021/2/26 更新，朋友提醒新版的 ”entry” 已經轉移到了 <input type=”hidden” name=”entry.xxx" value=””> 的位置，大家操作時注意一下

  位置轉移惹

輸入必填欄位資料按下送出(submit)按鈕後複製上面的網址，這個網址是之後網頁用來發送訊息的url

  網址會長的像:[https://docs.google.com/forms/這些是你自己的網址/formResponse](https://docs.google.com/forms/u/0/d/e/1FAIpQLSeijRuLDQE62gbapDYgd_3pbmg7GyWY1CWPVeaEK-TiLDKzPw/formResponse)

## STEP 3 / 7: 選一個你喜歡的表單模板

在這裡我提供一些大家可以直接使用的[表單模板](https://www.mockplus.com/blog/post/bootstrap-form-template)，大家可以去找自己喜歡的，接下來我直接說明細節，完整的程式碼可以在[這裡下載](https://github.com/dean9703111/customer_feedback_form)

因為我設計的這份問卷裡面需要填寫：姓名、email、評分、您的建議，並且需要要以form表單傳送，所以會有html結構如下(您的結構可能跟我有些許不同)：

```
<form id="google-form" class="contact100-form validate-form">
        <span class="contact100-form-title">
          餐後回饋
        </span>

        <label class="label-input100" for="first-name">姓名 *</label>
        <div class="wrap-input100 validate-input" data-validate="姓名為必填">
          <input id="name" class="input100" type="text" placeholder="王大明">
          <span class="focus-input100"></span>
        </div>

        <label class="label-input100" for="email">email</label>
        <div class="wrap-input100">
          <input id="email" class="input100" type="text" placeholder="Eg. example@email.com">
          <span class="focus-input100"></span>
        </div>

        <label class="label-input100" for="email">評分(最高五分) *</label>
        <div class="wrap-input100 validate-input" data-validate="煩請評分">
          <select class="form-control" id="score" style="border: 0px;">
            <option>1</option>
            <option>2</option>
            <option>3</option>
            <option>4</option>
            <option>5</option>
          </select>
        </div>

        <label class="label-input100" for="message">您的建議</label>
        <div class="wrap-input100" data-validate="Message is required">
          <textarea id="message" class="input100" placeholder="Write us a message"></textarea>
          <span class="focus-input100"></span>
        </div>

        <div class="container-contact100-form-btn">
          <button class="contact100-form-btn">
            送出
          </button>
        </div>
      </form>

```

原始碼：[form_elements](https://gist.github.com/dean9703111/fe560b714307b9f4e52df4114ebd270c)（GitHub Gist）

但請記得在<form>、<input>、<select>這些tag裡面設定id，這樣後續在jquery裡面才能抓到相應的元素

## STEP 4 / 7: 將Google Form表單裡抽出的元素放進去HTML網頁

Google Form建立好了，網頁元素也都建立完成後就是將兩者bind在一起，以下是用jquery抓取html元素，並使用ajax將這個訊息送出，程式細節情看註解

記得在要 把 jquery 引入<script>裡面，我朋友卡在這個問題很久 XD

```
<script>
    $('#google-form').submit(function (e) {
      //在這裡我們要先擋掉form默認事件
      e.preventDefault();

      if ($('#name').val() && $('#score').val()) {//需要先確認必填欄位是否填寫
        $.ajax({
          // url為Google Form按下submit的aciotn
          url: "https://docs.google.com/forms/d/e/1FAIpQLSeijRuLDQE62gbapDYgd_3pbmg7GyWY1CWPVeaEK-TiLDKzPw/formResponse",
          crossDomain: true,//解決跨網域CORS的問題
          data: {// entry.xxxxx 這些需要填寫您表單裡面的值，與其相互對應
            "entry.2040133429": $('#name').val(),
            "entry.304089069": $('#email').val(),
            "entry.207113062": $('#score').val(),
            "entry.1310220291": $('#message').val()
          },
          type: "POST", //因為是要進行insert的動作，故事用POST
          dataType: "JSONP",
          complete: function () {
            //完成後把這些欄位清空
            $('#name').val('')
            $('#email').val('')
            $('#score').val('')
            $('#message').val('')
            //最後跳轉到感謝的頁面
            window.location.replace("thank.html");
          }
        });
      }
    });
  </script>

```

原始碼：[bind-html-googeForm](https://gist.github.com/dean9703111/726895bb8b6902f16b26e8254b954e03)（GitHub Gist）

## STEP 5 / 7:測試是否正常運作

你可以直接開啟html檔(點擊兩下就會跳出網頁了)，輸入數值後按下送出(如果你使用我的程式會看到如下成果)

接下來去Google Form的Responses區域看看是否資訊正常進入

到這裡就能確認你的程式正常運作了(撒花～)

## STEP 6 / 7:登入github帳戶，並上傳你的專案

github帳戶的創建應該很多地方都有教學，這裡我就簡化這個步驟，說明最簡單上傳的方式

登入帳號後選擇Repositories，在按下New新增一個

填寫一些基礎資訊，這裡需要特別注意的是你必須要選擇Public，不然你這個專案是無法讓大家在網路上觀看的

  選擇上傳存在的檔案

  直接把整個資料夾拖拉進去就好了

## STEP 7 / 7:將專案開放給大家連線

接下來要對這個專案做一些設定，先點擊最右側的Settings

找到Source的位置，選擇master branch後，上方就會出現連結了

不過要注意這個連結是這個專案的根目錄，如果要導向到你的網頁需要設定路徑喔：

EX:

根目錄 [https://dean9703111.github.io/test/](https://dean9703111.github.io/test/)

實體檔案位置 [https://dean9703111.github.io/test/](https://dean9703111.github.io/test/)**index.html**

做到這裡順利的話就能分享連結給朋友炫耀自己的表單啦～

如果在實作的過程中遇到任何問題歡迎在下方留言

**參考資源**：[Bootstrap form templete](https://colorlib.com/wp/template/contact-form-v17/)、[Bootstrap thank you page](https://codepen.io/JacobLett/pen/vyegPV)、

[Create websites using Google Forms](https://hackernoon.com/create-websites-using-google-forms-49a19cc8beb8)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202006/google-form-as-backend/
