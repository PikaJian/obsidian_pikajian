# Branch

## 

## **Git 建立 Remote Branch 的相關指令操作**

Git 遠端 Branch 的建立、操作、使用、刪除方式.

### **Git 建立 遠端 Branch**

1. git clone git@[github.com](http://github.com):user/project.git
2. cd project
3. **git checkout -b new\_branch** # 建立 local branch
4. **git push -u origin new\_branch** # 建立遠端 branch (將 new\_branch 建立到遠端)
5. git fetch
6. vim index.html # 修改
7. git commit -m 'test' -a # commit
8. git push
9. 註: new\_branch 要換成你要的 branch name, 以上所有 new\_branch 都要對應著修改成同樣名稱.

### **Git 使用 遠端 Branch**

1. git clone git@[github.com](http://github.com):user/project.git
2. cd project
3. git branch -r # 看遠端有什麼 branch
4. **git checkout origin/new\_branch -b new\_branch** # 建立 local new\_branch 並與遠端連接
5. vim index.html # 修改
6. git commit -m 'test' -a # commit
7. git push
8. 註: new\_branch 要換成你要的 branch name, 以上所有 new\_branch 都要對應著修改成同樣名稱.

### **Git 刪除 遠端 Branch**

* git push origin :new\_branch # 刪除遠端的 branch
* git push origin --delete new\_branch # 刪除遠端的 branch

### **Git Branch 的 Merge**

1. git branch new\_branch # 建立 branch
2. git checkout new\_branch # 切到 new\_branch, git checkout -b new\_branch 可以同時建立 + 切換
3. vim index.html # 修改
4. git commit -m 'test' -a # commit
5. git checkout master # 切回 master
6. **git merge new\_branch** # 將 new\_branch merge 到 master
7. git branch -d new\_branch # 若砍不掉就用 -D

### **更新所有 Repository branch**

* git remote update

### **Git 兩人實際操作 Remote BRanch 範例**

1. user1 建立 remote branch：先建立 local branch 再 push 產生 remote branch
	1. git checkout -b new\_branch # local 建立 new\_branch
	2. ... git commit something ...
	3. git push origin new\_branch # 會自動建立新的 remote new\_branch
	4. git pull origin new\_branch # 拉線上新版下來
2. user2 合作開發，拉下 remote new\_branch 建立 local new\_branch
	1. git clone git@[github.com](http://github.com):user/project.git
	2. git co origin/new\_branch -b new\_branch # 連結 remote new\_branch 成 local new\_branch
	3. git push origin new\_branch # push 到 remote new\_branch
	4. git pull origin new\_branch # 拉線上新版下來
