# Ubuntu Server基本環境

- 作者：林鼎淵（Dean Lin）
- 網址：https://deanlin.net/blog/202006/ubuntu-server-basic-setup/
- 發布：2020-06-15
- 主題：技術教學
- 標籤：Ubuntu、Ubuntu Server、Servers、SSH
- 摘要：這篇文章是以部署網頁為導向的環境設定

這篇文章是以部署網頁為導向的環境設定

## 1、ssh的安裝(為了ssh連線)

```
sudo apt-get install ssh -y

```

**如果遇到：package ‘ssh’ has no installation candidate的錯誤，要記得更新**

```
apt-get update

```

## 2、生成金鑰

```
ssh-keygen
cd .ssh
mv id_rsa.pub authorized_keys

```

## 3、修改配置文件，/etc/ssh/sshd_config

```
sudo vi /etc/ssh/sshd_config

```

將# AuthorizedKeysFile %h/.ssh/authorized_keys，把前面的#去掉即可，這個是允許ssh登入的檔案路徑

註:如果server有安裝gitlab，可能會被拒絕連線，要再加上這個設定:StrictModes no

修改完記得要將ssh重啟才會生效：

```
sudo service ssh restart

```

## 4、windows上啟動putty，進行如下配置

（1）Session-Logging-Hostname:填上你的linux的ip

（2）Windows －Translation － 在下拉選單裡選上UTF－8，這裡不設定，登入後將會出現中文亂碼。

（3）Connection- Data — Auto login username:填上你登入ubuntu時用的使用者名稱。

（4）Connection- SSH-Auth-Private key file for authentication:選上id_rsa.ppk；

## 5、設定 Authorized_keys 信任清單

```
vi authorized_keys

```

進去後貼上rsa key

## 6、確定能用ssh key登入後再去修改配置文件

```
sudo vi /etc/ssh/sshd_config

```

修改：PasswordAuthentication no 禁止使用者用密碼登入

之後記得重啟

```
sudo service ssh restart

```

## 7、 Ubuntu based GNU/Linux 上的防火牆

```
sudo apt-get install ufw
sudo ufw enable
sudo ufw allow from 192.168.0.0/12(你允許的網路環境)

```

## 8、每台的ubuntu default editor都改成vim

```
sudo update-alternatives — set editor /usr/bin/vim.basic

```

## 9、將ip改為static ip

先看新增的網卡名稱

```
cd /proc/sys/net/ipv4/conf

```

**Ubuntu 16作法：**

```
sudo vi /etc/network/interfaces
auto ens160
iface ens160 inet static
address 192.168.3.60
netmask 255.255.0.0
gateway 192.168.1.254
dns-nameservers 168.95.1.1 168.95.192.1

```

改完後

```
sudo /etc/init.d/networking restart

```

**Ubuntu 18作法：**

```
vi /etc/netplan/50-cloud-init.yaml

```

```
network:
    ethernets:
        enp0s3:
            addresses: [192.168.0.10/24]
            gateway4: 192.168.0.1
            nameservers:
              addresses: [8.8.8.8,8.8.4.4]
            dhcp4: no
    version: 2

```

原始碼：[50-cloud-init.yaml](https://gist.github.com/dean9703111/d0fcc16dd59df0e5152449c27cd6d1f9)（GitHub Gist）

改完後

```
sudo netplan try

```

**Ubuntu 20作法：**

```
sudo vi /etc/netplan/00-installer-config.yaml

```

```
# This is the network config written by 'subiquity'
network:
  ethernets:
    ens160:
      addresses: [your_ip_address/12]
      gateway4: your_gateway
      optional: true
      nameservers:
        addresses: [168.95.1.1,168.95.192.1,8.8.8.8,8.8.4.4]
      dhcp4: no
  version: 2

```

原始碼：[00-installer-config.yaml](https://gist.github.com/dean9703111/33b1639b9d52bae158f0c0eb363f025b)（GitHub Gist）

> PS.如果發現改成靜態網址後無法ssh登入，請把 addresses 的遮罩改為 `/12`

## 10、ubuntu新增使用者並設成 Sudo user

```
$ sudo adduser newuser
$ usermod -aG sudo newuser

```

如果要取消這位使用者的 sudoer 資格

```
$sudo deluser newuser sudo

```

如果要刪除這位使用者

```
sudo deluser — remove-home userNameHere

```

Add public key to allow remote SSH login for the new user

```
su — newuser
mkdir ~/.ssh
vim ~/.ssh/authorized_keys

```

查詢帳號有 sudoer 的權限

```
$ sudo cat /etc/group | grep sudo
$ sudo cat /etc/group | grep admin

```

## 11、Linux與Windows間傳送檔案

複製本機檔案到linux

```
pscp d:\path\file_name account@192.10.3.64:/path

```

## 12、LVM storage相關問題

因為ubuntu 18.4使用的時候，不知道設定為何默認初始硬碟只有4GB，如果要釋放出剩餘的硬碟空間，用以下指令即可

```
sudo lvextend -rl +100%FREE /dev/mapper/ubuntu — vg-ubuntu — lv

```

參考資源：

[**Ufw防火牆設定**](https://www.peterdavehello.org/2016/01/ubuntu-based-gnulinux-firewall-ufw-essential-config/)

[**如何在Ubuntu 16.04上使用PM2部署Node.js应用程序**](https://web.archive.org/web/20210411220744/https://www.howtoing.com/deploy-node-js-application-with-pm2-on-ubuntu)

[**windows下如何使用ssh遠端登入Linux**](https://tw.saowen.com/a/45fd6b0293c602372e720ea216d07c8ac0751f49c19e235126260221367818e6)

[**在Linux與Windows間傳送檔案 — 步驟教學**](https://loveamberbird.wordpress.com/2013/08/21/%E3%80%90%E7%AD%86%E8%A8%98%E3%80%91%E5%9C%A8linux%E8%88%87windows%E9%96%93%E5%82%B3%E9%80%81%E6%AA%94%E6%A1%88%E6%AD%A5%E9%A9%9F%E6%95%99%E5%AD%B8/)

[**win10如果要產生登入的SSH KEY**](https://stackoverflow.com/questions/31813080/windows-10-ssh-keys)

[**sudo被玩壞參考**](https://blog.csdn.net/youyudexiaowangzi/article/details/48729897)

[LVM storage釋放剩餘空間](https://askubuntu.com/questions/1090773/understanding-free-space-volumes-on-ubuntu%20https://blog.xuite.net/lichangying/wretch/214360626-%E5%A6%82%E4%BD%95%E5%8B%95%E6%85%8B%E6%94%BE%E5%A4%A7LVM+on+Linux)

---

關於作者：林鼎淵（Dean Lin），台灣的生成式 AI 企業內訓講師、外商資安公司軟體專家，也是生成式 AI 創新學院發起人。出版 7 本書（包含全台第一本 ChatGPT 應用專書），兩度獲得 iThome 鐵人賽佳作，累積上百場授課與顧問經驗。（https://deanlin.net/about/）
引用時請附上原文網址：https://deanlin.net/blog/202006/ubuntu-server-basic-setup/
