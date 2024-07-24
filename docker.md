# 指令

## 一、基本指令

### 刪除Container

* docker rm [Name]

### 新增Container

* docker run --name [CONTAINER_NAME] -p [主機port]:[CONTAINER_PORT] -d [IMAGE_NAME]

* docker run --name CentOS -p 8082:80 -d [IMAGE_NAME]

### 啟動Container

* docker start [CONTAINER_NAME]

### 停止Container

* docker stop [CONTAINER_NAME]

### 如果Container 考慮使用KILL指令

* docker kill [CONTAINER ID]

### 列出所有 Container

* docker ps

* => 抓出服務的CONTAINER ID : 97dc599628a7

### 列出所有的Images

* docker images

### 刪除Images
* docker rmi [IMAGE_ID]

### 使用目標 conatiner 現況建立 image

Docker commit [OLD_CONTAINER_NAME] [NEW_CONTAINER_NAME]

### 主機 docker 之間複製檔案

* docker cp test.txt [CONTAINER ID]:/usr/home/

* 將當前目錄的test.txt檔案複製到container容器的/usr/home目錄下

* docker cp [CONTAINER ID]:/usr/home/ [目錄名稱]

* 將container容器內檔案複製到宿主機


## 二、跑nginx Server

## 1.拉 docker image

* 指令 : docker pull nginx

## 2.部屬指定程式

* docker run [OPTION] [CONTAINER_NAME] [程式路徑] [IMAGE_NAME]

* 範例 : docker run -p 8011:80 --name Web1 -v C://Users/a5108/Desktop/git專案/docker/eginx:/usr/share/nginx/html -d nginx


## 三、跑 asp.net core Server

* 範例 : docker run -p 8011:80 --name web2 -v C://Users/a5108/Desktop/git專案/Web/LoginManager:/usr/share/nginx/html -d mcr.microsoft.com/dotnet/sdk:3.1

* 範例 : docker run -p 8081:80 --name web1 -v C://docker/web/openweb:/usr/share/nginx/html -d 87a94228f133

* 範例 : docker run -p 8082:80 --name web2 -v C://docker/web/test:/usr/share/nginx/html -d 87a94228f133

---

## 四、跑 CentOS 7docker

### 1.拉image

* docker pull centos:centos7

### 2.運行容器

* docker run -itd --name mycentos centos:centos7

### 3.進入OS

* docker exec -it mycentos /bin/bash

## 五、主機與linux container之間 複製檔案

## 參考資源

* https://www.cnblogs.com/yyfh/p/11613763.html

* https://ithelp.ithome.com.tw/articles/10186431

* https://blog.gtwang.org/linux/docker-commands-and-container-management-tutorial/

nginx container ID => 199a9c07adf1
docker start Web1

## 六、建立Sql Server 的容器

* docker pull mcr.microsoft.com/mssql/server
* docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=1qaz@WSX3edc" -p 1433:1433 --name sqlserver -h sqlserver -d 80bdc8efc889

## 七、Asp.Net Core 跑在docker 裡面

### 編譯 dokcerfile

* 先cd 到dockerfile所在的目錄

* docker build -t [要產生的映象檔名稱] .

* docker run -d --name [容器名稱] --rm -p 8081:80 [上面映像檔的名稱]

範例：docker run -d --name backcoresite --rm -p 8081:80 backcore

## 八、把容器打包成image

### 提交容器的變更 docker commit [CONTAINER_NAME]

* 範例：docker commit sqlserver

### 下指令 docker images，以確認新建image 的imageID

### docker tag [新建的ImageID] [取新的ImageName]

* 範例：docker tag 308c3889961e shilvain/linux_sqlserver:v1
* 範例：docker tag 308c3889961e shilvain/shilvainbackcore:v1

### 推送上Hub

* 範例：docker push shilvain/linux_sqlserver:v1

## 九、本機docker image 部屬至nas

docker build -t shilvain/shilvainbackweb:v13 .

----現有docke image 打包成tar檔----
docker save -o C:\Users\a5108\OneDrive\桌面\shilvain\dockerimages\authapi.tar shilvain/ginderauthserverapi:v8
docker save -o C:\Users\a5108\OneDrive\桌面\shilvain\dockerimages\authweb.tar shilvain/ginderauthserverbackend:v4

個人後台
docker save -o C:\Users\a5108\OneDrive\桌面\shilvain\dockerimages\backweb.tar shilvain/shilvainbackweb:v12
docker save -o C:\Users\a5108\OneDrive\桌面\shilvain\dockerimages\backweb_v13.tar shilvain/shilvainbackweb:v13

----將tar檔推到nas上----
docker --tls -H="tcp://192.168.0.9:2376" load --input "C:\Users\a5108\OneDrive\桌面\shilvain\dockerimages\authapi.tar"
docker --tls -H="tcp://192.168.0.9:2376" load --input "C:\Users\a5108\OneDrive\桌面\shilvain\dockerimages\authweb.tar"

個人後台
docker --tls -H="tcp://192.168.0.9:2376" load --input "C:\Users\a5108\OneDrive\桌面\shilvain\dockerimages\backweb.tar"
docker --tls -H="tcp://192.168.0.9:2376" load --input "C:\Users\a5108\OneDrive\桌面\shilvain\dockerimages\backweb_v13.tar"

----控制遠端nas container----
docker --tls -H="tcp://192.168.0.9:2376" run -d --name motocycle -p 9000:80 moto

docker --tls -H="tcp://192.168.0.9:2376" --version # 查詢遠端 nas docker 版本號





