# Git 指令大全

## 1.git clone [Repository URL]
##### 說明：複製遠端Repository 到local
##### 範例：git clone https://XXXX.OOOOO.GGGGG.git

## 2.git pull
##### 說明：更新遠端Repository到local Repository (Fetch+Merge)

## 3.git status
##### 說明：檢查當前Repository 的檔案狀態

## 4.git add [檔案或資料夾] or git add .
##### 說明：將檔案加入版控(通常都是執行git add . 將全部檔案加進版控)
##### 範例：
* @ git add .
* @ git add Test
* @ git add test.html

## 5.git commit -m ["提交說明內容"]
##### 說明：提交目前的異動，透過-m參數設定提交說明文字
##### 範例：git commit -m "first commit"

## 6.git push
##### 說明：將local的提交更新到遠端Repository(預設是master)

## 7.git push origin [BRANCH_NAME]
##### 說明：發佈至遠端指定的分支（Branch）
##### 範例：git push origin develop

## 8.git log
##### 說明：查看先前的 commit 記錄

## 9.git branch
##### 說明：查看分支

## 10.git branch [BRANCH_NAME]
##### 說明：建立分支
##### 範例：git branch develop

## 11.git checkout [BRANCH_NAME]
##### 說明：取出分支
##### 範例：git checkout developer

## 12.git checkout -b [BRANCH_NAME]
##### 說明：建立並跳到該分支
##### 範例：git checkout -b developer

## 13.git branch -D [BRANCH_NAME]
##### 說明：強制刪除指定分支（須先切換至其他分支再做刪除）
##### 範例：git branch -D developer

## 14.git reset --hard [HASH]
##### 說明：強制恢復到指定的 commit（透過 Hash 值）
##### 範例：git reset --hard 0bd2934

## 15.git checkout [HASH]
##### 說明：切換到指定的 commit（與 git checkout [BRANCH_NAME] 相同)。
##### 範例：git checkout 0bd2934

## 16.git branch -m <OLD_BRANCH_NAME> <NEW_BRANCH_NAME>
##### 說明：修改分支名稱
##### 範例：git branch -m develop developer