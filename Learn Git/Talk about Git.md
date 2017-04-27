http://www.open-open.com/lib/view/open1488975151399.html

# 进入Git

## windows

windows下进入 Git Bash 后，会显示当前在哪个分支，如：`Administrator@Desktop-Win10 MINGW64 /g/CS-Linux (master)`说明在`master`分支上

## Ubuntu

# git commit 规范指南

Git 每次提交代码，都要写 Commit message（提交说明），否则就不允许提交。一般来说，commit message 应该清晰明了，说明本次提交的目的。

目前，社区有多种 Commit message 的写法规范。本文介绍 Angular 规范 是目前使用最广的写法，比较合理和系统化，并且有配套的工具。前前端框架 Angular.js 采用的就是该规范。如下图：

![img](http://static.open-open.com/lib/uploadImg/20170414/20170414103139_565.png)

## Commit message 的作用

### 提供更多的历史信息，方便快速浏览。

比如，下面的命令显示上次发布后的变动，每个commit占据一行。你只看行首，就知道某次 commit 的目的。

```
$ git log <last tag> HEAD --pretty=format:%s
```

![img](http://static.open-open.com/lib/uploadImg/20170414/20170414103139_590.png)

### 可以过滤某些commit（比如文档改动），便于快速查找信息

```
$ git log <last release> HEAD --grep feature
```

### 可以直接从commit生成Change log。

Change Log 是发布新版本时，用来说明与上一个版本差异的文档，详见后文。

![img](http://static.open-open.com/lib/uploadImg/20170414/20170414103139_382.png)

### 其他优点

- 可读性好，清晰，不必深入看代码即可了解当前commit的作用。
- 为 Code Reviewing做准备
- 方便跟踪工程历史
- 让其他的开发者在运行 git blame 的时候想跪谢
- 提高项目的整体质量，提高个人工程素质

## Commit message 的格式

每次提交，Commit message 都包括三个部分：header，body 和 footer。

```
<type>(<scope>): <subject>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```

其中，header 是必需的，body 和 footer 可以省略。

不管是哪一个部分，任何一行都不得超过72个字符（或100个字符）。这是为了避免自动换行影响美观。

### Header

Header部分只有一行，包括三个字段： type （必需）、 scope （可选）和 subject （必需）。

type

用于说明 commit 的类别，只允许使用下面7个标识。

- feat：新功能（feature）
- fix：修补bug
- docs：文档（documentation）
- style： 格式（不影响代码运行的变动）
- refactor：重构（即不是新增功能，也不是修改bug的代码变动）
- test：增加测试
- chore：构建过程或辅助工具的变动

如果type为 feat 和 fix ，则该 commit 将肯定出现在 Change log 之中。其他情况（ docs 、 chore 、 style 、 refactor 、 test ）由你决定，要不要放入 Change log，建议是不要。

scope

scope用于说明 commit 影响的范围，比如数据层、控制层、视图层等等，视项目不同而不同。

例如在 Angular ，可以是 $location , $browser , $compile , $rootScope , ngHref , ngClick , ngView 等。

如果你的修改影响了不止一个 scope ，你可以使用 * 代替。

subject

subject 是 commit 目的的简短描述，不超过50个字符。

其他注意事项：

- 以动词开头，使用第一人称现在时，比如change，而不是changed或changes
- 第一个字母小写
- 结尾不加句号（.）

### Body

Body 部分是对本次 commit 的详细描述，可以分成多行。下面是一个范例。

```
More detailed explanatory text, if necessary.  Wrap it to 
about 72 characters or so. 

Further paragraphs come after blank lines.

- Bullet points are okay, too
- Use a hanging indent
```

有两个注意点:

- 使用第一人称现在时，比如使用change而不是changed或changes。
- 永远别忘了第2行是空行
- 应该说明代码变动的动机，以及与以前行为的对比。

### Footer

Footer 部分只用于以下两种情况：

不兼容变动

如果当前代码与上一个版本不兼容，则 Footer 部分以BREAKING CHANGE开头，后面是对变动的描述、以及变动理由和迁移方法。

```
BREAKING CHANGE: isolate scope bindings definition has changed.

    To migrate the code follow the example below:

    Before:

    scope: {
      myAttr: 'attribute',
    }

    After:

    scope: {
      myAttr: '@',
    }

    The removed `inject` wasn't generaly useful for directives so there should be no code using it.
```

关闭 Issue

如果当前 commit 针对某个issue，那么可以在 Footer 部分关闭这个 issue 。

```
Closes #234
```

### Revert

还有一种特殊情况，如果当前 commit 用于撤销以前的 commit，则必须以revert:开头，后面跟着被撤销 Commit 的 Header。

```
revert: feat(pencil): add 'graphiteWidth' option

This reverts commit 667ecc1654a317a13331b17617d973392f415f02.
```

Body部分的格式是固定的，必须写成 This reverts commit &lt;hash> .，其中的hash是被撤销 commit 的 SHA 标识符。

如果当前 commit 与被撤销的 commit，在同一个发布（release）里面，那么它们都不会出现在 Change log 里面。如果两者在不同的发布，那么当前 commit，会出现在 Change log 的Reverts小标题下面。

---

# Git-工作流

为了更好的使用Git进行项目管理, 一些牛人发明了不同的工作流. 本文是发表的Atlassian网站上的 [文章](https://www.atlassian.com/git/tutorials/comparing-workflows) , 比较了4种常见的Git工作流方式. 可以通过本文了解目前常见的Git工作流.

常见的4种Git工作流:

1. 中心化工作流
2. Feature分支工作流
3. Gitflow工作流
4. Forking工作流

## **中心化工作流**

![img](http://static.open-open.com/lib/uploadImg/20170308/20170308201231_932.png)

这种方式与SVN工作方式相同. 但由于Git的特性, 每个开发者都可以将开发任务保存在本地仓库中, 避免考虑远程服务器的顺序. 并且可以利用Git分支的特性

### **工作方式**

和SVN类似, 中心化工作流使用一个中心仓库保存项目的所有变更. 默认开发分支叫做 master , 所有的变更都提交到这个分支上. 该工作流只需要 master 分支, 不需要额外分支.

开发人员首先从中心仓库克隆代码. 然后在本地的项目中, 编辑代码并提交变更. 这些变更当前保存在本地仓库中, 与中心仓库是隔离的. 这可以让开发人员只在方便的时候同步代码.

开发人员通过push将本地变更推送到中心仓库的master分支.

**处理冲突**

在开发人员推送代码前, 必须先拉取中心仓库的最新代码, 并在此基础上rebase. 也就是将自己的代码增加在别人的代码上. 这样可以产生一个线性的历史.

如果此时产生冲突, Git会暂停rebase并需要由开发人员手动处理冲突.

**示例**

**某人初始化了中心仓库**

首先某人需要在服务器上创建一个中心仓库:

```
ssh user@host git init --bare /path/to/repo.git

```

**所有人从中心仓库克隆代码**

每个开发人员都将项目克隆到本地:

```
git clone ssh://user@host/path/to/repo.git

```

Git会自动使用 origin 作为仓库的别名

**John开始开发某个功能**

John可以在他的本地仓库开发新功能, 使用标准的Git提交流程:

- 编辑代码
- stage
   用于暂存工作目录的部分代码变更, 但不会提交到仓库
- commit
   由于这些变更是发生在本地的, 所以John可以重复该过程, 而不用担心中心仓库的变化

```
git status # 查看仓库状况
git add <some-file> # 暂存一个文件
git commit # 提交一个文件</some-file>

```

**Mary也开始开发另一个功能**

在John开发的同时, Mary开始开发另一个功能, 也使用标准的Git提交流程

和John一样, Mary也不用考虑中心仓库的变化

**John将他的代码推送到中心仓库**

John完成了功能, 他应该讲本地的提交推送到中心仓库, 这样其他成员就可以访问新的代码

```
git push origin master

```

origin 是中心仓库的别名, master 是分支名

由于当前远程分支并没有别人提交代码, 所以不会产生任何冲突

**Mary提交了她的代码**

现在Mary也完成了代码, 她也将代码推送到中心服务器

```
git push origin master

```

但是当前中心仓库已经有了John的新代码, 而Mary本地的代码还是旧的, 所以Git会拒绝推送请求, 并提示以下信息来阻止Mary覆盖最新的代码:

```
error: failed to push some refs to '/path/to/repo.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Merge the remote changes (e.g. 'git pull')
hint: before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.

```

现在Mary需要先拉取中心仓库的最新代码到本地, 将代码整合后, 再推送到中心仓库

**Mary在最新代码基础上rebase**

Mary使用 git pull 来从中心仓库中拉取最新代码

```
git pull --rebase origin master

```

--rebase 选项会使Git将Mary所有的提交移动到master分支的最新部分, 从而组成一条线性的提交历史, 如图:

- 如果忘记该选项, 也是可以的. 但会在其他人同步中心仓库时产生一个 merge commit
- 在中心工作流下, 推荐使用rebase美化提交历史, 而不要生成merge commit

**Mary解决了冲突**

Rebase会将每个本地commit一个接一个地传递给最新的master分支. 也就是说你会在每次commit都处理合并冲突, 而不是在一次大的合并冲突中一次性处理. 这可以保持你的commit尽可能地专注, 也可以保持一个干净的项目历史. 最终使你更容易找到bug出现的位置, 如果需要回滚的话, 也能把对项目的影响降到最低.

如果Mary和John工作在不相关的功能上, Rebase一般不会产生冲突. 但如果真的产生了冲突, Git会暂停当前commit的rebase, 并输出如下信息, 同时携带一些相关说明:

```
CONFLICT (content): Merge conflict in <some-file>

```

Git最伟大的事情是任何人都可以解决自己的合并冲突. 在本例中, Mary只需要简单的执行 git status 来查看问题所在. 冲突文件会出现在Unmerged paths部分:

```
# Unmerged paths:
# (use "git reset HEAD <some-file>..." to unstage)
# (use "git add/rm <some-file>..." as appropriate to mark resolution)
#
# both modified: <some-file>

```

然后, Mary编辑该文件. 如果结果合适, 她可以暂存这些文件, 然后执行 git rebase 来完成其他事情:

```
git add <some-file>
git rebase --continue

```

大功告成. Git将会移动到下一个commit, 并对其他冲突执行重复的操作.

如果你到这一步后不知道该如何继续, 不要慌张. 只需要执行以下命令, 你就可以回退到执行 git pull --rebase 的时候:

```
git rebase --abort

```

**Mary成功发布了功能**

在Mary与中央仓库同步后, 他就可以发布她的变更:

```
git push origin master

```

### **接下来做什么**

如你所见, 使用Git命令便可方便地替换传统SVN开发环境. 这对于传统SVN团队是好的, 但这并不是Git的本质.

如果你的团队喜欢使用中心化工作流, 但希望追踪合作的详细内容, 那么 Feature分支工作流 值得一试. 通过为每一个功能声明一个单独的分支, 你便可以在新功能合并到项目之前进行深入的讨论.

## **Feature分支工作流**

一旦你掌握了 中心化工作流 , 那么在开发过程中增加feature分支则是一件非常简单的事, 这样做可以鼓励开发者合作, 并追踪讨论.

Feature分支工作流的核心理念是所以的功能开发应当发生在一个单独的分支上, 而不能在master分支. 这种方式使得多个开发者可以工作在某个功能上, 而不会干扰主要代码. 同时也意味着master分支永远不会包含有问题的代码, 这对持续集成环境具有极大的优势.

将功能开发包裹起来还能实现 pull request , 它是一种对分支进行讨论的方式, 能够在某个功能合并到项目之前让其他开发者有机会看到. 或者是你在某个功能中间遇到困难, 你可以创建一个pull request来询问同事有什么好的建议. 关键点在于, pull request让团队评论每个人的工作变得极其简单.

### **如何工作**

Feature分支工作流仍然使用中央仓库, master仍然代表主要项目历史. 但是开发者在每次开始工作时, 都要创建一个新的分支. 功能分支应当有一个具有描述性的名称, 例如 animated-menu-items 或 issue-#1061 . 这是为了让每个分支有一个清晰, 专注的目标.

Git对待master分支和feature分支没有任何技术上的区别, 所以开发者可以像在中心化工作流一样编辑, 暂存, 提交变更到一个feature分支.

此外, feature分支也可以被推送到中央仓库. 这样可以在不影响主要代码的情况下, 与其他开发者分享某个feature. 由于 master 分支是唯一的”特殊”分支, 在中央仓库保存多个功能分支不会产生任何影响. 当然, 这也便于每个人保存各自的本地提交.

### **Pull Request**

除了隔离功能开发之外, 分支的变更还可以通过 pull request 进行讨论. 当某人完成了一个功能, 他们不需要立即合并到master中, 而是将功能分支推送到中央仓库, 然后发起一个pull request来请求合并到master中. 这种方式可以让其他开发者有机会在合并代码之前审查变更.

代码审查是pull request的主要优势, 但是他们实际上是被设计为讨论代码的. 你可以把pull request看作是某个分支的讨论. 例如, 如果一个开发者的某个功能需要帮助, 那就可以发起一个pull request. 与此相关的人会收到通知, 他们可以很快地查看问题.

当pull request被接受时, 所进行的操作同中心化工作流类似. 首先, 你需要确保本地的master已经同步为最新. 然后, 将功能分支合并到master分支, 再将更新后的master分支推送到中央仓库.

### **示例**

本例演示使用pull request进行代码审查.

**Mary开始了一个新功能**

在Mary开始开发新功能之前, 她需要工作在一个单独的分支. 她可以使用如下命令创建一个新的分支:

```
git checkout -b marys-feature master

```

上面命令会从 master 分支创建一个新的 marys-feature 分支, -b 标志告诉Git如果分支不存在的话则创建一个. 在这个功能分支上, Mary像之前一样编辑, 暂存, 提交代码:

```
git status
git add <some-file>
git commit

```

**Mary去吃午饭**

Mary在她的功能分支上增加了一些commit. 在她去吃午饭前, 最好将她的功能分支推送到中央仓库. 这相当于一个备份, 如果Mary同其他开发者共同协作的话, 这样做也可以让其它开发者访问她的commit.

```
git push -u origin marys-feature

```

该命令将 marys-feature 推送到中央仓库, -u 标志将其添加为远端追踪分支. 在设置追踪分支后, Mary可以不使用任何参数调用 git push 来推送他的功能

**Mary完成了功能**

当Mary吃完午饭回来后, 她完成了她的功能. 在合并到 master 之前, 他需要发起一个pull request来让其他人知道她做了什么. 但首先, 她应该确保中央仓库有她最新的提交:

```
git push

```

然后, 她在Git图形化界面中发起一个pull request, 请求将 marys-feature 合并入 master , 其他成员会自动收到通知. pull request最伟大的事是它可以在相关commit中显式评论, 所以可以很方便地对某段变更提出问题.

**Bill收到了pull request**

Bill收到了pull request, 并查看了 marys-feature . 他认为需要在合并之前做一些改动, 之后他和Mary通过pull request进行了一系列的磋商.

**Mary修改代码**

Mary使用相同的方式来修改代码. 她编辑, 暂存, 提交, 推送变更到中央仓库. 他的所有动作都会在pull request中记录并显示, 而Bill也可以继续评论.

如果Bill愿意的话, 他可以将 marys-feature 拉取到他的本地仓库中, 自己来操作代码. 他的所有commit也会显示在pull request中.

**Mary发布了功能**

当Bill接受了pull request后, 需要有人来将功能分支合并到主分支中(这可以由Bill或Mary来做):

```
git checkout master
git pull
git pull origin marys-feature
git push

```

首先, 无论谁来执行合并, 都需要切换到他的 master 分支, 并拉取最新代码. 然后使用 git pull origin marys-feature 将 marys-feature 合并到中央仓库的代码副本中. 你也可以使用简单的 git merge marys-feature , 但之前的命令可以保证你能够拉取到最新的功能分支代码. 最后, 更新后的 master 分支要被推送回 origin .

这一流程通常会产生一个merge commit. 有些开发者喜欢这样, 因为这可以表示功能的合并. 但如果你喜欢线性的历史, 你可以在合并之前, 在 master 上将功能分支rebase进去.

有些图形界面会在点击 接受 按钮后自动处理pull request的合并.

与此同时, John也做了相同的事

当Mary和Bill在 marys-feature 分支讨论pull request时, John也在他的功能分支上做着相同的事. 通过使用不同的分支来隔离功能, 每个人都可以独立工作.

### **接下来做什么**

feature分支工作流对于开发项目来说非常的弹性. 但问题是有时过于弹性. 对于大一些的团队, 通常会为不同的分支指派更多的角色. Gitflow工作流则是管理功能开发, 发布准备, 维护的常用模式.

## Gitflow工作流

Gitflow工作流是围绕项目发布定义的一种严格的分支模型. 要比Feature分支工作流更为复杂, 为大型项目管理提供了全新的框架.

该工作流没有比Feature工作流增加任何新的概念或命令. 而是为不同的分支指派了更为具体的角色, 并定义了这些分支如何以及何时进行交互. 它为准备, 维护, 发布都使用了独立的分支. 同时你也可以使用Feature分支工作流的所有特性: pull request, 隔离实验, 更有效率的协作.

### **如何工作**

Gitflow工作流仍然使用中央仓库作为所有开发者的交流中心. 唯一的区别是分支结构的不同.

### **历史分支**

除了单独的 master 分支, 该工作流使用两个分支来记录项目的历史. master 分支存储主要发布历史, develop 分支用于feature分支的整合. 同时使用tag位 master 分支来标记版本号.

### **功能分支**

每个新功能应当在单独的分支上开发, 同时能够推送到中央仓库用于备份或协作. 但在这里功能分支是从 develop 分离出来的. 当一个功能完成时, 该功能分支也会合并到 develop 分支中. 功能分支不能直接同 master 分支交互.

### **发布分支**

当 develop 分支拥有足够多的功能, 适合发布的时候, 你需要从 develop 分支分离一个 release 分支. 创建该分支意味着进入了下一个发布周期, 所以在这一点之后不能增加任何新的功能, 而只能在 release 分支上增加bug fix, 文档生成, 或是其他与发布相关的任务. 当可以发布的时候, release 分支会合并到 master 分支中, 并对master分支使用版本号打上标签. 然后将 release 合并回 develop 分支.

使用单独的分支用作发布可以让一个团队在完善当前发布版本的同时, 由另一个团队继续为下一次发布功能进行开发. 同时也创建了良好的开发环境(例如, 现在可以很容易的说”本周我们开发4.0版本”, 在仓库中也可以看到该版本).

常用惯例:

- 从 develop 分支分离
- 合并入 master 分支
- 命名惯例: release-* 或 release/*

### **维护分支**

维护分支, 或叫做 hotfix 分支, 通常用于对发布版本进行快速修复. 这是唯一可以直接从 master 分支中分离出来的分支. 当修复完毕后, 该分支应该同时合并到 master 和 develop 分支(或是当前的release分支), 此外 master 分支应该使用新的版本号打标签.

使用单独的分支进行bug修复可以让团队在不影响其他工作或发布周期的情况下进行工作.

### **示例**

本例演示了如何使用该工作流完成一个发布周期

**创建一个开发分支**

第一步是从 master 分支创建一个 develop 分支. 最简单的做法是在本地创建一个空的 develop 分支, 将其推送到服务器:

```
git branch develop
git push -u origin develop

```

该分支会包含项目的完整历史, 而 master 只包含版本片段. 其他开发者现在应该从中央仓库克隆并检出开发分支:

```
git clone ssh://user@host/path/to/repo.git
git checkout -b develop origin/develop

```

现在每个人都有一个本地分支副本.

**Mary和John开发新功能**

John和Mary开始在一个单独的分支上工作. 他们都需要为相应的功能创建单独的分支. 他们应该从 develop 上创建分支:

```
git checkout -b some-feature develop

```

他们都使用相同的方式在功能分支上提交代码: 编辑, 暂存, 提交:

```
git status
git add <some-file>
git commit

```

**Mary完成了功能**

Mary增加了几个commit之后, 她感觉功能已经开发完毕. 如果她的团队使用pull request, 那么现在便可以创建一个pull request来请求将他的功能合并入 develop . 如果没有使用pull request的话, 她可以将功能分支合并入本地的 develop 分支, 然后将 develop 分支推送到中央仓库:

```
git pull origin develop
git checkout develop
git merge some-feature
git push
git branch -d some-feature

```

第一条命令保证 develop 分支是最新的. 注意功能分支不能直接合并到 master 分支中. 冲突可以像中心化工作流相同的方式解决.

Mary准备发布

当John仍然在他的功能上工作时, Mary开始准备发布项目. 同功能开发一样, 她使用单独的分支来包裹发布的准备. 这一步也是发布版本号建立的地方:

```
git checkout -b release-0.1 develop

```

这一分支是为了理清发布代码, 进行测试, 更新文档, 以及其他与发布相关的事.

当Mary创建该分支并将其推送到中央仓库后, 发布功能便已冻结. 任何不在 develop 分支的功能都必须推迟到下一个发布周期.

**Mary完成发布**

当 release 分支准备就绪时, Mary将 release 分支合并到 master 和 develop 分支中, 然后删除 release 分支. 注意, 将代码合并会 develop 分支非常重要, 因为一些关键的更新可能是在 release 分支更新的. 如果Mary的团队强调代码审查, 那么此时是提交pull request的最好时机:

```
git checkout master
git merge release-0.1
git push
git checkout develop
git merge release-0.1
git push
git branch -d release-0.1

```

发布分支作为功能开发(develop)和发布(master)的缓冲. 当你合并到 master 中时, 你应该为commit打标签用于参考

```
git tag -a 0.1 -m "Initial public release" master
git push --tags

```

Git有一些hook, 当某个特定行为在仓库内发生时, 会执行相应的脚本. 可以配置一个hook, 当推送 master 分支或推送标签到中央仓库时, 自动创建一个发布版本

**用户发现一个bug**

发布版本后, Mary与John继续开发后续的功能. 此时有一个用户开启了一个工单, 投诉当前发行版中的一个bug. 为了处理这个bug, Mary(或是John)从 master 分支创建了一个维护分支, 修复了问题, 然后直接合并回 master :

```
git checkout -b issue-#001 master
# Fix the bug
git checkout master
git merge issue-#001
git push

```

与发布分支类似, 维护分支包含重要的更新, 也需要被合并到 develop 中, 所以Mary也需要进行这项合并. 然后便可删除维护分支:

```
git checkout develop
git merge issue-#001
git push
git branch -d issue-#001

```

### **接下来做什么**

现在你应该了解了中心化工作流, feature分支工作流, Gitflow工作流.

要记住, 工作流只是演示可以做什么, 并不是死板的规定. 所以不用担心你会不赞同某些方式. 最终的目标是让Git为你工作.

## **Forking工作流**

Forking工作流与上面讨论的工作流完全不同. 这种工作流为 *每一个* 开发者提供一个单独的服务端仓库, 而没有中央仓库. 这意味着每个开发者都有两个仓库: 本地私有仓库, 公开的服务端仓库.

Forking工作流的优势在于不需要每个人都把代码推送到同一个中央仓库, 便可集成代码. 开发者只需要推送到他们各自的服务端仓库, 唯一的项目管理员推送到官方仓库. 管理员可以在不提供其他开发者访问官方仓库权限的情况下接受commit

这样可以为大型, 组织化的团队(包括不可信的第三方)提供一个弹性的方式来安全的协作. 也是开源项目的理想工作流.

### **如何工作**

同其他工作流一样, Forking工作流从官方仓库开始. 但是当新的开发者开始工作时, 他们并不直接从官方仓库克隆代码.

他们会 **fork** 官方仓库并在服务器上创建一个官方仓库代码的副本. 这份副本作为他们个人的公开仓库 - 其它开发者不可以对其进行push, 但是其它开发者可以从该仓库拉取变更(一会儿说为什么这样做). 当他们在服务端创建了副本后, 开发者执行 git clone 将副本拷贝到本地机器, 作为本地私有的开发环境.

当他们准备发布本地提交时, 他们要将commit推送到自己的公开仓库中, 而不是官方仓库. 然后, 他们向官方仓库发起一个pull request, 让项目管理员知道有变更. pull request也可以用于讨论问题.

管理员拉取开发者的变更到本地仓库, 检查是否有问题, 然后将其合并到本地的 master 分支, 然后推送到服务器官方仓库的 master 分支. 现在代码已经是项目的一部分. 其他开发者可以从官方仓库同步到本地仓库.

### **官方仓库**

理解”官方”仓库非常重要. 从技术角度将, Git不会把任何仓库看做官方仓库. 实际上项目管理员的仓库就是官方仓库.

### **Forking工作流的分支**

所有的私人公开仓库只是便于同其他开发者分享代码. 每个人仍然需要使用分支来隔离独立的功能, 就像Feature分支工作流和Gitflow工作流一样. 唯一的区别是这些分支的分享方式. 在Forking工作流中, 他们会被拉取到其他开发者的本地仓库, 而在Feature工作流和Gitflow工作流中他们会被推送到官方仓库.

### **示例**

项目管理员初始化了官方仓库

任何基于Git的项目, 第一步是在服务端创建一个团队成员都可以访问的官方仓库. 一般来说, 该仓库也会作为管理员的公开仓库.

公开仓库应该是空的, 无论他们是否代表官方代码. 所以项目管理员应该执行以下代码来建立官方仓库:

```
ssh user@host
git init --bare /path/to/repo.git

```

**开发者fork官方仓库**

下一步, 所以其他开发者需要fork这个官方仓库. 可以使用 git clone 来操作, 或是使用网站提供的fork按钮.

之后, 每个开发者自己的服务端仓库. 同官方仓库一样, 它们都是空的仓库.

**开发者克隆它们fork的仓库**

然后每个开发者需要克隆它们自己的公开仓库. 可以使用 git clone 命令.

本例假设使用Bitbucket托管仓库.

```
git clone https://user@bitbitcket.org/user/repo.git

```

与其他工作流使用单一的 origin 远端不同, Forking工作流使用两个远端 - 一个用于官方仓库, 另一个用于开发者自己的服务端仓库. 你可以给这些远端取任意名称, 通常使用 origin 作为你fork的仓库的远端名称(在你git clone时会自动创建), 使用 upstream 作为官方仓库的远端名称.

```
git remote add upstream https://bitbucket.org/maintainer/repo

```

你需要使用上面的命令创建upstream远端. 这可以让你保持本地仓库与官方项目同步. 注意如果你的upstream仓库开启了验证(比如它不是开源的), 逆序要提供一个用户名:

```
git remote add upstream https://user@bitbucket.org/maintainer/repo.git

```

然后会要求用户在克隆或拉取官方代码之前提供一个有效的密码

**开发者在各自功能上工作**

在他们克隆的本地仓库中, 开发者像其他工作流一样编辑代码, 提交变更, 创建分支:

```
git checkout -b some-feature
# Edit some code
git commit -a -m "Add first draft of some feature"

```

他们的所有变更都是完全私有的, 直到他们推送到各自的公开仓库. 如果官方仓库有任何更新, 他们可以使用 git pull 拉取最新commit:

```
git pull upstream master

```

由于开发者都工作在单独的功能分支上, 这通常会产生fast-forward合并

**开发者发布他们的功能**

当开发者准备好分享他们的新功能时, 他们需要做两件事. 首先, 他们需要推送代码到公开仓库, 以便其他开发者能够访问. 他们的 origin 远端应该已经建立, 所以只需要做以下:

```
git push origin feature-branch

```

这里的 origin 是开发者自己的服务端仓库, 而不是官方代码.

第二, 他们需要通知项目管理员合并代码到官方仓库. Bitbucket提供了 Pull Request 按钮来使用表单实现该功能. 一般情况下, 你会希望将你的功能分支合并到upstream远端的 master 分支

**项目管理员合并功能**

当项目管理员接收到pull request时, 他们的工作是决定是否将它们合并到官方代码中. 他们可以做以下两件事之一:

1. 在pull request中直接检查代码
2. 将代码拉取到他们本地仓库, 手动合并

第一种方式更为简单, 可以让管理员查看变更的diff, 进行评论, 并使用图形化界面进行合并. 然而, 当pull request产生冲突时, 第二个选项也是必须的. 当产生冲突时, 管理员需要从开发者的服务端仓库fetch功能分支, 将他们合并到本地 master 分支, 然后解决冲突:

```
git fetch https://bitbucket.org/user/repo feature-branch
# Inspect the changes
git checkout master
git merge FETCH_HEAD

```

当变更集成到本地 master 后, 管理员要将其推送到服务端的官方仓库, 这样其他开发者便能够访问:

```
git push origin master
```

记住, 管理员的origin指的是他的公开仓库, 也就是官方代码仓库.

**开发者同步官方仓库代码**

由于主仓库代码更新了, 其他开发者应该同步官方仓库:

```
git pull upstream master

```

### **接下来做什么**

如果你之前使用SVN, 那么Forking工作流是一个颠覆性的改变. 但不必担心 - 它所做的一切只是基于Feature分支工作流的高层次抽象.

---



# git merge & git rebase 

> git merge和git rebase两个都是用来合并两个分支用的

`git merge b_brance`  # 将 b_brance 分支合并到当前分支上

# git checkout <branch_name>

