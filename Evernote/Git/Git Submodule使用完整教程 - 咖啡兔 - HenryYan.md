# Git Submodule使用完整教程 - 咖啡兔 - HenryYan

## 1.对于公共资源各种程序员的处理方式

每个公司的系统都会有一套统一的系统风格，或者针对某一个大客户的多个系统风格保持统一，而且如果风格改动后要同步到多个系统中；这样的需求几乎每个开发人员都遇到，下面看看各个层次的程序员怎么处理：

假如对于系统的风格需要几个目录：css、images、js。

		**普通**程序员，把最新版本的代码逐个复制到每个项目中，如果有N个项目，那就是要复制**N x 3**次；如果漏掉了某个文件夹没有复制…@（&#@#。
	
		**文艺**程序员，使用Git Submodule功能，执行：**git submodule update**，然后冲一杯咖啡悠哉的享受着。
	

* * *

引用一段《[Git权威指南](http://book.douban.com/subject/6526452/)》的话： 项目的版本库在某些情况虾需要引用其他版本库中的文件，例如公司积累了一套常用的函数库，被多个项目调用，显然这个函数库的代码不能直接放到某个项目的代码中，而是要独立为一个代码库，那么其他项目要调用公共函数库该如何处理呢？分别把公共函数库的文件拷贝到各自的项目中会造成冗余，丢弃了公共函数库的维护历史，这显然不是好的方法。

## 2.开始学习Git Submodule

“工欲善其事，必先利其器”！

既然文艺程序员那么轻松就搞定了，那我们就把过程一一道来。

**说明**：本例采用**两个项目**以及**两个公共类库**演示对submodule的操作。因为在一写资料或者书上的例子都是一个项目对应1～N个lib，但是实际应用往往并不是这么简单。

### 2.1 创建Git Submodule测试项目

#### 2.1.1 准备环境

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3 | `➜ henryyan@hy-hp  ~`  `pwd`<br>`/home/henryyan`<br>`mkdir` `-p submd``/repos` |

创建需要的本地仓库：

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5 | `cd` `~``/submd/repos`<br>`git --git-``dir``=lib1.git init --bare`<br>`git --git-``dir``=lib2.git init --bare`<br>`git --git-``dir``=project1.git init --bare`<br>`git --git-``dir``=project2.git init --bare` |

初始化工作区：

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2 | `mkdir` `~``/submd/ws`<br>`cd` `~``/submd/ws` |

#### 2.1.2 初始化项目

初始化project1：

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8<br>9<br>10<br>11<br>12<br>13<br>14<br>15<br>16<br>17<br>18<br>19<br>20<br>21<br>22<br>23<br>24<br>25<br>26<br>27<br>28<br>29<br>30<br>31<br>32<br>33 | `➜ henryyan@hy-hp  ~``/submd/ws`  `git clone ..``/repos/project1``.git`<br>`Cloning into project1...`<br>`done``.`<br>`warning: You appear to have cloned an empty repository.`<br><br>`➜ henryyan@hy-hp  ~``/submd/ws`  `cd` `project1`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1` `git:(master)` `echo` `"project1"` `> project-infos.txt`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1` `git:(master) ✗` `ls`<br>`project-infos.txt`<br><br>`➜ henryyan@hy-hp  ~``/submd/ws/project1` `git:(master) ✗ git add project-infos.txt`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1` `git:(master) ✗ git status`<br>`# On branch master`<br>`#`<br>`# Initial commit`<br>`#`<br>`# Changes to be committed:`<br>`#   (use "git rm --cached <file>..." to unstage)`<br>`#`<br>`#   new file:   project-infos.txt`<br>`#`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1` `git:(master) ✗ git commit -m` `"init project1"`<br>`[master (root-commit) 473a2e2] init project1`<br> `1 files changed, 1 insertions(+), 0 deletions(-)`<br> `create mode 100644 project-infos.txt`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1` `git:(master) git push origin master`<br>`Counting objects: 3,` `done``.`<br>`Writing objects: 100% (3``/3``), 232 bytes,` `done``.`<br>`Total 3 (delta 0), reused 0 (delta 0)`<br>`Unpacking objects: 100% (3``/3``),` `done``.`<br>`To` `/home/henryyan/submd/ws/``..``/repos/project1``.git`<br> `* [new branch]      master -> master`<br>`<``/file``>` |

初始化project2：

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8<br>9<br>10<br>11<br>12<br>13<br>14<br>15<br>16<br>17<br>18<br>19<br>20<br>21<br>22<br>23<br>24<br>25<br>26<br>27<br>28<br>29<br>30<br>31<br>32<br>33<br>34 | `➜ henryyan@hy-hp  ~``/submd/ws/project1` `cd` `..`<br>`➜ henryyan@hy-hp  ~``/submd/ws`  `git clone ..``/repos/project2``.git`<br>`Cloning into project2...`<br>`done``.`<br>`warning: You appear to have cloned an empty repository.`<br><br>`➜ henryyan@hy-hp  ~``/submd/ws`  `cd` `project2`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project2` `git:(master)` `echo` `"project2"` `> project-infos.txt`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project2` `git:(master) ✗` `ls`<br>`project-infos.txt`<br><br>`➜ henryyan@hy-hp  ~``/submd/ws/project2` `git:(master) ✗ git add project-infos.txt`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project2` `git:(master) ✗ git status`<br>`# On branch master`<br>`#`<br>`# Initial commit`<br>`#`<br>`# Changes to be committed:`<br>`#   (use "git rm --cached <file>..." to unstage)`<br>`#`<br>`#   new file:   project-infos.txt`<br>`#`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project2` `git:(master) ✗ git commit -m` `"init project2"`<br>`[master (root-commit) 473a2e2] init project2`<br> `1 files changed, 1 insertions(+), 0 deletions(-)`<br> `create mode 100644 project-infos.txt`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project2` `git:(master) git push origin master`<br>`Counting objects: 3,` `done``.`<br>`Writing objects: 100% (3``/3``), 232 bytes,` `done``.`<br>`Total 3 (delta 0), reused 0 (delta 0)`<br>`Unpacking objects: 100% (3``/3``),` `done``.`<br>`To` `/home/henryyan/submd/ws/``..``/repos/project2``.git`<br> `* [new branch]      master -> master`<br>`<``/file``>` |

#### 2.1.3 初始化公共类库

初始化公共类库lib1：

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8<br>9<br>10<br>11<br>12<br>13<br>14<br>15<br>16<br>17<br>18 | `➜ henryyan@hy-hp  ~``/submd/ws`  `git clone ..``/repos/lib1``.git`<br>`Cloning into lib1...`<br>`done``.`<br>`warning: You appear to have cloned an empty repository.`<br>`➜ henryyan@hy-hp  ~``/submd/ws`  `cd` `lib1`<br>`➜ henryyan@hy-hp  ~``/submd/ws/lib1` `git:(master)` `echo` `"I'm lib1."` `> lib1-features`<br>`➜ henryyan@hy-hp  ~``/submd/ws/lib1` `git:(master) ✗ git add lib1-features`<br>`➜ henryyan@hy-hp  ~``/submd/ws/lib1` `git:(master) ✗ git commit -m` `"init lib1"`<br>`[master (root-commit) c22aff8] init lib1`<br> `1 files changed, 1 insertions(+), 0 deletions(-)`<br> `create mode 100644 lib1-features`<br>`➜ henryyan@hy-hp  ~``/submd/ws/lib1` `git:(master) git push origin master`<br>`Counting objects: 3,` `done``.`<br>`Writing objects: 100% (3``/3``), 227 bytes,` `done``.`<br>`Total 3 (delta 0), reused 0 (delta 0)`<br>`Unpacking objects: 100% (3``/3``),` `done``.`<br>`To` `/home/henryyan/submd/ws/``..``/repos/lib1``.git`<br> `* [new branch]      master -> master` |

初始化公共类库lib2：

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8<br>9<br>10<br>11<br>12<br>13<br>14<br>15<br>16<br>17<br>18<br>19 | `➜ henryyan@hy-hp  ~``/submd/ws/lib1` `git:(master)` `cd` `..`<br>`➜ henryyan@hy-hp  ~``/submd/ws`  `git clone ..``/repos/lib2``.git`<br>`Cloning into lib2...`<br>`done``.`<br>`warning: You appear to have cloned an empty repository.`<br>`➜ henryyan@hy-hp  ~``/submd/ws`  `cd` `lib2`<br>`➜ henryyan@hy-hp  ~``/submd/ws/lib2` `git:(master)` `echo` `"I'm lib2."` `> lib2-features`<br>`➜ henryyan@hy-hp  ~``/submd/ws/lib2` `git:(master) ✗ git add lib2-features`<br>`➜ henryyan@hy-hp  ~``/submd/ws/lib2` `git:(master) ✗ git commit -m` `"init lib2"`<br>`[master (root-commit) c22aff8] init lib2`<br> `1 files changed, 1 insertions(+), 0 deletions(-)`<br> `create mode 100644 lib2-features`<br>`➜ henryyan@hy-hp  ~``/submd/ws/lib2` `git:(master) git push origin master`<br>`Counting objects: 3,` `done``.`<br>`Writing objects: 100% (3``/3``), 227 bytes,` `done``.`<br>`Total 3 (delta 0), reused 0 (delta 0)`<br>`Unpacking objects: 100% (3``/3``),` `done``.`<br>`To` `/home/henryyan/submd/ws/``..``/repos/lib2``.git`<br> `* [new branch]      master -> master` |

### 2.2 为主项目添加Submodules

#### 2.2.1 为project1添加lib1和lib2

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8<br>9<br>10<br>11<br>12<br>13<br>14<br>15<br>16<br>17<br>18<br>19<br>20<br>21<br>22<br>23<br>24<br>25<br>26<br>27<br>28<br>29<br>30<br>31 | `➜ henryyan@hy-hp  ~``/submd/ws/lib2` `git:(master)` `cd` `..``/project1`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1` `git:(master)` `ls`<br>`project-infos.txt`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1` `git:(master) git submodule add ~``/submd/repos/lib1``.git libs``/lib1`<br>`Cloning into libs``/lib1``...`<br>`done``.`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1` `git:(master) ✗ git submodule add ~``/submd/repos/lib2``.git libs``/lib2`<br>`Cloning into libs``/lib2``...`<br>`done``.`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1` `git:(master) ✗` `ls`<br>`libs  project-infos.txt`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1` `git:(master) ✗` `ls` `libs`<br>`lib1  lib2`<br><br>`➜ henryyan@hy-hp  ~``/submd/ws/project1` `git:(master) ✗ git status`<br>`# On branch master`<br>`# Changes to be committed:`<br>`#   (use "git reset HEAD <file>..." to unstage)`<br>`#`<br>`#   new file:   .gitmodules`<br>`#   new file:   libs/lib1`<br>`#   new file:   libs/lib2`<br>`#`<br><br>`# 查看一下公共类库的内容`<br><br>`➜ henryyan@hy-hp  ~``/submd/ws/project1` `git:(master)` `cat` `libs``/lib1/lib1-features`<br>`I'm lib1.`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1` `git:(master)` `cat` `libs``/lib2/lib2-features`<br>`I'm lib2.`<br>`<``/file``>` |

好了，到目前为止我们已经使用**git submodule add**命令为**project1**成功添加了两个公共类库（lib1、lib2），查看了当前的状态发现添加了一个新文件(**.gitmodules**)和两个文件夹(libs/lib1、libs/lib2)；那么新增的**.gitmodules**文件是做什么用的呢？我们查看一下文件内容便知晓了：

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7 | `n@hy-hp  ~``/submd/ws/project1` `git:(master) ✗` `cat` `.gitmodules`<br>`[submodule` `"libs/lib1"``]`<br>    `path = libs``/lib1`<br>    `url =` `/home/henryyan/submd/repos/lib1``.git`<br>`[submodule` `"libs/lib2"``]`<br>    `path = libs``/lib2`<br>    `url =` `/home/henryyan/submd/repos/lib2``.git` |

原来如此，**.gitmodules**记录了每个submodule的引用信息，知道在当前项目的位置以及仓库的所在。

好的，我们现在把更改提交到仓库。

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8<br>9<br>10<br>11<br>12<br>13<br>14<br>15<br>16 | `➜ henryyan@hy-hp  ~``/submd/ws/project1` `git:(master) ✗ git commit -a -m` `"add submodules[lib1,lib2] to project1"`<br>`[master 7157977] add submodules[lib1,lib2] to project1`<br> `3 files changed, 8 insertions(+), 0 deletions(-)`<br> `create mode 100644 .gitmodules`<br> `create mode 160000 libs``/lib1`<br> `create mode 160000 libs``/lib2`<br><br>`➜ henryyan@hy-hp  ~``/submd/ws/project1` `git:(master) git push`<br>`Counting objects: 5,` `done``.`<br>`Delta compression using up to 2 threads.`<br>`Compressing objects: 100% (4``/4``),` `done``.`<br>`Writing objects: 100% (4``/4``), 491 bytes,` `done``.`<br>`Total 4 (delta 0), reused 0 (delta 0)`<br>`Unpacking objects: 100% (4``/4``),` `done``.`<br>`To` `/home/henryyan/submd/ws/``..``/repos/project1``.git`<br>   `45cbbcb..7157977  master -> master` |

    假如你是第一次引入公共类库的开发人员，那么项目组的其他成员怎么Clone带有Submodule的项目呢，下面我们再clone一个项目讲解如何操作。
    

### 2.3 Clone带有Submodule的仓库

模拟开发人员B……

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8 | `➜ henryyan@hy-hp  ~``/submd/ws/project1` `git:(master)` `cd` `~``/submd/ws`<br>`➜ henryyan@hy-hp  ~``/submd/ws`  `git clone ..``/repos/project1``.git project1-b`<br>`Cloning into project1-b...`<br>`done``.`<br>`➜ henryyan@hy-hp  ~``/submd/ws`  `cd` `project1-b`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1-b` `git:(master) git submodule`<br>`-c22aff85be91eca442734dcb07115ffe526b13a1 libs``/lib1`<br>`-7290dce0062bd77df1d83b27dd3fa3f25a836b54 libs``/lib2` |

看到submodules的状态是hash码和文件目录，但是注意前面有一个减号：**\-**，含义是该子模块还没有检出。

OK，检出project1-b的submodules……

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8<br>9<br>10 | `➜ henryyan@hy-hp  ~``/submd/ws/project1-b` `git:(master) git submodule init`<br>`Submodule` `'libs/lib1'` `(``/home/henryyan/submd/repos/lib1``.git) registered` `for` `path` `'libs/lib1'`<br>`Submodule` `'libs/lib2'` `(``/home/henryyan/submd/repos/lib2``.git) registered` `for` `path` `'libs/lib2'`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1-b` `git:(master) git submodule update`<br>`Cloning into libs``/lib1``...`<br>`done``.`<br>`Submodule path` `'libs/lib1'``: checked out` `'c22aff85be91eca442734dcb07115ffe526b13a1'`<br>`Cloning into libs``/lib2``...`<br>`done``.`<br>`Submodule path` `'libs/lib2'``: checked out` `'7290dce0062bd77df1d83b27dd3fa3f25a836b54'` |

    读者可以查看：.git/config文件的内容，最下面有submodule的注册信息！
    

验证一下类库的文件是否存在：

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3 | `➜ henryyan@hy-hp  ~``/submd/ws/project1-b` `git:(master)` `cat` `libs``/lib1/lib1-features` `libs``/lib2/lib2-features`<br>`I'm lib1.`<br>`I'm lib2.` |

    上面的两个命令(git submodule init & update)其实可以简化，后面会讲到！
    

### 2.3 修改Submodule

我们在开发人员B的项目上修改Submodule的内容。

先看一下当前Submodule的状态：

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4 | `➜ henryyan@hy-hp  ~``/submd/ws/project1-b` `git:(master)` `cd` `libs``/lib1`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1-b/libs/lib1`  `git status`<br>`# Not currently on any branch.`<br>`nothing to commit (working directory clean)` |

为什么是**Not currently on any branch**呢？不是应该默认在**master**分支吗？别急，一一解答！

Git对于Submodule有特殊的处理方式，在一个主项目中引入了Submodule其实Git做了3件事情：

		记录引用的仓库
	
		记录主项目中Submodules的目录位置
	
		记录引用Submodule的**commit id**
	

在**project1**中push之后其实就是更新了引用的commit id，然后project1-b在clone的时候获取到了submodule的commit id，然后当执行**git submodule update**的时候git就根据**gitlink**获取submodule的commit id，最后获取submodule的文件，所以clone之后不在任何分支上；但是master分支的commit id和HEAD保持一致。

查看~/submd/ws/project1-b/libs/lib1的引用信息：

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4 | `➜ henryyan@hy-hp  ~``/submd/ws/project1-b/libs/lib1`  `cat` `.git``/HEAD`<br>`c22aff85be91eca442734dcb07115ffe526b13a1`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1-b/libs/lib1`  `cat` `.git``/refs/heads/master`              <br>`c22aff85be91eca442734dcb07115ffe526b13a1` |

现在我们要修改lib1的文件需要先切换到**master**分支：

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6 | `➜ henryyan@hy-hp  ~``/submd/ws/project1-b/libs/lib1`  `git checkout master`<br>`Switched to branch` `'master'`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1-b/libs/lib1` `git:(master)` `echo` `"add by developer B"` `>> lib1-features`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1-b/libs/lib1` `git:(master) ✗ git commit -a -m` `"update lib1-features by developer B"`<br>`[master 36ad12d] update lib1-features by developer B`<br> `1 files changed, 1 insertions(+), 0 deletions(-)` |

在主项目中修改Submodule提交到仓库稍微繁琐一点，在**git push**之前我们先看看**project1-b**状态：

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8<br>9<br>10 | `➜ henryyan@hy-hp  ~``/submd/ws/project1-b` `git:(master) ✗ git status`<br>`# On branch master`<br>`# Changes not staged for commit:`<br>`#   (use "git add <file>..." to update what will be committed)`<br>`#   (use "git checkout -- <file>..." to discard changes in working directory)`<br>`#`<br>`#   modified:   libs/lib1 (new commits)`<br>`#`<br>`no changes added to commit (use` `"git add"` `and``/or` `"git commit -a"``)`<br>`<``/file``><``/file``>` |

**libs/lib1 (new commits)**状态表示**libs/lib1**有新的提交，这个比较特殊，看看**project1-b**的状态：

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8 | `➜ henryyan@hy-hp  ~``/submd/ws/project1-b` `git:(master) ✗ git` `diff`<br>`diff` `--git a``/libs/lib1` `b``/libs/lib1`<br>`index c22aff8..36ad12d 160000`<br>`--- a``/libs/lib1`<br>`+++ b``/libs/lib1`<br>`@@ -1 +1 @@`<br>`-Subproject commit c22aff85be91eca442734dcb07115ffe526b13a1`<br>`+Subproject commit 36ad12d40d8a41a4a88a64add27bd57cf56c9de2` |

从状态中可以看出**libs/lib1**的commit id由原来的**c22aff85be91eca442734dcb07115ffe526b13a1**更改为**36ad12d40d8a41a4a88a64add27bd57cf56c9de2**

注意：如果现在执行了git submodule update操作那么libs/lib1的commit id又会还原到c22aff85be91eca442734dcb07115ffe526b13a1，

这样的话刚刚的修改是不是就丢死了呢？不会，因为修改已经提交到了master分支，只要再git checkout master就可以了。

现在可以把**libs/lib1**的修改提交到仓库了：

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8 | `➜ henryyan@hy-hp  ~``/submd/ws/project1-b` `git:(master) ✗` `cd` `libs``/lib1`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1-b/libs/lib1` `git:(master) git push`<br>`Counting objects: 5,` `done``.`<br>`Writing objects: 100% (3``/3``), 300 bytes,` `done``.`<br>`Total 3 (delta 0), reused 0 (delta 0)`<br>`Unpacking objects: 100% (3``/3``),` `done``.`<br>`To` `/home/henryyan/submd/repos/lib1``.git`<br>   `c22aff8..36ad12d  master -> master` |

现在仅仅只完成了一步，下一步要提交**project1-b**引用submodule的commit id：

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8<br>9<br>10<br>11<br>12<br>13<br>14 | `➜ henryyan@hy-hp  ~``/submd/ws/project1-b/libs/lib1` `git:(master)` `cd` `~``/submd/ws/project1-b`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1-b` `git:(master) ✗ git add -u`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1-b` `git:(master) ✗ git commit -m` `"update libs/lib1 to lastest commit id"`<br>`[master c96838a] update libs``/lib1` `to lastest commit` `id`<br> `1 files changed, 1 insertions(+), 1 deletions(-)`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1-b` `git:(master) git push`<br>`Counting objects: 5,` `done``.`<br>`Delta compression using up to 2 threads.`<br>`Compressing objects: 100% (3``/3``),` `done``.`<br>`Writing objects: 100% (3``/3``), 395 bytes,` `done``.`<br>`Total 3 (delta 0), reused 0 (delta 0)`<br>`Unpacking objects: 100% (3``/3``),` `done``.`<br>`To` `/home/henryyan/submd/ws/``..``/repos/project1``.git`<br>   `7157977..c96838a  master -> master` |

OK，大功高成，我们完成了Submodule的修改并把**libs/lib1**的最新commit id提交到了仓库。

接下来要看看**project1**怎么获取submodule了。

### 2.4 更新主项目的Submodules

好的，让我们先进入**project1**目录同步仓库：

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8<br>9<br>10<br>11<br>12<br>13<br>14<br>15<br>16<br>17<br>18<br>19<br>20<br>21<br>22 | `➜ henryyan@hy-hp  ~``/submd/ws/project1-b` `git:(master)` `cd` `..``/project1`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1` `git:(master) git pull`<br>`remote: Counting objects: 5,` `done``.`<br>`remote: Compressing objects: 100% (3``/3``),` `done``.`<br>`remote: Total 3 (delta 0), reused 0 (delta 0)`<br>`Unpacking objects: 100% (3``/3``),` `done``.`<br>`From` `/home/henryyan/submd/ws/``..``/repos/project1`<br>   `7157977..c96838a  master     -> origin``/master`<br>`Updating 7157977..c96838a`<br>`Fast-forward`<br> `libs``/lib1` `\|    2 +-`<br> `1 files changed, 1 insertions(+), 1 deletions(-)`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1` `git:(master) ✗ git status`<br>`# On branch master`<br>`# Changes not staged for commit:`<br>`#   (use "git add <file>..." to update what will be committed)`<br>`#   (use "git checkout -- <file>..." to discard changes in working directory)`<br>`#`<br>`#   modified:   libs/lib1 (new commits)`<br>`#`<br>`no changes added to commit (use` `"git add"` `and``/or` `"git commit -a"``)`<br>`<``/file``><``/file``>` |

我们运行了**git pull**命令和**git status**获取了最新的仓库源码，然后看到了状态时**modified**，这是为什么呢？

我们用**git diff**比较一下不同：

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8 | `➜ henryyan@hy-hp  ~``/submd/ws/project1` `git:(master) ✗ git` `diff`<br>`diff` `--git a``/libs/lib1` `b``/libs/lib1`<br>`index 36ad12d..c22aff8 160000`<br>`--- a``/libs/lib1`<br>`+++ b``/libs/lib1`<br>`@@ -1 +1 @@`<br>`-Subproject commit 36ad12d40d8a41a4a88a64add27bd57cf56c9de2`<br>`+Subproject commit c22aff85be91eca442734dcb07115ffe526b13a1` |

从diff的结果分析出来时因为submodule的commit id更改了，我们前面刚刚讲了要在主项目更新submodule的内容首先要提交submdoule的内容，然后再更新主项目中引用的submodulecommit id；现在我们看到的不同就是因为刚刚更改了project1-b的submodule commit id；好的，我来学习一下怎么更新**project1**的公共类库。

follow me……

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8<br>9<br>10<br>11 | `➜ henryyan@hy-hp  ~``/submd/ws/project1` `git:(master) ✗ git submodule update`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1` `git:(master) ✗ git status`<br>`# On branch master`<br>`# Changes not staged for commit:`<br>`#   (use "git add <file>..." to update what will be committed)`<br>`#   (use "git checkout -- <file>..." to discard changes in working directory)`<br>`#`<br>`#   modified:   libs/lib1 (new commits)`<br>`#`<br>`no changes added to commit (use` `"git add"` `and``/or` `"git commit -a"``)`<br>`<``/file``><``/file``>` |

泥马，为什么没有更新？**git submodule update**命令不是更新子模块仓库的吗？

别急，先听我解释；因为子模块是在**project1**中引入的，**git submodule add ~/submd/repos/lib1.git libs/lib1**命令的结果，操作之后git只是把lib1的内容clone到了**project1**中，但是没有在仓库注册，证据如下：

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8<br>9<br>10<br>11<br>12 | `➜ henryyan@hy-hp  ~``/submd2/ws/project1` `git:(master) ✗` `cat` `.git``/config`<br>`[core]`<br>    `repositoryformatversion = 0`<br>    `filemode =` `true`<br>    `bare =` `false`<br>    `logallrefupdates =` `true`<br>`[remote` `"origin"``]`<br>    `fetch = +refs``/heads/``*:refs``/remotes/origin/``*`<br>    `url =` `/home/henryyan/submd/ws/``..``/repos/project1``.git`<br>`[branch` `"master"``]`<br>    `remote = origin`<br>    `merge = refs``/heads/master` |

我们说过**git submodule init**就是在**.git/config**中注册子模块的信息，下面我们试试注册之后再更新子模块：

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8<br>9<br>10<br>11<br>12<br>13<br>14<br>15<br>16<br>17<br>18<br>19<br>20<br>21<br>22<br>23<br>24<br>25<br>26<br>27<br>28<br>29<br>30<br>31 | `➜ henryyan@hy-hp  ~``/submd/ws/project1` `git:(master) ✗ git submodule init`<br>`Submodule` `'libs/lib1'` `(``/home/henryyan/submd/repos/lib1``.git) registered` `for` `path` `'libs/lib1'`<br>`Submodule` `'libs/lib2'` `(``/home/henryyan/submd/repos/lib2``.git) registered` `for` `path` `'libs/lib2'`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1` `git:(master) ✗ git submodule update`<br>`remote: Counting objects: 5,` `done``.`<br>`remote: Total 3 (delta 0), reused 0 (delta 0)`<br>`Unpacking objects: 100% (3``/3``),` `done``.`<br>`From` `/home/henryyan/submd/repos/lib1`<br>   `c22aff8..36ad12d  master     -> origin``/master`<br>`Submodule path` `'libs/lib1'``: checked out` `'36ad12d40d8a41a4a88a64add27bd57cf56c9de2'`<br><br>`➜ henryyan@hy-hp  ~``/submd/ws/project1` `git:(master)` `cat` `.git``/config`<br>`[core]`<br>    `repositoryformatversion = 0`<br>    `filemode =` `true`<br>    `bare =` `false`<br>    `logallrefupdates =` `true`<br>`[remote` `"origin"``]`<br>    `fetch = +refs``/heads/``*:refs``/remotes/origin/``*`<br>    `url =` `/home/henryyan/submd/ws/``..``/repos/project1``.git`<br>`[branch` `"master"``]`<br>    `remote = origin`<br>    `merge = refs``/heads/master`<br>`[submodule` `"libs/lib1"``]`<br>    `url =` `/home/henryyan/submd/repos/lib1``.git`<br>`[submodule` `"libs/lib2"``]`<br>    `url =` `/home/henryyan/submd/repos/lib2``.git`<br><br>`➜ henryyan@hy-hp  ~``/submd/ws/project1` `git:(master)` `cat` `libs``/lib1/lib1-features`<br>`I'm lib1.`<br>`add by developer B` |

上面的结果足以证明刚刚的推断，所以记得当需要更新子模块的内容时请先确保已经运行过**git submodule init**。

### 2.5 为project2添加lib1和lib2

这个操作对于读到这里的你来说应该是**轻车熟路**了，action：

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8<br>9<br>10<br>11<br>12<br>13<br>14<br>15<br>16<br>17<br>18<br>19<br>20<br>21<br>22<br>23<br>24<br>25<br>26<br>27<br>28<br>29<br>30<br>31<br>32<br>33<br>34<br>35<br>36<br>37<br>38<br>39<br>40 | `➜ henryyan@hy-hp  ~``/submd/ws/project1` `git:(master)` `cd` `~``/submd/ws/project2`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project2` `git:(master) git submodule add ~``/submd/repos/lib1``.git libs``/lib1`<br>`Cloning into libs``/lib1``...`<br>`done``.`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project2` `git:(master) ✗ git submodule add ~``/submd/repos/lib2``.git libs``/lib2`<br>`zsh: correct` `'libs/lib2'` `to` `'libs/lib1'` `[nyae]? n`<br>`Cloning into libs``/lib2``...`<br>`done``.`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project2` `git:(master) ✗` `ls`<br>`libs  project-infos.txt`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project2` `git:(master) ✗ git submodule init`<br>`Submodule` `'libs/lib1'` `(``/home/henryyan/submd/repos/lib1``.git) registered` `for` `path` `'libs/lib1'`<br>`Submodule` `'libs/lib2'` `(``/home/henryyan/submd/repos/lib2``.git) registered` `for` `path` `'libs/lib2'`<br><br>`➜ henryyan@hy-hp  ~``/submd/ws/project2` `git:(master) ✗ git status`<br>`# On branch master`<br>`# Changes to be committed:`<br>`#   (use "git reset HEAD <file>..." to unstage)`<br>`#`<br>`#   new file:   .gitmodules`<br>`#   new file:   libs/lib1`<br>`#   new file:   libs/lib2`<br>`#`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project2` `git:(master) ✗ git commit -a -m` `"add lib1 and lib2"`<br>`[master 8dc697f] add lib1 and lib2`<br> `3 files changed, 8 insertions(+), 0 deletions(-)`<br> `create mode 100644 .gitmodules`<br> `create mode 160000 libs``/lib1`<br> `create mode 160000 libs``/lib2`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project2` `git:(master) git push`<br>`Counting objects: 5,` `done``.`<br>`Delta compression using up to 2 threads.`<br>`Compressing objects: 100% (4``/4``),` `done``.`<br>`Writing objects: 100% (4``/4``), 471 bytes,` `done``.`<br>`Total 4 (delta 0), reused 0 (delta 0)`<br>`Unpacking objects: 100% (4``/4``),` `done``.`<br>`To` `/home/henryyan/submd/ws/``..``/repos/project2``.git`<br>   `6e15c68..8dc697f  master -> master`<br><br>`<``/file``>` |

我们依次执行了添加submodule并commit和push到仓库，此阶段任务完成。

### 2.6 修改lib1和lib2并同步到project1和project2

假如开发人员C同时负责**project1**和**project2**，有可能在修改**project1**的某个功能的时候发现lib1或者lib2的某个组件有bug需要修复，这个需求多模块和大型系统中经常遇到，我们应该怎么解决呢？

假如我的需求如下：

		在lib1中添加一个文件：README，用来描述lib1的功能
	
		在lib2中的lib2-features文件中添加一写文字：学习Git submodule的修改并同步功能
	

#### 2.6.1 在lib1中添加一个文件：README

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8<br>9<br>10<br>11<br>12<br>13<br>14<br>15<br>16 | `➜ henryyan@hy-hp  ~``/submd/ws/project2` `git:(master)` `cd` `libs``/lib1`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project2/libs/lib1` `git:(master)` `echo` `"lib1 readme contents"` `> README`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project2/libs/lib1` `git:(master) ✗ git add README`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project2/libs/lib1` `git:(master) ✗ git commit -m` `"add file README"`<br>`[master 8c666d8] add` `file` `README`<br> `1 files changed, 1 insertions(+), 0 deletions(-)`<br> `create mode 100644 README`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project2/libs/lib1` `git:(master) git push`<br>`Counting objects: 4,` `done``.`<br>`Delta compression using up to 2 threads.`<br>`Compressing objects: 100% (2``/2``),` `done``.`<br>`Writing objects: 100% (3``/3``), 310 bytes,` `done``.`<br>`Total 3 (delta 0), reused 0 (delta 0)`<br>`Unpacking objects: 100% (3``/3``),` `done``.`<br>`To` `/home/henryyan/submd/repos/lib1``.git`<br>   `36ad12d..8c666d8  master -> master` |

前面提到过现在仅仅只完成了一部分，我们需要在**project2**中再更新lib1的commit id：

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8<br>9<br>10<br>11<br>12<br>13<br>14 | `➜ henryyan@hy-hp  ~``/submd/ws/project2` `git:(master) ✗ git status`<br>`# On branch master`<br>`# Changes not staged for commit:`<br>`#   (use "git add <file>..." to update what will be committed)`<br>`#   (use "git checkout -- <file>..." to discard changes in working directory)`<br>`#`<br>`#   modified:   libs/lib1 (new commits)`<br>`#`<br>`no changes added to commit (use` `"git add"` `and``/or` `"git commit -a"``)`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project2` `git:(master) ✗ git add libs``/lib1`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project2` `git:(master) ✗ git commit -m` `"update lib1 to lastest commit id"`<br>`[master ce1f3ba] update lib1 to lastest commit` `id`<br> `1 files changed, 1 insertions(+), 1 deletions(-)`<br>`<``/file``><``/file``>` |

    我们暂时不push到仓库，等待和lib2的修改一起push。
    

#### 2.6.2 在lib2中的lib2-features文件添加文字

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8<br>9<br>10<br>11<br>12<br>13<br>14<br>15<br>16<br>17<br>18<br>19<br>20<br>21<br>22<br>23<br>24<br>25<br>26<br>27<br>28<br>29<br>30<br>31<br>32<br>33<br>34<br>35<br>36<br>37<br>38<br>39<br>40<br>41<br>42<br>43<br>44<br>45<br>46<br>47<br>48<br>49<br>50<br>51<br>52<br>53<br>54<br>55<br>56<br>57<br>58<br>59<br>60<br>61<br>62 | `➜ henryyan@hy-hp  ~``/submd/ws/project2` `git:(master)` `cd` `libs``/lib2`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project2/libs/lib2` `git:(master)` `echo` `"学习Git submodule的修改并同步功能"` `>> lib2-features`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project2/libs/lib2` `git:(master) ✗ git add lib2-features`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project2/libs/lib2` `git:(master) ✗ git commit -m` `"添加文字：学习Git submodule的修改并同步功能"`<br>`[master e372b21] 添加文字：学习Git submodule的修改并同步功能`<br> `1 files changed, 1 insertions(+), 0 deletions(-)`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project2/libs/lib2` `git:(master) git push`<br>`Counting objects: 5,` `done``.`<br>`Delta compression using up to 2 threads.`<br>`Compressing objects: 100% (2``/2``),` `done``.`<br>`Writing objects: 100% (3``/3``), 376 bytes,` `done``.`<br>`Total 3 (delta 0), reused 0 (delta 0)`<br>`Unpacking objects: 100% (3``/3``),` `done``.`<br>`To` `/home/henryyan/submd/repos/lib2``.git`<br>   `7290dce..e372b21  master -> master`<br><br>`➜ henryyan@hy-hp  ~``/submd/ws/project2/libs/lib2` `git:(master)` `echo` `"学习Git submodule的修改并同步功能"` `>> lib2-features`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project2/libs/lib2` `git:(master) ✗ git add lib2-features`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project2/libs/lib2` `git:(master) ✗ git commit -m` `"添加文字：学习Git submodule的修改并同步功能"`<br>`[master e372b21] 添加文字：学习Git submodule的修改并同步功能`<br> `1 files changed, 1 insertions(+), 0 deletions(-)`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project2/libs/lib2` `git:(master) git push`<br>`Counting objects: 5,` `done``.`<br>`Delta compression using up to 2 threads.`<br>`Compressing objects: 100% (2``/2``),` `done``.`<br>`Writing objects: 100% (3``/3``), 376 bytes,` `done``.`<br>`Total 3 (delta 0), reused 0 (delta 0)`<br>`Unpacking objects: 100% (3``/3``),` `done``.`<br>`To` `/home/henryyan/submd/repos/lib2``.git`<br>   `7290dce..e372b21  master -> master`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project2/libs/lib2` `git:(master)` `cd` `-`<br>`~``/submd/ws/project2`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project2` `git:(master) ✗ git status`<br>`# On branch master`<br>`# Your branch is ahead of 'origin/master' by 1 commit.`<br>`#`<br>`# Changes not staged for commit:`<br>`#   (use "git add <file>..." to update what will be committed)`<br>`#   (use "git checkout -- <file>..." to discard changes in working directory)`<br>`#`<br>`#   modified:   libs/lib2 (new commits)`<br>`#`<br>`no changes added to commit (use` `"git add"` `and``/or` `"git commit -a"``)`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project2` `git:(master) ✗ git add libs``/lib2` <br>`➜ henryyan@hy-hp  ~``/submd/ws/project2` `git:(master) ✗ git commit -m` `"update lib2 to lastest commit id"`<br>`[master df344c5] update lib2 to lastest commit` `id`<br> `1 files changed, 1 insertions(+), 1 deletions(-)`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project2` `git:(master) git status`<br>`# On branch master`<br>`# Your branch is ahead of 'origin/master' by 2 commits.`<br>`#`<br>`nothing to commit (working directory clean)`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project2` `git:(master) git push`<br>`Counting objects: 8,` `done``.`<br>`Delta compression using up to 2 threads.`<br>`Compressing objects: 100% (6``/6``),` `done``.`<br>`Writing objects: 100% (6``/6``), 776 bytes,` `done``.`<br>`Total 6 (delta 0), reused 0 (delta 0)`<br>`Unpacking objects: 100% (6``/6``),` `done``.`<br>`To` `/home/henryyan/submd/ws/``..``/repos/project2``.git`<br>   `8dc697f..df344c5  master -> master`<br>`<``/file``><``/file``>` |

### 2.7 同步project2的lib1和lib2的修改到project1

现在project2已经享受到了最新的代码带来的**快乐**，那么既然project1和project2属于同一个风格，或者调用同一个功能，要让这两个(可能几十个)项目保持一致。

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3 | `➜ henryyan@hy-hp  ~``/submd/ws/project2` `git:(master)` `cd` `..``/project1`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1` `git:(master) git pull`<br>`Already up-to-``date``.` |

看看上面的结果对吗？为什么lib1和lib2更新了但是没有显示**new commits**呢？说到这里我记得刚刚开始学习的时候真得要晕死了，Git跟我玩捉迷藏游戏，为什么我明明提交了但是从**project1**更新不到任何改动呢？

帮大家分析一下问题，不过在分析之前先看看当前(project1和project2)的submodule状态：

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8<br>9 | `# project2 的状态，也就是我们刚刚修改后的状态`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project2` `git:(master) git submodule`<br> `8c666d86531513dd1aebdf235f142adbac72c035 libs``/lib1` `(heads``/master``)`<br> `e372b21dffa611802c282278ec916b5418acebc2 libs``/lib2` `(heads``/master``)`<br><br>`# project1 的状态，等待更新submodules`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1` `git:(master) git submodule`<br> `36ad12d40d8a41a4a88a64add27bd57cf56c9de2 libs``/lib1` `(remotes``/origin/HEAD``)`<br> `7290dce0062bd77df1d83b27dd3fa3f25a836b54 libs``/lib2` `(heads``/master``)` |

两个项目有两个区别：

		commit id各不相同
	
		**libs/lib1**所处的分支不同
	

#### 2.7.1 更新project1的lib1和lib2改动

我们还记得刚刚在**project2**中修改的时候把lib1和lib2都切换到了master分支，目前project1中的lib1不在任何分支，我们先切换到master分支：

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8<br>9<br>10<br>11<br>12<br>13<br>14<br>15<br>16<br>17<br>18<br>19 | `➜ henryyan@hy-hp  ~``/submd/ws/project1` `git:(master)` `cd` `libs``/lib1`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1/libs/lib1`  `git checkout master`<br>`Previous HEAD position was 36ad12d... update lib1-features by developer B`<br>`Switched to branch` `'master'`<br>`Your branch is behind` `'origin/master'` `by 1 commit, and can be fast-forwarded.`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1/libs/lib1` `git:(master) git pull`<br>`remote: Counting objects: 4,` `done``.`<br>`remote: Compressing objects: 100% (2``/2``),` `done``.`<br>`remote: Total 3 (delta 0), reused 0 (delta 0)`<br>`Unpacking objects: 100% (3``/3``),` `done``.`<br>`From` `/home/henryyan/submd/repos/lib1`<br>   `36ad12d..8c666d8  master     -> origin``/master`<br>`Updating c22aff8..8c666d8`<br>`Fast-forward`<br> `README        \|    1 +`<br> `lib1-features \|    1 +`<br> `2 files changed, 2 insertions(+), 0 deletions(-)`<br> `create mode 100644 README`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1/libs/lib1` `git:(master)` |

果不其然，我们看到了刚刚在project2中修改的内容，同步到了project1中，当然现在更新了**project1**的**lib1**，commit id也会随之变动：

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8<br>9<br>10<br>11<br>12<br>13<br>14<br>15<br>16<br>17<br>18<br>19 | `➜ henryyan@hy-hp  ~``/submd/ws/project1/libs/lib1` `git:(master)` `cd` `../../`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1` `git:(master) ✗ git status`<br>`# On branch master`<br>`# Changes not staged for commit:`<br>`#   (use "git add <file>..." to update what will be committed)`<br>`#   (use "git checkout -- <file>..." to discard changes in working directory)`<br>`#`<br>`#   modified:   libs/lib1 (new commits)`<br>`#`<br>`no changes added to commit (use` `"git add"` `and``/or` `"git commit -a"``)`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1` `git:(master) ✗ git` `diff`<br>`diff` `--git a``/libs/lib1` `b``/libs/lib1`<br>`index 36ad12d..8c666d8 160000`<br>`--- a``/libs/lib1`<br>`+++ b``/libs/lib1`<br>`@@ -1 +1 @@`<br>`-Subproject commit 36ad12d40d8a41a4a88a64add27bd57cf56c9de2`<br>`+Subproject commit 8c666d86531513dd1aebdf235f142adbac72c035`<br>`<``/file``><``/file``>` |

现在最新的commit id和project2目前的状态一致，说明真的同步了；好的，现在可以使用相同的办法更新**lib2**了：

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8<br>9<br>10<br>11<br>12 | `➜ henryyan@hy-hp  ~``/submd/ws/project1` `git:(master) ✗` `cd` `libs``/lib2`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1/libs/lib2` `git:(master) git pull`<br>`remote: Counting objects: 5,` `done``.`<br>`remote: Compressing objects: 100% (2``/2``),` `done``.`<br>`remote: Total 3 (delta 0), reused 0 (delta 0)`<br>`Unpacking objects: 100% (3``/3``),` `done``.`<br>`From` `/home/henryyan/submd/repos/lib2`<br>   `7290dce..e372b21  master     -> origin``/master`<br>`Updating 7290dce..e372b21`<br>`Fast-forward`<br> `lib2-features \|    1 +`<br> `1 files changed, 1 insertions(+), 0 deletions(-)` |

#### 2.7.2 更新project1的submodule引用

在**2.7.1**中我们更新了**project1**的**lib1**和**lib2**的最新版本，现在要把最新的commit id保存到**project1**中以保持最新的引用。

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8<br>9<br>10<br>11<br>12<br>13<br>14<br>15<br>16<br>17<br>18<br>19<br>20<br>21<br>22<br>23 | `➜ henryyan@hy-hp  ~``/submd/ws/project1` `git:(master) ✗ git status`<br>`# On branch master`<br>`# Changes not staged for commit:`<br>`#   (use "git add <file>..." to update what will be committed)`<br>`#   (use "git checkout -- <file>..." to discard changes in working directory)`<br>`#`<br>`#   modified:   libs/lib1 (new commits)`<br>`#   modified:   libs/lib2 (new commits)`<br>`#`<br>`no changes added to commit (use` `"git add"` `and``/or` `"git commit -a"``)`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1` `git:(master) ✗ git commit -a -m` `"update lib1 and lib2 commit id to new version"`<br>`[master 8fcca50] update lib1 and lib2 commit` `id` `to new version`<br> `2 files changed, 2 insertions(+), 2 deletions(-)`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1` `git:(master) git push`<br>`Counting objects: 5,` `done``.`<br>`Delta compression using up to 2 threads.`<br>`Compressing objects: 100% (3``/3``),` `done``.`<br>`Writing objects: 100% (3``/3``), 397 bytes,` `done``.`<br>`Total 3 (delta 0), reused 0 (delta 0)`<br>`Unpacking objects: 100% (3``/3``),` `done``.`<br>`To` `/home/henryyan/submd/ws/``..``/repos/project1``.git`<br>   `c96838a..8fcca50  master -> master`<br>`<``/file``><``/file``>` |

### 2.8 更新project1-b项目的子模块(使用脚本)

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8<br>9<br>10<br>11<br>12<br>13<br>14<br>15<br>16<br>17<br>18<br>19<br>20<br>21<br>22<br>23 | `➜ henryyan@hy-hp  ~``/submd/ws/project1-b` `git:(master) git pull`<br>`remote: Counting objects: 5,` `done``.`<br>`remote: Compressing objects: 100% (3``/3``),` `done``.`<br>`remote: Total 3 (delta 0), reused 0 (delta 0)`<br>`Unpacking objects: 100% (3``/3``),` `done``.`<br>`From` `/home/henryyan/submd/ws/``..``/repos/project1`<br>   `c96838a..8fcca50  master     -> origin``/master`<br>`Updating c96838a..8fcca50`<br>`Fast-forward`<br> `libs``/lib1` `\|    2 +-`<br> `libs``/lib2` `\|    2 +-`<br> `2 files changed, 2 insertions(+), 2 deletions(-)`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1-b` `git:(master) ✗ git status`<br>`# On branch master`<br>`# Changes not staged for commit:`<br>`#   (use "git add <file>..." to update what will be committed)`<br>`#   (use "git checkout -- <file>..." to discard changes in working directory)`<br>`#`<br>`#   modified:   libs/lib1 (new commits)`<br>`#   modified:   libs/lib2 (new commits)`<br>`#`<br>`no changes added to commit (use` `"git add"` `and``/or` `"git commit -a"``)`<br>`<``/file``><``/file``>` |

Git提示lib1和lib2有更新内容，这个判断的依据来源于submodule commit id的引用。

现在怎么更新呢？难道还是像project1中那样进入子模块的目录然后**git checkout master**，接着**git pull**？

而且现在仅仅才两个子模块、两个项目，如果在真实的项目中使用的话可能几个到几十个不等，再加上N个submodule，自己算一下要怎么更新多少个submodules？

例如笔者现在做的一个项目有**5**个web模块，每个web模块引用公共的css、js、images、jsp资源，这样就有20个submodule需要更新！！！

**工欲善其事，必先利其器**，写一个脚本代替手动任务。

#### 2.8.1 牛刀小试

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4 | `➜ henryyan@hy-hp  ~``/submd/ws/project1-b` `git:(master) ✗` `grep` `path .gitmodules \|` `awk` `'{ print $3 }'` `>` `/tmp/study-git-submodule-dirs`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1-b` `git:(master) ✗` `cat` `/tmp/study-git-submodule-dirs`<br> `libs``/lib1`<br> `libs``/lib2` |

我们通过分析**.gitmodules**文件得出子模块的路径，然后就可以根据这些路径进行更新。

#### 2.8.2 上路

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2 | `➜ henryyan@hy-hp  ~``/submd/ws/project1-b` `git:(master) ✗` `mkdir` `bin`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1-b` `git:(master) ✗` `vi` `bin``/update-submodules``.sh` |

把下面的脚本复制到**bin/update-submodules.sh**中：

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8<br>9 | `#!/bin/bash`<br>`grep` `path .gitmodules \|` `awk` `'{ print $3 }'` `>` `/tmp/study-git-submodule-dirs`<br><br>`# read`<br>`while` `read` `LINE`<br>`do`<br>    `echo` `$LINE`<br>    `(``cd` `./$LINE && git checkout master && git pull)`<br>`done` `<` `/tmp/study-git-submodule-dirs` |

稍微解释一下上面的脚本执行过程：

		首先把子模块的路径写入到文件**/tmp/study-git-submodule-dirs**中；
	
		然后读取文件中的子模块路径，依次切换到master分支(修改都是在master分支上进行的)，最后更新最近改动。
	

#### 2.8.3 2013-01-19更新

网友**@紫煌**给出了更好的办法，一个命令就可以代替上面的**bin/update-submodules.sh**的功能：

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1   | `git submodule foreach git pull` |

此命令也脚本一样，循环进入（enter）每个子模块的目录，然后执行**foreach**后面的命令。

> 该后面的命令可以任意的，例如 git submodule foreach ls -l 可以列出每个子模块的文件列表

#### 2.8.3 体验工具带来的便利

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8<br>9<br>10<br>11<br>12<br>13<br>14<br>15<br>16<br>17<br>18<br>19<br>20<br>21<br>22<br>23<br>24<br>25<br>26<br>27<br>28<br>29<br>30<br>31<br>32<br>33<br>34<br>35<br>36<br>37<br>38<br>39<br>40<br>41<br>42<br>43<br>44 | `➜ henryyan@hy-hp  ~``/submd/ws/project1-b` `git:(master) ✗ git submodule`<br>`+36ad12d40d8a41a4a88a64add27bd57cf56c9de2 libs``/lib1` `(heads``/master``)`<br>`+7290dce0062bd77df1d83b27dd3fa3f25a836b54 libs``/lib2` `(heads``/master``)`<br><br>`# 添加执行权限`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1-b` `git:(master) ✗` `chmod` `+x .``/bin/update-submodules``.sh`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1-b` `git:(master) ✗ .``/bin/update-submodules``.sh`<br>`libs``/lib1`<br>`Already on` `'master'`<br>`remote: Counting objects: 4,` `done``.`<br>`remote: Compressing objects: 100% (2``/2``),` `done``.`<br>`remote: Total 3 (delta 0), reused 0 (delta 0)`<br>`Unpacking objects: 100% (3``/3``),` `done``.`<br>`From` `/home/henryyan/submd/repos/lib1`<br>   `36ad12d..8c666d8  master     -> origin``/master`<br>`Updating 36ad12d..8c666d8`<br>`Fast-forward`<br> `README \|    1 +`<br> `1 files changed, 1 insertions(+), 0 deletions(-)`<br> `create mode 100644 README`<br>`libs``/lib2`<br>`Switched to branch` `'master'`<br>`remote: Counting objects: 5,` `done``.`<br>`remote: Compressing objects: 100% (2``/2``),` `done``.`<br>`remote: Total 3 (delta 0), reused 0 (delta 0)`<br>`Unpacking objects: 100% (3``/3``),` `done``.`<br>`From` `/home/henryyan/submd/repos/lib2`<br>   `7290dce..e372b21  master     -> origin``/master`<br>`Updating 7290dce..e372b21`<br>`Fast-forward`<br> `lib2-features \|    1 +`<br> `1 files changed, 1 insertions(+), 0 deletions(-)`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1-b` `git:(master) ✗ git submodule`<br> `8c666d86531513dd1aebdf235f142adbac72c035 libs``/lib1` `(heads``/master``)`<br> `e372b21dffa611802c282278ec916b5418acebc2 libs``/lib2` `(heads``/master``)`<br><br> `➜ henryyan@hy-hp  ~``/submd/ws/project1-b` `git:(master) ✗ git status`<br>`# On branch master`<br>`# Untracked files:`<br>`#   (use "git add <file>..." to include in what will be committed)`<br>`#`<br>`#   bin/`<br>`nothing added to commit but untracked files present (use` `"git add"` `to track)`<br>`<``/file``>` |

更新之后的两个变化：

		git submodule的结果和project2的submodule commit id保持一致；
	
		**project1-b**不再提示**new commits**了。
	

现在可以把工具添加到仓库了，当然你可以很骄傲的分享给其他项目组的同事。

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8<br>9<br>10<br>11<br>12<br>13<br>14 | `➜ henryyan@hy-hp  ~``/submd/ws/project1-b` `git:(master) ✗ git add bin``/update-submodules``.sh`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1-b` `git:(master) ✗ git commit -m` `"添加自动更新submodule的快捷脚本^_^"`<br>`[master 756e788] 添加自动更新submodule的快捷脚本^_^`<br> `1 files changed, 9 insertions(+), 0 deletions(-)`<br> `create mode 100755 bin``/update-submodules``.sh`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1-b` `git:(master) git push`<br>`Counting objects: 5,` `done``.`<br>`Delta compression using up to 2 threads.`<br>`Compressing objects: 100% (3``/3``),` `done``.`<br>`Writing objects: 100% (4``/4``), 625 bytes,` `done``.`<br>`Total 4 (delta 0), reused 0 (delta 0)`<br>`Unpacking objects: 100% (4``/4``),` `done``.`<br>`To` `/home/henryyan/submd/ws/``..``/repos/project1``.git`<br>   `8fcca50..756e788  master -> master` |

### 2.9 新进员工加入团队，一次性Clone项目和Submodules

一般人使用的时候都是使用如下命令：

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3 | `git clone` `/path/to/repos/foo``.git`<br>`git submodule init`<br>`git submodule update` |

    新员工不耐烦了，嘴上不说但是心里想：怎么那么麻烦？
    

上面的命令简直弱暴了，直接一行命令搞定：

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1   | `git clone --recursive` `/path/to/repos/foo``.git` |

–**recursive**参数的含义：可以在clone项目时同时clone关联的submodules。

git help 对其解释：

\--recursive, --recurse-submodules
   After the clone is created, initialize all submodules within, using their default settings. This is equivalent to running git
   submodule update --init --recursive immediately after the clone is finished. This option is ignored if the cloned repository
   does not have a worktree/checkout (i.e. if any of --no-checkout/-n, --bare, or --mirror is given)

#### 2.9.1 使用一键方式克隆project2

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8<br>9<br>10<br>11 | `➜ henryyan@hy-hp  ~``/submd/ws`  `git clone --recursive ..``/repos/project2``.git project2-auto-clone-submodules`<br>`Cloning into project2-auto-clone-submodules...`<br>`done``.`<br>`Submodule` `'libs/lib1'` `(``/home/henryyan/submd/repos/lib1``.git) registered` `for` `path` `'libs/lib1'`<br>`Submodule` `'libs/lib2'` `(``/home/henryyan/submd/repos/lib2``.git) registered` `for` `path` `'libs/lib2'`<br>`Cloning into libs``/lib1``...`<br>`done``.`<br>`Submodule path` `'libs/lib1'``: checked out` `'8c666d86531513dd1aebdf235f142adbac72c035'`<br>`Cloning into libs``/lib2``...`<br>`done``.`<br>`Submodule path` `'libs/lib2'``: checked out` `'e372b21dffa611802c282278ec916b5418acebc2'` |

舒服……

## 3.移除Submodule

**牢骚**：搞不明白为什么git不设计一个类似：git submodule remove的命令呢？

我们从project1.git克隆一个项目用来练习移除submodule：

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8<br>9<br>10<br>11<br>12<br>13 | `➜ henryyan@hy-hp  ~``/submd/ws`  `git clone --recursive ..``/repos/project1``.git project1-remove-submodules`<br>`Cloning into project1-remove-submodules...`<br>`done``.`<br>`Submodule` `'libs/lib1'` `(``/home/henryyan/submd/repos/lib1``.git) registered` `for` `path` `'libs/lib1'`<br>`Submodule` `'libs/lib2'` `(``/home/henryyan/submd/repos/lib2``.git) registered` `for` `path` `'libs/lib2'`<br>`Cloning into libs``/lib1``...`<br>`done``.`<br>`Submodule path` `'libs/lib1'``: checked out` `'8c666d86531513dd1aebdf235f142adbac72c035'`<br>`Cloning into libs``/lib2``...`<br>`done``.`<br>`Submodule path` `'libs/lib2'``: checked out` `'e372b21dffa611802c282278ec916b5418acebc2'`<br>`➜ henryyan@hy-hp  ~``/submd/ws`  `cd` `!# Git Submodule使用完整教程 - 咖啡兔 - HenryYan

<br>`➜ henryyan@hy-hp  ~``/submd/ws`  `cd` `project1-remove-submodules` |

### 3.1 Step by

1、删除git cache和物理文件夹

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4 | `➜ henryyan@hy-hp  ~``/submd/ws/project1-remove-submodules` `git:(master) git` `rm` `-r --cached libs/`<br>`rm` `'libs/lib1'`<br>`rm` `'libs/lib2'`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1-remove-submodules` `git:(master) ✗` `rm` `-rf libs` |

2、删除.gitmodules的内容（或者整个文件） 因为本例只有两个子模块，直接删除文件：

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1   | `➜ henryyan@hy-hp  ~``/submd/ws/project1-remove-submodules` `git:(master) ✗` `rm` `.gitmodules` |

    如果仅仅删除某一个submodule那么打开.gitmodules文件编辑，删除对应submodule配置即可。
    

3、删除.git/config的submodule配置 源文件：

\[core\]
    repositoryformatversion = 0 
    filemode = true
    bare = false
    logallrefupdates = true
\[remote "origin"\]
    fetch = +refs/heads/\*:refs/remotes/origin/\*
    url = /home/henryyan/submd/ws/../repos/project1.git
\[branch "master"\]
    remote = origin
    merge = refs/heads/master
\[submodule "libs/lib1"\]
    url = /home/henryyan/submd/repos/lib1.git
\[submodule "libs/lib2"\]
    url = /home/henryyan/submd/repos/lib2.git

删除后：

\[core\]
    repositoryformatversion = 0 
    filemode = true
    bare = false
    logallrefupdates = true
\[remote "origin"\]
    fetch = +refs/heads/\*:refs/remotes/origin/\*
    url = /home/henryyan/submd/ws/../repos/project1.git
\[branch "master"\]
    remote = origin
    merge = refs/heads/master

4、提交更改

[?](http://www.kafeitu.me/git/2012/03/27/git-submodule.html#)

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8<br>9<br>10<br>11<br>12<br>13<br>14<br>15<br>16<br>17<br>18<br>19<br>20<br>21<br>22<br>23<br>24<br>25<br>26<br>27<br>28<br>29<br>30<br>31 | `➜ henryyan@hy-hp  ~``/submd/ws/project1-remove-submodules` `git:(master) ✗ git status`<br>`# On branch master`<br>`# Changes to be committed:`<br>`#   (use "git reset HEAD <file>..." to unstage)`<br>`#`<br>`#   deleted:    libs/lib1`<br>`#   deleted:    libs/lib2`<br>`#`<br>`# Changes not staged for commit:`<br>`#   (use "git add/rm <file>..." to update what will be committed)`<br>`#   (use "git checkout -- <file>..." to discard changes in working directory)`<br>`#`<br>`#   deleted:    .gitmodules`<br>`#`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1-remove-submodules` `git:(master) ✗ git add .gitmodules`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1-remove-submodules` `git:(master) ✗ git commit -m` `"删除子模块lib1和lib2"`<br>`[master 5e2ee71] 删除子模块lib1和lib2`<br> `3 files changed, 0 insertions(+), 8 deletions(-)`<br> `delete mode 100644 .gitmodules`<br> `delete mode 160000 libs``/lib1`<br> `delete mode 160000 libs``/lib2`<br>`➜ henryyan@hy-hp  ~``/submd/ws/project1-remove-submodules` `git:(master) git push`<br>`Counting objects: 3,` `done``.`<br>`Delta compression using up to 2 threads.`<br>`Compressing objects: 100% (2``/2``),` `done``.`<br>`Writing objects: 100% (2``/2``), 302 bytes,` `done``.`<br>`Total 2 (delta 0), reused 0 (delta 0)`<br>`Unpacking objects: 100% (2``/2``),` `done``.`<br>`To` `/home/henryyan/submd/ws/``..``/repos/project1``.git`<br>   `756e788..5e2ee71  master -> master`<br>`<``/file``><``/file``><``/file``>` |

## 4.结束语

对于Git Submodule来说在刚刚接触的时候多少会有点凌乱的赶紧，尤其是没有接触过**svn:externals**。
本文从开始创建项目到使用git submodule的每个步骤以及细节、原理逐一讲解，希望到此读者能驾驭它。
学会了Git submdoule你的项目中再也不会出现大量重复的资源文件、公共类库，更不会出现多个版本，甚至一个客户的多个项目风格存在各种差异。
本文的写作来源于工作中的实践，如果您对于某个做法有更好的办法还请赐教，希望能留下您宝贵的意见。
