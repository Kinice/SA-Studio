# SA-Studio

## 说明
本git库秉承着开源、共享、互助的原则建立，开放给SA-Studio成员进行资源分享。

同时，本git库也可以作为你的git学习第二站。你可以在有理论基础之后，随意在这个库中按照规则进行改动，用来练习基本的git操作，比如基本的add、commit、push、pull，disk、暂存区和分支相关的checkout、reset、branch、merge、rebase，最终理解github多人协作的fork、pull request是什么意思。

*声明：本库不限于本小组成员使用，但本小组成员对本库有优先使用权和修改权。*

## 使用方法说明：

1. 将本git库fork到自己的地盘下。
2. 将自己fork来的git库clone到你的本地硬盘上。
3. 进入SA-Studio文件夹，建立一个以自己名称（包括但不限于自己的名字、昵称、代号等，但必须让我们看的出你是谁）命名的文件夹，将你自己的文件、项目以及一切你想共享的资源放进去。
4. **在每次push以前，请首先同步最新的改动，否则可能会在pull request的时候出现冲突。所有冲突请在本地解决，有冲突的pull request将被打回。**
5. 同步父git库的方法：

	1. 在自己的本地，执行`git remote add upstream https://github.com/Kinice/SA-Studio.git`。
	2. 每次要同步的时候，执行`git fetch upstream`，`git merge upstream/master`，有冲突解决冲突。
6. 将改动push到你的远程仓库。
7. 在自己地盘的此git库中，点击new pull request，说明分享内容，提交，等待内容审核。没有意义的更新内容将被废弃！
8. 静待merge。
9. 如果你是萌新，对git本身不了解，建议先去看[廖雪峰大大的git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)「号称史上最浅显易懂的Git教程」。
9. 之后有兴趣可以看看我下面写的：《理解git日常操作》（逃。如果想继续进阶就可以看《Git权威指南》或者官方文档走起。

## 理解git日常操作

### 前言

git已经是一个非常普及且常用的代码管理工具，算是程序员标配之一。但近期面试了大约几十个3-5年前端工程师外加实习生，发现大多数的git水平都停留在add、commit、push、pull这个最简单的流程，只会用GUI，甚至一点都没接触过的也大有人在。

所以我想从git工作区划分开始，解释一下平时常用的一些git命令的真正含义，希望看过这篇文章的朋友能对git本身有一定的认识，知道用不同的命令时git进行了哪些操作，更好的利用好git这个优秀的工具。

阅读本文需要知道git是什么东西，以及一点简单的命令使用。萌新请看这里：[廖雪峰大大的git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)。

### git是用来管理修改的

当我们对一个项目进行修改后，需要对改动进行add、commit来把改动合并到本地分支，然后执行push来将这次的commit推送到分支关联的远程服务器的分支，这其实就是最简单的git工作流程，这样就可以把本地修改的代码上传到服务器了。

那么，git的作用就仅仅是将本地的代码放到服务器上吗？显然不是，要不然git就仅仅是一个网盘了。

git与网盘的不同点之一在于：git管理的是**文件的修改**，是过程量，就像是物理中的加速度。

当你在本地修改文件之后，执行`git status`就会看到一片红色的文件名：

![](https://github.com/Kinice/SA-Studio/blob/master/SunZhaopeng/WechatIMG19.jpeg)

如上图，有三个文件的名字是红的，分成了两个部分：

1. Changes not staged for commit
2. Untracked files

第一个，我们看到主语是Changes，也就是说，这一段是**还没有被staged，以用来提交的修改**。关于staged的含义，先理解成「声明、暂存」，之后会详细解释。

第二个是**没有被跟踪的文件**。

从此我们可以看出，git对于文件是这样处理的：首先，对于新文件，提交过一次之后，git就会跟踪这个文件。每当文件内容进行修改，git就会**按行**分析出这个文件修改了哪些行。对于这些修改，用`git diff <filename>`就可以查看到：

![](https://github.com/Kinice/SA-Studio/blob/master/SunZhaopeng/WechatIMG20.jpeg)

对于程序员来说，写代码的量上来之后，或者你修改的文件多了以后，可能就会出现忘掉修改了哪些内容之类的问题。git就会帮我们跟踪这些文件，把你的修改以简单明了的方式展现出来。对于我这么个前端来说，diff让我减少了一大堆语法错误（逃。

### 把git抽象成几个区域

我们已经知道git管理的是文件的修改了，那么有关于这些修改的提交都是一些什么过程呢？

git将我们的工作流程抽象成了几个区域，总结一下就是：

* 工作区（就是你的磁盘，你的文件）
* 暂存区（刚才提到的stage）
* 版本库（分支）
* 远程代码库（远程分支）

如图所示：

![](https://github.com/Kinice/SA-Studio/blob/master/SunZhaopeng/WechatIMG4119.jpeg)

好，就根据此图，归结一下我们一次工作并提交代码的过程吧：

1. 写代码（修改工作区的文件）
2. 提交加入到暂存区：`git add <filepath>`
3. 把暂存区的修改提交到版本库中：`git commit`
4. 此时本地版本库就跟远程库状态不同了，把本地库推送到远程库保持同步：`git push <origin> <branch>`

如图所示：

![](https://github.com/Kinice/SA-Studio/blob/master/SunZhaopeng/WechatIMG21.jpeg)

接下来再讲一讲具体各个区域代表了什么：

#### 工作区

其实工作区没什么好讲的，这个区域其实就是抽象了你的磁盘文件本身。但是，单独抽象出工作区对于git概念的整体理解是相当有必要的。

比如说，工作区其实是整个git流程的源头，你对工作区的修改其实就是git要保存的对象。所以，当你开始修改工作区之后，**一路向前推进的流程中工作区的内容是绝对不会变的**。

但是，当你发现你修改错误之后，就可以借用版本库和暂存区的内容修改工作区。比如：

* 将暂存区的改动同步到工作区，就是`git checkout`。
* 将版本库的代码同步到工作区，就是`git reset HEAD <commitId> --hard`。

这种对于工作区的修改其实是**非常危险**的：如果你将改动checkout掉，在没有手动备份的情况下你修改的这部分内容就找不回来了。所以，修改代码要谨慎，checkout要谨慎，版本回退要谨慎。

#### 暂存区

暂存区的概念虽然在所有git教程中都是重点提到的内容，但是实际工作中大部分同学对此都是懵懵懂懂。我认为，这是因为暂存区的英文翻译导致的╮(╯▽╰)╭。

暂存区的英文名称叫**stage**。这个stage呢，既是个名词也是个动词，但是都没有“存储”的意思，这是很多人疑惑的地方。我们从暂存区的作用来看看stage到底是什么意思。

在用git提交的过程中，每当工作区发生了修改，`git status`就会显示你修改的文件为红色，代表这些文件发生过修改，上面提到了，这些文件在git里叫做“Changes not staged for commit”。而当你执行`git add [filepath]`之后，这些文件就变成了绿色，如图：

![](https://github.com/Kinice/SA-Studio/blob/master/SunZhaopeng/WechatIMG22.jpeg)

绿色的文件git称之为“Changes to be committed”，就是将要被提交的修改。也就是说，**暂存区会集中一批修改**，统一提交成一个commit。

举个例子，git的提交流程就像是一个生产流水线：你先咔咔生产一批产品（修改文件），然后把生产好的文件放到一个存放的地方（git add），当你觉得这批货不管是质量还是数量都OK了，就把这批货打包装箱（git commit）。

这样就明白暂存区是啥意思了吧。所以我认为，暂存区存在的意义就是**规范你的每一次commit**。

还有很多人不理解暂存区和工作区的关系，有一种情况可以加深对其的理解：当你把一个东西add到暂存区之后，再次从工作区修改这个文件，会出现这样的情况：

![](https://github.com/Kinice/SA-Studio/blob/master/SunZhaopeng/WechatIMG23.jpeg)

这说明暂存区和工作区是分开的。暂存区保存的是**当你add时的修改**，而你再次修改工作区时，git又会检测到你的修改跟暂存区中的不一样，就出现了未暂存和已暂存同时出现的情况，这时继续`git add`，暂存区中的修改就会集中这两次的修改内容，然后commit就会把两次修改提交成同一个了。

最后，已经在暂存区中的修改如何去掉，git已经提示了：“`git reset HEAD <file>...`”。观感上，就是把暂存区中的某些修改从中删去了，实际上，这个**`git reset`的操作是从版本库同步代码到暂存区中**的意思。也就是说，在这里的`git reset`完成了从版本库修改暂存区的操作。

而暂存区是如何修改文件区的呢？ 在说暂存区时已经提到过，答案就是`git checkout <filename>`啦。checkout就是“检出”的意思，当出现上面图中暂存区和工作区都有修改的时候，checkout会从暂存区“检出”修改到工作区中，使工作区与暂存区同步（关于checkout其他用法后面再说）。


