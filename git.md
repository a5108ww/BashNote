# Git 指令大全

## 1.第一次下載遠端Repository到local
##### 範例：git clone https://XXXX.OOOOO.GGGGG.git

## 2.更新遠端Repository到local(Fetch+Merge)
##### 範例：git pull origin master

## 3.檢查當前Repository 的檔案狀態
#### 範例：git status

## 4.將檔案加入版控(通常都是執行git add . 將全部檔案加進版控)
##### 範例：
* @ git add .
* @ git add Test
* @ git add test.html

## 5.提交目前的異動，透過-m參數設定提交說明文字
##### 範例：git commit -m "first commit"

## 6.將local的提交更新到遠端Repository
##### 範例：git push origin master

## 7.發佈至遠端指定的分支（Branch）
##### 範例：git push origin develop

## 8.查看先前的 commit 記錄
##### 範例：git log

## 9.查看分支
##### 範例：git branch

## 10.建立分支
##### 範例：git branch develop

## 11.切換分支
##### 範例：git checkout developer
##### 範例：git switch master
##### 範例：git switch origin/dev(加上 origin/ 代表會在local建立local branch)

## 12.建立並切換到該分支
##### 範例：git checkout -b developer

## 13.強制刪除指定分支（須先切換至其他分支再做刪除）
##### 範例：git branch -D developer

## 14.強制恢復到指定的 commit（透過 Hash 值）
##### 範例：git reset --hard 0bd2934

## 15.切換到指定的 commit
##### 範例：git checkout 0bd2934

## 16.修改分支名稱
##### 範例：git branch -m develop developer (develop 舊名稱，developer新名稱)

## 17.同步遠端Repository的提交紀錄到地端
##### 範例：git fetch origin

## 18.還原檔案
##### 範例：git checkout .  (還原所有檔案)
##### 範例：git checkout 1.txt (還原特定檔案)

## 18.print 現在的指向的分支
##### 範例：cat .git/HEAD

## 19.合併分支
##### 範例：git merge dev
##### 步驟：
###### 1.切換要合併的分支，git checkout master
###### 2.執行合併dev至master語法，git merge dev
###### 3.推上遠端，git push