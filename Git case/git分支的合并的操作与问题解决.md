## git分支的合并的操作与问题解决
### git分支（branch）的操作命令
1.查看本地分支：   **$ git branch ** 或者 **$ git branch -l **

2.查看远程分支 ：  **$ git branch -r **	或者 ** $ git brach -a**
	
---- -r是仅查询远程的分支， -a查询包含远程分支和本地分支

3.创建本地分支：**$ git branch name**

---- name为你想要创建的分支名 并且创建分之后不会自动切换到新的分支；

4.切换分支 ： **$ git checkout name** 

---- 可以先查询本地的分支，然后再checkout到你想要切换的分支；
 
5.创建新分支并立即切换到新分支：**$ git checkout -b name**

----此命令可以直创建成功后切换到你所新创建的分支

6.删除分支：** $ git branch -d name ** 

---- -d选项只能删除已经参与了合并的分支，对于未有合并的分支是无法删除的。如果想强制删除一个分支，可以使用-D选项

7.创建远程分支(本地分支push到远程)：**$ git push origin [name]**

---- 创建远程分支，并且同时把本地当前分支下的内容推送到所新建的远程分支

8.删除远程分支：**$ git push origin :heads/[name]** 或 $ **git push origin :[name]** 


```
如果想把本地的某个分支test提交到远程仓库，并作为远程仓库的master分支，或者作为另外一个名叫test的分支，如下： 

$git push origin test:master         // 提交本地test分支作为远程的master分支

$git push origin test:test              // 提交本地test分支作为远程的test分支 
```

### git分支合并（merge）的操作
 在git中，可以使用 **git merge** 和 **git rebase** 两个命令来进行分支的合并。            
**git merge** 和 **git rebase** 在大体上都差不多，下文主要以 **git merge** 来例来讲解分支的合并流程。 
                                            
git命令： **$ git merge branchname **   

----这个命令把分支"branchname"合并到了当前分支里面。 
  
  ***在执行这个命令前，需要先查看你当前所在的分支，是否是两个合并的分支中的一个，如果不是你想要合并的分支，先切换到两个分支中的一个，然后才可以进行合并的命令。如果在本地中未找到你想要的分支，你就需要把所有这个git库所有的分支先 pull 下来***
  
 **下拉所有当前库中的所有分支 ： $ git pull -- all**
 
 ---- 在进行上面操作时需要先把分支切换到主分支
 
 ```
 当你成功切换到其中一条分支后执行以下命令
 
 $ git merge otherBranchName 
 
 ----otherBranchNmae 就是另外一条分支。
 
 ```
 
 
 
### 解决合并中的冲突

如果执行自动合并没有成功的话，git会在索引和工作树里设置一个特殊的状态， 提示你如何解决合并中出现的冲突。
有冲突(conflicts)的文件会保存在**索引**中，除非你解决了问题了并且更新了索引，否则执行 git commit都会失败:

```
如果存在冲突，当你执行 $ git commit 命令后 执行 git status 会显示这些文件没有合并(unmerged),这些有冲突的文件里面会添加像下面的冲突标识符:
fileName : needs merge

<<<<<<< HEAD:file.txt

Hello world

=======

Goodbye

>>>>>>> 77976da35a11db4580b80ae27e8d65caf5208086:file.txt

你所需要的做是就是编辑解决冲突，（接着把冲突标识符删掉），再执行下面的命令:

$ git add .
$ git commit -m “”


```

###撒销一个合并
如果你觉得你合并后的状态是一团乱麻，想把当前的修改都放弃，你可以用下面的命令回到合并之前的状态：

**$ git reset --hard HEAD**

----次命令不仅仅局限于撤销分支的合并。当你在在本地修改代码后，对于自己修改的不满意，并且不想自己手动改回去，可以执行上面的代码，代码会自动恢复到最后一次commit的版本。

或者你已经把合并后的代码提交，但还是想把它们撒销：
**$ git reset --hard ORIG_HEAD**

---- 但是刚才这条命令在某些情况会很危险，如果你把一个已经被另一个分支合并的分支给删了，那么 以后在合并相关的分支时会出错。
关于撤销的更多内容请参考[《git reset简介》](http://hubingforever.blog.163.com/blog/static/1710405792012411094228/)


更多内容可以参看 
[git合并与冲突的解决](http://blog.csdn.net/hudashi/article/details/7668798)
[分支与合并@基础](http://gitbook.liuhui998.com/3_3.html)
[高级分支与合并](http://gitbook.liuhui998.com/5_3.html)

