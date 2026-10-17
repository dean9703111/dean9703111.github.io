# Redmine 超詳細安裝教學：Redmine 4.1.1 on Ubuntu 16.04

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202102/redmine-installation-ubuntu/
- 發布：2021-02-18
- 主題：團隊合作
- 系列：Redmine 專案管理
- 標籤：Redmine、Project Management Tool、安裝教學、Ubuntu Server、專案管理
- 摘要：專案管理的工具非常之多，但是 jira、notion、clickup、trello 這類的專案管理都需要付錢，目前公司採用 Teams 的 planner、Lists 又不符合實際使用情境；之前有短暫的導入 testlink，不過那個 20 年前的 UI…

專案管理的工具非常之多，但是 jira、notion、clickup、trello 這類的專案管理都需要付錢，目前公司採用 Teams 的 planner、Lists 又不符合實際使用情境；之前有短暫的導入 testlink，不過那個 20 年前的 UI 設計很快就勸退大家。基於上述原因決定嘗試使用 Redmine 這個專案管理工具，本篇文章會詳細說明安裝的步驟即遭遇過的問題，文章大綱如下：

```
一、安裝環境
二、安裝順序
  STEP 1：安裝 MySQL
  STEP 2：建立 Redmine 使用者帳號
  STEP 3：安裝 RVM
  STEP 4：安裝 Ruby
  STEP 5：安裝 Passenger and Nginx
  STEP 6：設定 Nginx
  STEP 7：在 MySQL 增加 redmine 的使用者
  STEP 8：安裝 Redmine
  STEP 9：安裝 Redmine 外掛
  STEP 10：設定 Redmine 寄信功能
三、備註

```

## 一、安裝環境

官方沒有建議的硬體規格，但是看到有 [文章分享](https://www.redmineup.com/pages/blog/self-hosted-on-in-the-cloud-how-to-choose-infrastructure-and-hardware-configuration-for-redmine) 最低硬體配置如下：

- Processor: Intel Celeron (min 300 MHz)
- RAM: 1–2 GB RAM
- HDD: 30–40 GB

但通常使用最低硬體配置得到的結果就是容易卡，很多人同時上線使用時會讓卡到讓你懷疑人生，下面是我安裝時的配置目前使用起來算是流暢給大家參考用：

- Processor: 使用 VMWARE 的虛擬單核心
- RAM: 8GB RAM
- HDD: 500 GB
- OS: ubuntu 16.04

## 二、安裝順序

> 操作時請注意角色權限(root、redmine)，否則安裝到一半會卡住

## STEP 1：安裝 MySQL

> 角色： root

```
1. 安裝 MySQL 套件
    ```
    apt install build-essential mysql-server libmysqlclient-dev imagemagick libmagickwand-dev
    ```
2. 安裝後驗證 MySQL Version
    ```
    mysql -u root -p
    ```

```

原始碼：[mysql_install.md](https://gist.github.com/dean9703111/b38188a5ef4141b93c01b319a80b371b)（GitHub Gist）

  看到類似上方的 MySQL 資訊就代表你安裝成功嚕！

## STEP 2：建立 Redmine 使用者帳號

> 角色： root

```
1. 建立 Redmine 使用者，資料放在 `/data/redmine`
    ```
    adduser --system \
        --shell /bin/bash \
        --gecos 'Redmine Administrator' \
        --group \
        --home /data/redmine redmine;
    ```
2. 建立 rvm group
    ```
    groupadd rvm
    ```

3. 允許 redmine 可以 sudo
    ```
    adduser redmine sudo
    ```

4. 設定 redmine 密碼
    ```
    passwd redmine
    ```

5. 將 redmine 使用者加入 rvm 群組
    ```
    usermod -a -G rvm redmine
    ```

```

原始碼：[add_redmine_user.md](https://gist.github.com/dean9703111/7443c1b56e1f7a585472834ea2ec65b0)（GitHub Gist）

## STEP 3：安裝 RVM

> 角色： redmine

```
1. 安裝gnupg2
    ```
    sudo apt install gnupg2
    ```
2. 安裝 GPG keys:
    ```
    gpg2 --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
    ```
3. 安裝 RVM
    ```
    \curl -sSL https://get.rvm.io | bash -s stable
    ```
4. 讓 rvm 指令生效
    ```
    source /data/redmine/.rvm/scripts/rvm
    ```
5. 確認安裝的RVM版本
    ```
    rvm --version
    ```

```

原始碼：[install_RVM.md](https://gist.github.com/dean9703111/c05ee64e18057de7a430df6b474ebd9e)（GitHub Gist）

  看到類似上方的 RVM 資訊就代表你安裝成功嚕！

## STEP 4：安裝 Ruby

> 角色： redmine

作者的 **RUBY_VERSION 為 2.3.8**，這裡安裝的版本請[參考 Redmine 的官方建議](https://www.redmine.org/projects/redmine/wiki/redmineinstall#Ruby-interpreter)，如果安裝其他的版本後續安裝 Redmine 時會發生錯誤

```
1. 檢查 rvm 必要的套件
    ```
    rvm requirements
    ```
2. 安裝指定的 ruby version
    ```
    rvm install ${RUBY_VERSION}
    ```
3. 將此版本設定為預設
    ```
    rvm use ${RUBY_VERSION} --default
    ```
4. 確認安裝的 Ruby 版本
    ```
    ruby --version
    ```

```

原始碼：[intsall_ruby.md](https://gist.github.com/dean9703111/26719eedd99c32afc8f8d5820e058078)（GitHub Gist）

  看到類似上方的 ruby 資訊就代表你安裝成功嚕！

## STEP 5：安裝 Passenger and Nginx

> 角色： root

```
1. 安裝 Passenger PGP key 和 HTTPS support for package manager
    ```
    apt install -y dirmngr gnupg
    ```
    ```
    apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 561F9B9CAC40B2F7
    ```
    ```
    apt install -y apt-transport-https ca-certificates
    ```
2. Add passenger APT repository
    ```
    sh -c 'echo deb https://oss-binaries.phusionpassenger.com/apt/passenger xenial main > /etc/apt/sources.list.d/passenger.list'
    ```
    ```
    apt update
    ```
3. 安裝 Passenger and NGINX
    ```
    apt install -y nginx-extras passenger
    ```

```

原始碼：[install_Passenger_Nginx.md](https://gist.github.com/dean9703111/b44c3fa2e4c3a95b133182431af9da3c)（GitHub Gist）

## STEP 6：設定 Nginx

> 角色： root

```
1. 編輯 Nginx 設定檔案
    ```
    vi /etc/nginx/nginx.conf
    ```
2. 移除此行註解#：`include /etc/nginx/passenger.conf;`
    ```conf
    ...
    ##
    # Phusion Passenger config
    ##
    # Uncomment it if you installed passenger or passenger-enterprise
    ##

    include /etc/nginx/passenger.conf;
    ```
3. 接下來要調整 Nginx 的網站設定，所以建議先備份原本的設定
    ```
    cp /etc/nginx/sites-available/default /etc/nginx/sites-available/default.orig
    ```
5. 編輯 Nginx 的網站設定
    ```
    vi /etc/nginx/sites-available/default
    ```
6. 調整網站的根目錄位置以及 Passenger 設定
    ```
    #root /var/www/html; #將原本的註解或是移除
    root /data/redmine/current/public;
    passenger_enabled on;
    client_max_body_size 10m;
    ```
7. 把 location 段落註解
    ```
    #location / {
            # First attempt to serve request as file, then
            # as directory, then fall back to displaying a 404.
    #       try_files $uri $uri/ =404;
    #}
    ```

```

原始碼：[set_nginx.md](https://gist.github.com/dean9703111/dc2d42e3059a7beff2912bbd34be52ea)（GitHub Gist）

完成上述步驟後再做以下驗證：

1. 更改目錄權限： `chown -R www-data /var/www`
2. 重啟 Nginx： `systemctl restart nginx`
3. 驗證 Passenger and NGINX: `/usr/bin/passenger-config validate-install`

4. 檢查 Ngninx 狀態與 Passenger process: `/usr/sbin/passenger-memory-stats`

## STEP 7：在 MySQL 增加 redmine 的使用者

> 角色： root

```

1. 登入 MySQL
    ```
    mysql -u root -p
    ```
3. 創建 redmine 資料庫
    ```
    CREATE DATABASE redmine CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ```
4. 創建 redmine 使用者，密碼記得自行調整
    ```
    CREATE USER 'redmine'@'localhost' IDENTIFIED BY 'password';
    ```
5. 給予使用者權限
    ```
    GRANT ALL PRIVILEGES ON redmine.* TO 'redmine'@'localhost';
    ```
6. 使權限生效
    ```
    FLUSH PRIVILEGES;
    ```
7. 離開 MySQL
    ```
    quit;
    ```

```

原始碼：[mysql_add_redmine_user.md](https://gist.github.com/dean9703111/f0d6fa77b683a40cec8d44e5be51e36c)（GitHub Gist）

## STEP 8：安裝 Redmine

> 角色： redmine

### 下載官方 image

```
1. 到[官方網站](https://www.redmine.org/projects/redmine/wiki/Download)了解最新的stable版本（筆者安裝版本為 4.1.1）
2. 前往放置 redmine image 的目錄
    ```
    cd /data/redmine
    ```
3. 下載官方 image 並解壓縮
    ```
    wget https://www.redmine.org/releases/redmine-4.1.1.tar.gz
    ```
    ```
    tar zxvf redmine-4.1.1.tar.gz
    ```
4. 建立連結資料夾 current
    ```
    ln -s redmine-4.1.1 current
    ```
5. 前往連結資料夾
    ```
    cd current
    ```

```

原始碼：[redmine_download.md](https://gist.github.com/dean9703111/523ff799ed15fd78787e5664bcd589e3)（GitHub Gist）

### 調整 redmine 裡面的資料庫設定

```
1. 將 redmine 的資料庫範例設定檔複製一份
    ```
    cp -pR config/database.yml.example config/database.yml
    ```
2. 編輯資料庫設定檔案
    ```
    vi config/database.yml
    ```
3.  調整 production 的資訊即可，內容填上 STEP7 建立的使用者資訊即可
    ```yml
    production:
      adapter: mysql2
      database: redmine
      host: localhost
      username: redmine
      password: "your password"
      # Use "utf8" instead of "utfmb4" for MySQL prior to 5.7.7
      encoding: utf8mb4
    ```

```

原始碼：[redmine_database_config.md](https://gist.github.com/dean9703111/9cb24f48fd8b8fa969c4138a12186a76)（GitHub Gist）

### 下載相關套件

```
1. 先確認當前資料夾為 `/data/redmine/current`
    ```
    pwd
    ```
2. 執行 gem, bundle 安裝依賴套件
    ```
    gem install bundler
    ```
    ```
    bundle install --without development test
    ```
    > 如果安裝過程中遇到錯誤可能是因為沒有安裝到 ruby 的套件
    > 1. 先安裝 `sudo apt-get install ruby-dev`
    > 2. 再執行一次 `bundle install --without development test` 確認有無報錯

```

原始碼：[redmine_install_package.md](https://gist.github.com/dean9703111/540d8de42ff10326664acdb6f29c08d4)（GitHub Gist）

### 初始化 redmine

```
1. 先確認當前資料夾為 /data/redmine/current
    ```
    pwd
    ```
2. 用 Rake 啟動 redmine
    ```
    bundle exec rake generate_secret_token
    ```
3. Migrate Database Schema(初始化資料庫)
    ```
    RAILS_ENV=production bundle exec rake db:migrate
    ```
4. Load Initial Data(下面會請你選擇語言，預設為英文，語言後續都能再做調整)
    ```
    RAILS_ENV=production bundle exec rake redmine:load_default_data
    ```
5. 用 Nginx 重啟 Redmine
    ```
    sudo systemctl restart nginx
    ```

```

原始碼：[redmine_init.md](https://gist.github.com/dean9703111/79b903f7c4f1f22821aa91bbb21dca06)（GitHub Gist）

接下來你可以瀏覽 `http:<server_ip>` ，如果順利的話可以看到 Redmine 登入畫面如下：

  預設 `admin/admin` 登入

## STEP 9：安裝 Redmine 外掛

> 角色： redmine

Plugin 安裝方式雷同，下面以 `Redmine Dashboard 2`為範例安裝

```
1. 先進入 redmine 的 plugin 資料夾
    ```
    cd /current/plugins
    ```
2. 下載 plugin，最新的 release 請參考[官方](https://github.com/jgraichen/redmine_dashboard)
    ```
    wget https://github.com/jgraichen/redmine_dashboard/archive/v2.9.0.tar.gz
    ```
3. 解壓縮檔案
    ```
    tar zxvf v2.9.0.tar.gz
    ```
4. 移除壓縮檔
    ```
    rm v2.9.0.tar.gz
    ```
6.  安裝相關套件
    ```
    bundle install
    ```
7. 將套件相關 DB 進行 migrate 到 production
    ```
    rake db:migrate RAILS_ENV=production
    ```
8. 將套件安裝至 production
    ```
    bundle exec rake redmine:plugins:migrate RAILS_ENV=production
    ```
9. 安裝完畢後重啟讓設定生效
    ```
    sudo systemctl restart nginx
    ```

```

原始碼：[redmine_plugin.md](https://gist.github.com/dean9703111/d625c9f43d9667e086f3eb73328becb6)（GitHub Gist）

  安裝成功後的外掛程式會直接出現喔～

## STEP 10：設定 Redmine 寄信功能

> 角色： redmine

如果沒做這個步驟是寄不出信的，需要做下面的調整即可

  設定前在：網站管理>設定>電子郵件提醒選項。會出現的畫面

```
1. 進入 redmine 設定的資料夾
    ```
    cd /current/config
    ```
2. 將 redmine 的設定範例設定檔複製一份
    ```
    cp -pR configuration.yml.example configuration.yml
    ```
3. 編輯 redmine 設定檔案
    ```
    vi configuration.yml
    ```
4. 調整 production 的資訊即可，下面用 Gmail 作為範例
    ```
    production:
      delivery_method: :smtp
      smtp_settings:
        enable_starttls_auto: true
        address: "smtp.gmail.com"
        port: 587
        domain: "smtp.gmail.com" # 'your.domain.com' for GoogleApps
        authentication: :plain
        user_name: "email"
        password: "email pw"
     ```
5. 設定完畢後重啟生效
    ```
    sudo systemctl restart nginx
    ```

```

原始碼：[redmine_email.md](https://gist.github.com/dean9703111/120f7f2368b770899f5cacdc8344e61a)（GitHub Gist）

  網站管理>設定>電子郵件提醒選項的右下角會有“寄送測試郵件的按鈕”提供測試喔

  收到的測試郵件會長這個樣子

## 三、備註

1. 如果進入網頁時畫面沒有顯示應該是因為防火牆問題，可以用下面指令嘗試：`sudo ufw allow http` 、`sudo ufw allow https`
2. 參考資源：[Redmine 安裝篇：Redmine 4.0.x on Ubuntu 16.04](https://rickhw.github.io/2019/05/11/Redmine/Install-Redmine40x-on-Ubuntu1604/)、[Redmine Plugins and Themes](https://rickhw.github.io/2017/03/18/Redmine/Redmine-Plugins/)、[HowTo Install Redmine on Ubuntu step by step](https://www.redmine.org/projects/redmine/wiki/HowTo_Install_Redmine_on_Ubuntu_step_by_step)

```
安裝完後如果想進一步了解如何使用 Redmine，可以參考以下文章：
👍 如何用 Redmine 做專案管理＆體驗心得
👍 Redmine Plugins & Themes 介紹：那些我成功擴充的外掛
👍 用 Redmine 做專案管理 — 管理者不可不知的設定
👍 用 Redmine 做專案管理 — 一般用戶不可不知的基礎功能
👍 Redmine 教育訓練 — 依角色權限說明

```

### 🙂 讀完這篇文章後

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202102/redmine-installation-ubuntu/
