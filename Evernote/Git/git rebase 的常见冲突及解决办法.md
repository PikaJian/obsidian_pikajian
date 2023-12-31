**1. git cherry-pick与git rebase**        进行两个独立分支的代码合并时有两个比较常用的命令，分别是**git cherry-pick <commit-id>**和**git rebase**。        cherry-pick主要可以用于拷贝某个单独的patch，它的灵活性更大，而rebase主要用于整个分支的一次性合并。git rebase的使用语法为：

`git rebase <upstream-branch-name> <to-branch-name>`

执行上述命令的过程为：

1. 切换到to-branch分支；
2. 将to-branch中比upstream-branch多的commit先撤销掉，并将这些commit放在一块临时存储区（.git/rebase）；
3. 将upstream-branch中比to-branch多的commit应用到to-branch上，此刻to-branch和upstream-branch的代码状态一致；
4. 将存放的临时存储区的commit重新应用到to-branch上；
5. 结束。        执行完上述第3步后，to-branch的代码状态已经改变，接着执行第4步时则可能会产生合并冲突。

**2. 合并冲突的解决办法**        解决合并冲突几个常见的办法是：

1. 手动编辑冲突文件，手动删除或者保留冲突的代码；
2. 对于“both added”、“both deleted”、“both modified”等类型的冲突，若想完整地保留某一方的修改可以执行git checkout --ours(或者--theirs) <文件名>来选择想要保留的版本。需要注意的是由于git rebase 是先撤销再应用commit，所以这里的ours指的是upstream-branch，theirs指的是我们将要应用的临时commit。
3. 对于“added by us/them”、“deleted by us/them”等类型的冲突需要使用git rm <file-name>和git add <file-name>来删除/添加file。在此过程中需要特别注意谁是us，谁是them。        冲突解决完之后，使用git add <file-name>来标记冲突已解决，最后执行git rebase --continue继续。如果中间遇到某个补丁不需要应用，可以用下面命令忽略：

`git rebase --skip`        如果想回到rebase执行之前的状态，可以执行：

`git rebase --abort`**[git rebase 的常见冲突及解决办法](https://www.cnblogs.com/blackeyes/p/7636768.html)**