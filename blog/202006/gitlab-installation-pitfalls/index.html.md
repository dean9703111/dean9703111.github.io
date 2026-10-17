# Gitlab 安裝以及我曾經遇到的坑

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202006/gitlab-installation-pitfalls/
- 發布：2020-06-09（更新：2023-05-05）
- 主題：技術教學
- 標籤：GitLab、Email、Nginx、NAS
- 摘要：Gitlab被各大企業廣泛地用於專案管理上，他可以追蹤每個project的進度設定milestone開sprint、分配不同team相互的權限，甚至在同一個project裡面也能有不同角色的權限區分，CI/CD等功能也能很方便地去實現。

Gitlab被各大企業廣泛地用於專案管理上，他可以追蹤每個project的進度設定milestone開sprint、分配不同team相互的權限，甚至在同一個project裡面也能有不同角色的權限區分，CI/CD等功能也能很方便地去實現。

因為過去在使用的路上遇到各式各樣的問題，我把相關經驗整合在這裡，希望看到文章的朋友可以順利跨過這些難關，如果我的文章幫你解決問題歡迎多按幾次拍手。

此Gitlab安裝時的ubuntu版本為16.4，指令如有不同，請自行google，如果你願意，也非常歡迎回覆在下方分享您的經驗。

建議硬體需求

RAM至少4GB，以百人公司規模來說建議8GB以上大家使用上會順暢很多(如果你用官方建議2GB可能會連安裝都卡住，這部分有親自體驗過)

## 安裝前置作業

- 因為git在連線時需要使用ssh連線進行安全性驗證 所以需要openssh-server ca-certificates
- gitlab支援寄信通知或是驗證功能所以需要信箱伺服器支援需要postfix

```
curl openssh-server ca-certificates postfix
sudo apt-get install curl openssh-server ca-certificates postfix

```

## 安裝gitlab

- gitlab下載目前運行版本指令

```
curl -s https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | sudo bash
sudo apt-get install gitlab-ce=11.1.4-ce.0

```

2. 單純下載封包

```
wget — content-disposition https://packages.gitlab.com/gitlab/gitlab-ce/packages/ubuntu/trusty/gitlab-ce_11.1.4-ce.0_amd64.deb/download.deb

```

建議要備份下載的deb檔案到自己的硬碟，如果日後災難復原時會用上，像我是備份到公司的NAS Server上面

## gitlab環境配置

```
sudo gitlab-ctl reconfigure

```

如果環境沒問題，上面這個指令會順順的過去；**但如果做了版本升級或是降級，所有的環境都可能會產生或大或小的bug**，官方並沒有提供一個很健全的解決方案，建議未來的使用者除非很有把握不然建議不要隨意升級。

筆者在之後新版的gitlab有更新成功，但是舊版直接升級會遇到許多問題整理部份如下

Gitlab 500 Error: PG::ConnectionBad:

[https://gitlab.com/gitlab-org/omnibus-gitlab/issues/1637](https://gitlab.com/gitlab-org/omnibus-gitlab/issues/1637)

/opt/gitlab/embedded/bin/bundle:23:in `<main>’

[https://gitlab.com/gitlab-org/omnibus-gitlab/issues/3185](https://gitlab.com/gitlab-org/omnibus-gitlab/issues/3185)

輸入以下指令可獲得版本資訊:

```
sudo gitlab-rake gitlab:env:info

```

## 設定對外網址

輸入下面指令後尋找external_url，將其修改成自己的url與開放的port

```
sudo vi /etc/gitlab/gitlab.rb

```

```
external_url 'https://domain.com'.  //你自己的url

registry_nginx['redirect_http_to_https'] = true   //是否自動轉址為https
mattermost_nginx['redirect_http_to_https'] = true
nginx['enable'] = true
nginx['redirect_http_to_https'] = true
nginx['redirect_http_to_https_port'] = 80
nginx['ssl_certificate'] = "/etc/your_path/ssl/gitlab.xxxxx.com.crt"   //如果你的gitlab為https請設定ssl的路徑
nginx['ssl_certificate_key'] = "/etc/your_path/ssl/gitlab.xxxxx.com.key"

```

原始碼：[gitlab.rb](https://gist.github.com/dean9703111/8cfb4e440691d49a32f37be832d27329)（GitHub Gist）

每次修改過這個檔記得要重新配置才會生效

```
sudo gitlab-ctl reconfigure

```

(到這一部已經可以使用gitlab了，但是如果要寄信相關還要做如下設定)

## 寄信相關設定

```
sudo vi /etc/gitlab/gitlab.rb

```

把下面這些貼到最下面後sudo gitlab-ctl reconfigure就生效了

```
//下面的是email信箱設定(此為gmail範例)
gitlab_rails['smtp_enable'] = true
gitlab_rails['smtp_address'] = "smtp.gmail.com"
gitlab_rails['smtp_port'] = 587
gitlab_rails['smtp_user_name'] = "your@gmail.com"
gitlab_rails['smtp_password'] = "your_password"
gitlab_rails['smtp_domain'] = "smtp.gmail.com"
gitlab_rails['smtp_authentication'] = "login"
gitlab_rails['smtp_enable_starttls_auto'] = true
gitlab_rails['smtp_tls'] = false
gitlab_rails['smtp_openssl_verify_mode'] = 'peer' # Can be: 'none', 'peer', 'client_once', 'fail_if_no_peer_cert', see http://api.rubyonrails.org/classes/ActionMailer/Base.html
gitlab_rails['smtp_ca_path'] = "/etc/ssl/certs"
gitlab_rails['smtp_ca_file'] = "/etc/ssl/certs/ca-certificates.crt"

```

原始碼：[gitlab_gmail.rb](https://gist.github.com/dean9703111/16b5ea24384f6fe888cd99722b832ba3)（GitHub Gist）

可下指令測試是否設定成功

```
sudo gitlab-rails console
Notify.test_email(‘email@example.com’, ‘test’, ‘test’).deliver_now

```

但是當gitlab的server有設定雙網卡（同時有內網以及外網）的時候就有可能出現email無法寄送的狀況，這個時候你要確定你外網的優先權必須要高於內網。

看mail錯誤log

```
vi /var/log/mail.log

```

改mail postfix設定

```
vi /etc/postfix/main.cf
postfix reload

```

## ssh對外設定

如果你對外網址的22 port已經被其他專案佔用了（ex:我公司），所以要將預設ssh的22 port改為轉址的223 port(或是其他你希望的)，一樣是在sudo vi /etc/gitlab/gitlab.rb設定，這樣在gitlab才能用ssh下載

```
gitlab_rails[‘gitlab_shell_ssh_port’] = 223

```

## nginx設定

```
nginx['enable'] = true
nginx['redirect_http_to_https'] = true
nginx['redirect_http_to_https_port'] = 80
nginx['ssl_certificate'] = "/etc/your_path/ssl/gitlab.xxxxx.com.crt"   //如果你的gitlab為https請設定ssl的路徑
nginx['ssl_certificate_key'] = "/etc/your_path/ssl/gitlab.xxxxx.com.key"

```

原始碼：[gitlab_nginx.rb](https://gist.github.com/dean9703111/d2485274a4609b5f8ceef16b2db507cc)（GitHub Gist）

## 讓Gitlab可通內網外網

在gitlab分配兩張網卡後需要完成以下步驟

先看新增的網卡名稱

```
cd /proc/sys/net/ipv4/conf

```

確認完網卡後將他們編輯加入

```
sudo vi /etc/network/interfaces
auto ethXXX
iface ethXXX inet dhcp

```

改完後

```
sudo /etc/init.d/networking restart

```

用ifconfig查看是不是抓到

設定路由可以透過閘道連通

sudo /sbin/route add 10.1.1.1(目標內網ip) gw 10.1.1.254(內網閘道) dev ens192(網卡)

設定NAS上此共享資料夾必須符合NFS設定可參閱這篇文章

[http://blog.xuite.net/tolarku/blog/495291815-Qnap+%E8%A8%AD%E5%AE%9A+NFS+%E5%88%86%E4%BA%AB%E8%A8%AD%E5%AE%9A+-+nfs+export](http://blog.xuite.net/tolarku/blog/495291815-Qnap+%E8%A8%AD%E5%AE%9A+NFS+%E5%88%86%E4%BA%AB%E8%A8%AD%E5%AE%9A+-+nfs+export)

確認是否可連通，並觀看有哪些資料夾

```
sudo apt install nfs-common
showmount -e 10.1.1.1(目標內網ip)

```

將NAS上的共享資料夾掛載到ubuntu上面

```
sudo mount -t nfs 10.1.1.1(目標內網ip):ServerGit /home/your_path/gitlab_backup

```

確認是否掛載

```
df -h

```

## gitlab定時自動備份到NAS的設定

在/etc/gitlab/gitlab.rb設定gitlab的備份路徑

```
gitlab_rails[‘backup_path’]=”/home/your_path/gitlab_backup/”

```

每個月1日創建本月的資料夾的script

```
now_month=”$(date +’%Y%m’)”
#echo “$now_month”
cd /home/your_path/gitlab_backup
mkdir $now_month

```

將產生的backup檔案轉移到月份資料夾底下的script,並刪除比較久的資料

```
mv /home/your_path/gitlab_backup/*.tar /home/your_path/gitlab_backup/$(date +’%Y%m’)
find /home/your_path/gitlab_backup/$(date +’%Y%m’) -name “*gitlab_backup.tar” -mtime +3 -exec rm -rf {} \;
find /home/your_path/gitlab_backup/$(date “+%Y%m” -d ‘1 month ago’) -name “*gitlab_backup.tar” -mtime +3 -exec rm -rf {} \;

```

排程設定

```
sudo crontab -e
1 0 1 * * sh /home/your_path/script/mkdir.sh
1 1 * * * /opt/gitlab/bin/gitlab-rake gitlab:backup:create
1 2 * * * sh /home/your_path/script/mv_to_month.sh

```

Gitlab 的學問博大精深，希望這篇文章讓初學者上手時更加輕鬆。

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202006/gitlab-installation-pitfalls/
