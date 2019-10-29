## GitHub Pull Request入门 （PR）
## 提交日志(Commit Message)
    日志(Commit Message)要尽量避繁就简，这可以让你的PR更容易被人理解和接受。

    首行总结要尽量少于50字符。
    附加的更详细的说明，保持宽度在72字符左右。
    使用“Fix bug"代替”Fixed bug"或“Fixes bug"。
    多使用列表说明问题。
    代码礼仪(Code Standard)
    每个人都会有自己的代码风格，到底是用制表符(Tab)还是空格(Space)？每行是80字符还是120字符？这种圣战不应该在PR中出现，PR应该遵循项目已有的风格。例如：如果原来使用的驼峰命名变量，PR中就应该使用驼峰命名。

## 整理日志(Rebase)
    git rebase是一个让你可以改变历史的命令！通过它轻易地重新排列，编辑或合并历史日志。

    编辑以前提交过的日志。
    把多条日志合并成一条日志。
    删除或回滚一些不必要的日志。
    例如：有时图个方便，会把代码分阶段性提交，到真正完成时，希望Review的人看到是最终的结果。

      git commit -am "Add Account Form"
      git commit -am "Add Passwd Form"
      git commit -am "Add Verification Code Form"
    
    你需要把上面的都合并成一条提交日志。可以使用rebase功能。

    git rebase -i HEAD~3
    -i表示打开交互模式(interactive mode)。
    HEAD~3表示检查最近3条日志。
    输入的数字过大会导致fatal: Need a single revision错误，可以减小数字。
    随后会通过默认的编辑器(一般是vi)打开一个文本：

      pick cee46ac Add Verification Code Form
      pick 5dd4924 Add Passwd Form
      pick 27dc5ce Add Account Form
  ​
          # Rebase 925891e..cee46ac onto 925891e (3 commands)
          #
          # Commands:
          # p, pick = use commit
          # r, reword = use commit, but edit the commit message
          # e, edit = use commit, but stop for amending
          # s, squash = use commit, but meld into previous commit
          # f, fixup = like "squash", but discard this commit's log message
          # x, exec = run command (the rest of the line) using shell
          # d, drop = remove commit
          #
          # These lines can be re-ordered; they are executed from top to bottom.
          #
          # If you remove a line here THAT COMMIT WILL BE LOST.
          #
          # However, if you remove everything, the rebase will be aborted.
          #
          # Note that empty commits are commented out
    
        需要重点关注的是前3行，后面带#的是注释说明，不会出现在日志中。默认是pick选项，就是保持此条日志不变，其它选项可以看上面的注释，我们的目标是把3条合成一条，并修改日志内容。所以可以使用到f(fixup)与r(reword)选项。最终效果如下。

          r cee46ac Add Verification Code Form
          f 5dd4924 Add Passwd Form
          f 27dc5ce Add Account Form
        保存后，会直接再弹出一个文本编辑器用于重写日志内容：Add login UI.

## 关于rebase各种选项的详细说明。

## 提交变更(Submit PR)
    当所有的commit都准备好时，就可以在网页上选择对应的分支创建PR。最后检查一下

    标题，描述是否简洁清晰？
    如果改动是可见的，是否需要附上一个截图或gif说明？
    想PR被合并后自动关闭对应的issue，可以在描述的结尾加上一行Closes #issue编号。
    如果PR改动很大，你想边改边得到别人及时的反馈，可以先创建PR后，在标题上加上[WIP]是Work In Progress的缩写，表示工作还未完成。但尽量不要把未完成的PR提交到别人的项目上(可能会引起别人反感)，通常WIP的PR都是自己的项目里面使用就行了。

## 审查/合并(Review/Merge)
    PR提交后，维护者会对它进行逐行的审查(review)，大家可以共同讨论，看是否有考虑不周或者更好的方案，在这过程中，你可以根据建议随时改进代码，然后push到分支上，PR就会同步改动。当所有的改动都被批准后，PR应该就会被合并啦！

### 实战篇(Practice)
    目标： 通过提交Pull Request的方式把你的名字署在文末。

    首先Fork本项目，把它变成你自己GitHub下的项目。如果你没有设置好Git，可参照GitHub指引。
    成功后可在自己GitHub账户下看到notes项目。将项目用命令行clone到本地后创建新分支。
    
    git clone https://github.com/你自己GitHub用户名/notes.git
    cd notes
    git checkout -b learn/add-my-name
    然后用文本编辑器打开github-pull-request.md这个文件，在末尾加上你的名字。

    提交修改到远端自己刚才fork的项目中。

     git commit -am "我的第一个PR实验"
     git push origin learn/add-my-name


    通过PR的方式把你fork项目中的变更提交到我的项目中，完成PR!
    被合并后马上就能在文章中看到你的名字啦~

### 同步上游分支(Upstream)
    🙇‍♂️：如何保持fork分支与上游分支(upstream)同步？

    先查看一下你目前git状态。

     $ git remote -v
     origin  https://github.com/YOUR_USERNAME/YOUR_FORK.git (fetch)
     origin  https://github.com/YOUR_USERNAME/YOUR_FORK.git (push)
    以上说明: 你的origin分支指向的是你fork的分支。

### 指定上游地址。
    $ git remote add upstream https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git
    
    upstream分支指向上游地址，这里的upstream名字可以任意指定，只是一般都把上游地址都叫upstream。

### 检查地址是否设置成功。

     $ git remote -v
     origin    https://github.com/YOUR_USERNAME/YOUR_FORK.git (fetch)
     origin    https://github.com/YOUR_USERNAME/YOUR_FORK.git (push)
     upstream  https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git (fetch)
     upstream  https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git (push)
    
    从upstream分支上拉取最新代码。

     $ git fetch upstream
     remote: Counting objects: 75, done.
     remote: Compressing objects: 100% (53/53), done.
     remote: Total 62 (delta 27), reused 44 (delta 9)
     Unpacking objects: 100% (62/62), done.
     From https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY
      * [new branch]      master     -> upstream/master
    
    切换到自己的master上，然后把upstream分支上更新内容合并到master上。

     $ git checkout master
     Switched to branch 'master'
     $ git merge upstream/master
     Updating a422352..5fdff0f
     Fast-forward
      README                    |    9 -------
      README.md                 |    7 ++++++
      2 files changed, 7 insertions(+), 9 deletions(-)
      delete mode 100644 README
      create mode 100644 README.md
      
    这时你的本地master就和上游同步成功啦，但是这只是表示你的本地master，一般你还需要把本地master同步到GitHub下的远程分支上。

    $ git  push origin master