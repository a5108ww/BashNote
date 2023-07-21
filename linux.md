# 基本指令操作

* 所有指令操作都是在當前的目錄

---

## 1.移到特定目錄

* cd [路徑]
* 範例 :
* 目錄中有 A、B、C 三個目錄，A目錄中有一個D目錄，
* (1)要移到 A 目錄，指令為 => "cd ~ " + "cd A"
* (2)要移到 D 目錄，指令為 => cd A/D(如果當前目錄有shilvain目錄) or cd /A/D(如果當前目錄沒有shilvain目錄)
* 回到User的根目錄 cd 或 cd ~
* 回到上一層目錄 cd ..

## 2.顯示當前目錄的路徑

* pwd

## 3.顯示當前目錄的內容

* ls

## 4.建立目錄

* mkdir [目錄名稱]
* 範例 : mkdir test
* 一次新增多層目錄
* 範例 : mkdir -p test/test1/test2

## 5.刪除空目錄

* rmdir [目錄名稱]

## 6.刪除目錄

* rm -r (會一層一層詢問是否刪除)
* rm -d 直接刪除目錄
* rm -i 刪除前會詢問
* rm -f 強制刪除

刪除檔案
rm -f [FILENAME]

## 6.複製檔案

* 將上個目錄的特定檔案複製到當前目錄
cp ../linuxgsm.sh linuxgsm.sh

* 把當前目錄的特定檔案複製到子層目錄中
cp linusgsm.sh ./steam/linuxgsm.sh

## 修改檔案內容
vi [FILENAME]
1.按下a 或 i 進入編輯模式
2.打完或改完內容，按下esc退出編輯模式
3.輸入:,
4.輸入:wq

## 7.列出當前目錄中，特定目錄、檔案建立時間

* ls -ld [目錄名稱]

## 8.修改系統日期

* rm -rf /etc/localtime  (刪除目前時區)
* ln -s /usr/share/zoneinfo/Asia/Taipei /etc/localtime (更換時區)
(先cd / 到root 根目錄)

## 登出
* exit

## 使用root 身分登入
sudo su
sudo
## 執行exe檔

curl -sqL "https://steamcdn-a.akamaihd.net/client/installer/steamcmd_linux.tar.gz" | tar zxvf -

## 登入系統
* su -
* su
* su - [USERNAME]

## 執行.sh檔
* ./steamcmd.sh

## 檢查目錄權限
ls -l

## 變更目錄權限
chmod 770 rpm

## 新增使用者
useradd [USERNAME]

## 新增使用者群組
grouadd [GROUPNAME]

## 把現有使用者加進群組
usermod -a -G [GROUPNAME] [USERNAME]

## 新增使用者順便加進舊群組
useradd -G [GROUPNAME] [USERNAME]

## 刪除User
* userdel -r [USERNAME]

## 設定User 密碼
* passwd [USERNAME]

## 檢查User 所在群組
* groups [USERNAME]

## 變更User對特定檔案的權限
chown [USERNAME]:[GROUPNAME] [FILENAME 或目錄名]
範例 : chown vhserver:rootuser rpm

* 遇到bad ELF interpreter 問題
原因 => 64位系統中安裝了32位程式
解決方法 => yum install glibc.i686

* 再繼續執行./steamcmd.sh

sudo dnf install nmap-ncat

# 安裝 Valheim Server

Server Config 路徑 =>
lgsm/config-default/config-lgsm/vhserver/_default.cfg

## Valheim Server ID 896660

## 下載Sudo
Debian
apt-get install sudo -y

RHEL/CentOS
yum install sudo -y

## 安裝wget
sudo yum install wget
apt install wget

## 安裝curl
yum install curl

## 安裝epel (為了安裝dnf)(VM預設應該是會有這個package)
yum install epel-release -y

## 安裝dnf
yum install dnf

## 安裝git
sudo dnf install git-all

## 安裝 Oh My Zsh
sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"

## 更改 Vhserver 底下的設定檔

--------------------------------------------------------------------------------------------------
## 常見壓縮及解壓縮
.tar
打包：tar cvf FileName.tar DirName
解包： tar xvf FileName.tar

.gz
壓縮：gzip FileName
解壓1：gunzip FileName.gz
解壓2：gzip -d FileName.gz

.tar.gz
壓縮：tar zcvf FileName.tar.gz DirName
解壓：tar zxvf FileName.tar.gz

.bz2
壓縮： bzip2 -z FileName
解壓1：bzip2 -d FileName.bz2
解壓2：bunzip2 FileName.bz2

.tar.bz2
壓縮：tar jcvf FileName.tar.bz2 DirName
解壓：tar jxvf FileName.tar.bz2

.bz
壓縮：unkown
解壓1：bzip2 -d FileName.bz
解壓2：bunzip2 FileName.bz

.tar.bz
壓縮：unkown
解壓：tar jxvf FileName.tar.bz

.Z
壓縮：compress FileName
解壓：uncompress FileName.Z

.tar.Z
壓縮：tar Zcvf FileName.tar.Z DirName
解壓：tar Zxvf FileName.tar.Z

.tgz
壓縮：unkown
解壓：tar zxvf FileName.tgz


.tar.tgz
壓縮：tar zcvf FileName.tar.tgz FileName
解壓：tar zxvf FileName.tar.tgz

.zip
壓縮：zip FileName.zip DirName
解壓：unzip FileName.zip

.rar
壓縮：rar e FileName.rar
解壓：rar a FileName.rar

.lha
壓縮：lha -a FileName.lha FileName
解壓：lha -e FileName.lha

### 備註
* 開Centos的Linux 作業系統，系統必須包含以下的工具
1.sudo
2.wget
3.curl
4.dnf