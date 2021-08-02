---
layout: git
title: Git 使用注意事项总结
date: 2021-08-02 15:34:09
tags:
---



```groovy
➜  Taurus git:(mqtt_test) ✗ git push origin HEAD:refs/for/release_v2.18.0%wip
枚举对象中: 52, 完成.
对象计数中: 100% (52/52), 完成.
使用 12 个线程进行压缩
压缩对象中: 100% (28/28), 完成.
写入对象中: 100% (32/32), 16.54 KiB | 5.51 MiB/s, 完成.
总共 32（差异 15），复用 0（差异 0），包复用 0
remote: Resolving deltas: 100% (15/15)
remote: Processing changes: refs: 1, done    
remote: commit 4acd093: ERROR: missing Change-Id in message footer
remote: 
remote: Hint: to automatically insert a Change-Id, install the hook:
remote:   gitdir=$(git rev-parse --git-dir); scp -p -P 29418 juneyang@172.16.12.171:hooks/commit-msg ${gitdir}/hooks/
remote: and then amend the commit:
remote:   git commit --amend
remote: 
To ssh://172.16.12.171:29418/Taurus.git
 ! [remote rejected]   HEAD -> refs/for/release_v2.18.0%wip (commit 4acd093: missing Change-Id in message footer)
error: 推送一些引用到 'ssh://172.16.12.171:29418/Taurus.git' 失败

```

首次提交流程：

1. 提交日志commit （ cmd + K） 这里可以用IDE来提交。后面的流程用命令。

   正常操作即可。

2. hook操作

   > gitdir=$(git rev-parse --git-dir); scp -p -P 29418 juneyang@172.16.12.171:hooks/commit-msg ${gitdir}/hooks/

3. 再次提交

   > git commit --amend

4. 进入Vim操作中，添加日志 保存退出 `:wq` .

5. push 添加wip状态意味着 当次提交不会合并，在编辑中。

   >  git push origin HEAD:refs/for/release_v2.18.0%wip

Virgo:

git push origin HEAD:refs/for/sprint/s25

# **Aries:**

git push origin HEAD:refs/for/dev



注意 其他项目中的提交需要单独提交。防止提交遗漏。


第二轮修改后提交流程：

1. 如果没有通过被拒绝之后，修改后再次提交

   git add .

   git commit --amend



如果在IDE中使用commint --amend中的方法 会出现 日志修改无效的问题。



Virgo:

git push origin HEAD:refs/for/sprint/s25

# 异常总结：

1.Git中vim修改权限： E45: 已设定选项 'readonly' (请加 ! 强制执行)

初始设置进行修改操作后，无法保存，git命令行输入指令：

> :wq!

回车即可强制修改并保存。

2. Changed_ID  必须放到下面 否则会push失败。

# Vim操作：

> 保存文件退出
>
>  ：w  写入不退出
>
>  ：w！ 强制写入
>
>  ：wq 写入退出
>
> ：wq！写入强制退出
>
> ZZ　写入退出，使用shift+两次ＺＺ
>
> ：q　　退出
>
> ：q！强制退出

小结：
>'i'进入编辑模式
>'esc'进入命令模式
>':wq'保存退出
>'q!'不保存强制退出

参考：

1.[Gerrit的commit-msg hook使用指南](https://blog.csdn.net/taiyangdao/article/details/71079021)

2.[Git使用之撤销上次提交、审核代码不通过修改再次提交](https://blog.csdn.net/QingKong999/article/details/47040047)



FAQ:

https://github.com/eclipse/paho.mqtt.android/issues/212



如何解决冲突？

git reset --soft HEAD^





提交push命令

```groovy
git push origin HEAD:refs/for/${branch}
```

[Gerrit常见命令及最佳实践](https://juejin.cn/post/6844903942464225294)



git 提交了两个暂未入库的的问题

必须等前一个入库后，另一个才能操作。

git rebase - i 进入提交 历史修改

将前缀 修改为edit 或者简写为(e)

```groovy
git status   
交互式变基操作正在进行中；至 d14af27b
最后一条命令已完成（1 条命令被执行）：
   edit 62101c48 feat(VIRGO-719):新增通过Mqtt发送系统消息指令控制设备DEBUG模式
接下来要执行的命令（剩余 1 条命令）：
   pick 53ab51e6 fix(VIRGO-753):精简 mqtt 调试日志上传
  （使用 "git rebase --edit-todo" 来查看和编辑）
您在执行将分支 'sprint/s26' 变基到 'd14af27b' 的操作时编辑提交。
  （使用 "git commit --amend" 修补当前提交）
  （当您对您的修改满意后执行 "git rebase --continue"）

```

git commit - amend时

出现正常的变基

```groovy
#
# 日期：  Sun Feb 7 16:55:28 2021 +0800
#
# 交互式变基操作正在进行中；至 d14af27b 
# 最后一条命令已完成（1 条命令被执行）： 
#    edit 62101c48 feat(VIRGO-719):新增通过Mqtt发送系统消息指令控制
```

操作时当前有的分支为rebase + 提交的变基点。

操作完成后 回到正常的状态,

执行

```groovy
git rebase --continue
```

此时的分支状态不再出现rebase字样。即正常的分支。

新建分支交上传远程：

> git checkout -b "分支名"
>
> git push origin "分支名"

删除远程分支 ：

> git push  origin --delete "分支名"

删除本地分支：

> git branch -D  "分支名"



Git 还原到某一次提交

175070cf47f99df8ee0a2c6c14abe70b99ec5a85

示例：

找到想要还原的某一次提交：

> git log --oneline
>
> Git checkout xxxx .

合并提交记录

https://segmentfault.com/a/1190000023055669



# 冲突解决

背景：当提交代码到 review borad后未入库，别人的代码先入库，并且出现冲突，此时代码的操作。

代码丢失处理：

从 git polyGerrit中添加代码：

> git cherry-pick
>
>  git fetch "ssh://juneyang@172.16.12.171:29418/Gemini" refs/changes/79/6879/1 && git cherry-pick FETCH_HEAD

git diff

解决冲突后

git add 某一个文件 如：

```xml
git add src/main/res/values/strings.xml   
```

然后

```xml
git cherry-pick --continue  
```

最后 push

```xml
git push origin HEAD:refs/for/dev    
```



实战：

第一步： git fetch xxx cherry-pick xxx

![image-20210611103323886](https://tva1.sinaimg.cn/large/008i3skNgy1gre4g4urqyj313e07p0up.jpg)

第二步：解决冲突。

第三步：根据提示，git add.

![image-20210611103456364](https://tva1.sinaimg.cn/large/008i3skNgy1gre4hqi5tmj30rp07f3zm.jpg)

第四步： 执行 git cherry-pick --continue时出现

![image-20210611103557871](https://tva1.sinaimg.cn/large/008i3skNgy1gre4itaztnj30ot0gu40f.jpg)

最后一步： push。

git push origin HEAD:refs/for/dev

操作完成！



## git push error:

![image-20210616191335924](https://tva1.sinaimg.cn/large/008i3skNgy1grkbkzpnskj30ql08hdhe.jpg)

https://stackoverflow.com/questions/11972384/git-push-remote-rejected-change-closed

```groovy
Do this to fix issue:

git commit --amend
delete change id

save and quit
new change id will be added to the commit. it can be verified by git log.
push again
```

1.删除 change-id

Change-Id: I8b4d553b3eb0771e1a1727e9c39d9d22f80de478

2.  git commit --amend

之后 生成了新的 change-id



git 提交时 有被人插队，push 时发现 head 的指针不是指向最新的了，你需要做的就是

1. git log 查看 commit-id
2. git reset --soft commit-id

```groovy
git reset --soft ee6b6757ace472d299f862b4292fa80da2bd1fe5

```

3. 本地缓存 并更新  ：git stash save ui 、git  pull --rebase

4.  将工作区间刷新：git reset --hard f8be84627b6141d716ef84f61ef6f01e463ca8c3

5. 最后重新提交

   git stash pop、解决冲突、git add.  git commit 、git push……

第二种方法：

1. `git reset --soft HEAD^`
   撤销上一次提交,修改的内容保留
   如果暂存区为空，则将上一次修改放入暂存区
   如果暂存区非空，则将不改变暂存区
   该操作不改变工作区
2. `git reset --hard HEAD^`
   工作区，暂存区，，仓库均保持为上一次提交的内容



小结：

https://www.jianshu.com/p/63199427ee68