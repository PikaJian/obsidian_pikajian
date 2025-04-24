# git rebase

## 還沒 push 前可以做的(壞)事

不同於 SVN 等中央式版本控制系統，Git 的 Commit 其實還在本地端，所以可以修改 commit logs!!

* 修改 commit 訊息，甚至內容
* 合併 commits
* 打散 commit 成多個 commits
* 調換 commits 順序

### reset (回到特定節點)

    跟 revert 不同，reset 是直接砍掉 commits 歷史記錄
    git reset e37c75787
    git reset HEAD^ (留著修改在 working tree)
    git reset HEAD^ --soft (修改放到 staging area)
    git reset HEAD^ --hard (完全清除)
    

### 快速修正上一個 Commit

上一個commit有typo，想要重新commit

    1. git reset HEAD^
    2. 修正
    3. git commit -am “message”
    

可以簡化成

    1. 修正
    2. git commit -a --amend
    

### rebase (重新 commit 一遍)

    git rebase -i e37c7578
    
    跳出編輯器，出現：
    
    pick 048b59e first commit
    pick 995dbb3 change something
    pick aa3e16e changed
    
    # Rebase 0072886..1b6475f onto 0072886
    #
    # Commands:
    #  p, pick = use commit
    #  r, reword = use commit, but edit the commit message
    #  e, edit = use commit, but stop for amending
    #  s, squash = use commit, but meld into previous commit
    #  f, fixup = like "squash", but discard this commit's log message
    #  x, exec = run command (the rest of the line) using shell
    #
    # If you remove a line here THAT COMMIT WILL BE LOST.
    # However, if you remove everything, the rebase will be aborted.
    

rebase 跟 reset 都是危險的操作，因為它們是_改寫_歷史歷史，一不小心資料就不見囉。沒把握的話，操作前最好先用_branch_保存下來。

### 第四種合併方式: rebase

rebase 調整分支點 (僅適用於 local branch)

1. 從要被合併的 branch 頭重新分支
2. 將目前 branch 的 commits 記錄一筆一筆重新 apply/patch 上去

等於砍掉重練 commit log，僅適合還沒分享給別人的 local branch。

> 已經 push 分享出去的 commits 記錄，請千萬不要再修改記錄 push 出去!!

### 集大成：rebase + merge 的 完美 合併法

假設我們要將 feature branch 合併回主幹 develop

原因

* feature branch 很亂，不時 merge 與主幹同步
* feature branch 有 typo，commit 訊息想改
* feature branch 有些 commits 想合併或拆開

作法

* 先在 feature branch 做 git rebase develop -i
* (反覆整理直到滿意) git rebase 分岔點 -i
* 在從 develop branch 做 git merge feature –no-ff

實際操作可以參考這份錄影：[Git rebase 和 merge 合併操作示範錄影](http://ihower.tw/blog/archives/6704/)

注意事項

* 必須要加 –no-ff 才會有 merge commit。不然會是 fast-forward。
* rebase 之後的 feature branch 就不要再 push 出去了
* 如果有遠端的 feature branch，合併完也砍掉
* 不求一次 rebase 到完美，不然中間的 conflict 會搞混。
* 可以一次改點東西就 rebase 一次，然後開個臨時的 branch 存檔起來，再繼續 rebase 自己直到滿意為止。
