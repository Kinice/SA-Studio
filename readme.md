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
9. 之后有兴趣可以看看我下面写的：《理解git结构与简单操作》（逃。如果想继续进阶就可以看《Git权威指南》或者官方文档走起。

# 理解git结构与简单操作

## 前言

git已经是一个非常普及且常用的代码管理工具，算是程序员标配之一。但近期面试了大约几十个3-5年前端工程师外加实习生，发现大多数的git水平都停留在add、commit、push、pull这个最简单的流程，只会用GUI，甚至一点都没接触过的也大有人在。

所以我想从git工作区划分开始，解释一下平时常用的一些git命令的真正含义，不过并不会太过深入的解释git的一些偏门内容。希望看过这篇文章的朋友能对git本身有一定的认识，知道用不同的命令时，git产生了哪些改变，做到使用命令时心里有数，更好的利用好git这个优秀的工具。

阅读本文需要知道git是什么东西，以及一点简单的命令使用。萌新请看这里：[廖雪峰大大的git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)。

本文会把相对于git各个区域或者操作涉及到的git命令总结在段尾，可以结合段中内容回想涉及到的命令在这里完成了什么作用，最后再综合思考命令的作用。

个人才疏学浅，文笔粗陋，如文中有知识、文笔错误，或者有什么可以改进的地方，请随时提issue，或发E-mail（szp93@126.com）给我，感谢感谢。

## git是用来管理修改的

当我们对一个项目进行修改后，需要对改动进行add、commit来把改动合并到本地分支，然后执行push来将这次的commit推送到分支关联的远程服务器的分支，这其实就是最简单的git工作流程，这样就可以把本地修改的代码上传到服务器了。

那么，git的作用就仅仅是将本地的代码放到服务器上吗？显然不是，要不git就仅仅是一个网盘了。

git与网盘的不同点之一在于：git管理的是**文件的修改**，是过程量，就像是物理中的加速度。

当你在本地修改文件之后，执行`git status`就会看到一片红色的文件名：

![](https://github.com/Kinice/SA-Studio/blob/master/SunZhaopeng/WechatIMG19.jpeg)

如上图，有三个文件的名字是红的，分成了两个部分：

1. Changes not staged for commit
2. Untracked files

第一部分，这句话的主语是Changes，也就是说，这一段是**还没有被staged，以用来提交的修改**。关于staged的含义，先理解成「保存」，之后会详细解释。

第二个是**没有被跟踪的文件**。

从此我们可以看出，git对于文件是这样处理的：对于新文件，提交过一次之后，git就会跟踪这个文件。每当文件内容进行修改，git就会**按行**分析出这个文件修改了哪些行。对于这些修改，用`git diff <filename>`就可以查看到：

![](https://github.com/Kinice/SA-Studio/blob/master/SunZhaopeng/WechatIMG20.jpeg)

对于程序员来说，写代码的量上来之后，或者你修改的文件多了以后，可能就会出现忘掉修改了哪些内容之类的问题。git就会帮我们跟踪这些文件，把你的修改以简单明了的方式展现出来。对于我这么个前端来说，diff让我减少了一大堆语法错误（逃。

## 把git抽象成几个区域

我们已经知道git管理的是文件的修改了，那么有关于这些修改的提交都是一些什么过程呢？

git将我们的工作流程抽象成了几个区域，总结一下就是：

* 工作区（就是你的磁盘，你的文件）
* 暂存区（刚才提到的stage）
* 版本库（分支）
* 远程代码库（远程分支）

如图所示：

![](https://github.com/Kinice/SA-Studio/blob/master/SunZhaopeng/WechatIMG4119.jpeg)

好，就根据此图，归结一下我们一次工作并提交代码的过程吧：

1. 写代码（修改工作区）
2. 把修改加入到暂存区：`git add <filepath>`
3. 把暂存区的修改提交到版本库中：`git commit`
4. 此时本地版本库就跟远程库状态不同了，把本地库推送到远程库保持同步：`git push <origin> <branch>`

如图所示：

![](https://github.com/Kinice/SA-Studio/blob/master/SunZhaopeng/WechatIMG21.jpeg)

下面就是各个区域的具体解释。

### 工作区

其实工作区没什么好讲的，这个区域其实就是抽象了你的磁盘文件本身。但是，单独抽象出工作区对于git概念的整体理解是相当有必要的。

比如说，工作区其实是整个git流程的源头，你对工作区的修改其实就是git要保存的对象。所以，当你修改工作区完成（写完代码），**将修改从工作区提交到远程库的流程中工作区的内容是绝对不会变的**。简而言之，在提交的流程中只有你自己去修改文件，git不会去动。

但是，当你发现你修改错误之后，就可以借用git版本库和暂存区的内容反向修改工作区。比如：

* 将暂存区的改动同步到工作区，就是`git checkout`。
* 将版本库的代码同步到工作区，就是`git reset HEAD <commitId> --hard`。

这种对于工作区的修改其实是**非常危险**的：如果你将改动checkout掉，在没有手动备份的情况下你修改的这部分内容就找不回来了。所以，修改代码要谨慎，checkout要谨慎，版本回退要谨慎。

*涉及操作：*`git checkout`, `git reset HEAD <commitId> --hard`

### 暂存区

暂存区的概念虽然在所有git教程中都是重点提到的内容，但是实际工作中大部分同学对此都是懵懵懂懂。我认为，这是因为暂存区的英文翻译导致的╮(╯▽╰)╭。

暂存区的英文名称叫**stage**。这个stage呢，既是个名词也是个动词，但是都没有「存储」的意思，这是很多人疑惑的地方。我们从暂存区的作用来看看stage到底是什么意思。

在用git提交的过程中，每当工作区发生了修改，`git status`就会显示你修改的文件为红色，代表这些文件发生过修改，上面提到了，这些文件在git里叫做「Changes not staged for commit」(git status可以显示工作区修改、暂存区文件以及你本地分支相对于远程分支的情况)。而当你执行`git add <filepath>`之后，这些文件就变成了绿色，如图：

![](https://github.com/Kinice/SA-Studio/blob/master/SunZhaopeng/WechatIMG22.jpeg)

绿色的文件git称之为「Changes to be committed」，就是将要被提交的修改。也就是说，**暂存区会集中一批修改**，统一提交成一个commit。

举个例子，git的提交流程就像是一个生产流水线：你先咔咔生产一批产品（修改文件），然后把生产好的文件放到一个存放的地方（git add到暂存区），当你觉得这批货不管是质量还是数量都OK了，就把这批货打包装箱（git commit）。

这样就明白暂存区是啥意思了吧。所以我认为，暂存区存在的意义就是**规范你的每一次commit**。你可以把暂存区想象成一个分支的一次commit的内容，执行`git commit`提交时就把暂存区这个commit放到现有分支的顶端。

还有很多人不理解暂存区和工作区的关系，有一种情况可以加深对其的理解：当你把一个文件的修改add到暂存区之后，再次从工作区修改这个文件，会出现这样的情况：

![](https://github.com/Kinice/SA-Studio/blob/master/SunZhaopeng/WechatIMG23.jpeg)

这说明暂存区和工作区是分开的。暂存区保存的是**当你add时修改的内容**，而你再次修改工作区时，git又会检测到你的修改跟暂存区中的不一样，就出现了未暂存和已暂存同时出现的情况，这时继续`git add`，暂存区中的修改就会集中这两次的修改内容，然后commit就会把两次修改提交成同一个了。

综上所述，暂存区*stage*的英文含义像是一个阶段性的平台，用来保存你即将打包成一次commit的提交们。从英文含义角度理解，这个词儿其实还是挺生动形象的。（emmmmmm......）

而暂存区的修改是如何同步到文件区呢？ 在说暂存区时已经提到过，答案就是`git checkout <filename>`啦。checkout就是「检出」的意思，当出现上面图中暂存区和工作区都有修改的时候，checkout会从暂存区「检出」修改到工作区中，使工作区与暂存区同步（关于checkout其他用法后面再说）。

最后，已经在暂存区中的修改如何去掉，git已经提示了：「`git reset HEAD <file>...`」。观感上，就是把暂存区中的某些修改从中删去了，实际上，这个**git reset的操作是从版本库同步代码到暂存区中**的意思。也就是说，在这里的`git reset`完成了使用版本库的代码修改暂存区的操作。

*涉及操作：*`git status`, `git add`, `git commit`, `git checkout`, `git reset HEAD <file>`

### 版本库

版本库，也就是git的分支库，其实是git最核心的部分。当我们提到git时，其实就是在说git分支。如果你是一个个人开发者，你的项目只有一个人在开发，永远只有一个master分支，那么你可能永远接触不到分支的内容，你对git的了解会永远停留在「了解」这一阶段。

#### 版本库的基本组成

版本库这个名称来源，就是它管理的是一个个「版本」。

版本库是由分支组成的，分支则是由commit组成的。**commit就是版本库的基本单位**。在git中，每个都commit有一个独一无二的「commit id」，也就是说通过这个id你可以精确定位到某一个commit上。所以，为什么git要求每次commit的时候必须为commit添加明确的commit message，并且要专门开辟出stage暂存区来管理每一次commit的内容（svn就没有暂存区），就是为了能够随时进行commit（代码版本）层面的操作，明确每一次commit，保证出现问题时能快速定位，以及完成版本回退等操作。

#### 关于分支

简单的说，分支就是由commit串成的一条**时间线**，就是这样子的：

![](/Users/sunzhaopeng/Desktop/WechatIMG25.jpeg)

上面的每个圈圈都是一次commit。

在新建git仓库时，git会自动给你创建一条分支，叫master，你之后所做的所有操作都是在这个master分支上操作。不过，**并不要把一个分支就理解为是这样的一条时间线**。这里可能有点绕，也是git中最抽象的一个点之一。就是：

master分支其实是一个**指针**，它指向一个commit，代表了**在这条时间线上，master指针指向的那个commit时间点的代码状态**。可能对于指针概念由基础的同学会更好理解一点。如下图：

![](/Users/sunzhaopeng/Desktop/WechatIMG26.jpeg)

当我们建立一个分支时，使用两种方式：

* git branch <branch name>      (仅创建分支)
* git checkout -b <branch name> (创建分支并切换到该分支)

这样子建立的分支指针会指向目前你所在的那个分支(master)，假如我们创建了一个分支叫dev，那么我们的时间线就变成了这个样子：

![](/Users/sunzhaopeng/Desktop/WechatIMG27.jpeg)

可以看到，时间线还是那条时间线，不过已经有两条分支同时存在了，只是这两条分支都指向同一个commit，所以两条分支内容完全相同，git也就只会指一下指针，不会对代码和时间线做任何操作。但是，抽象来看，你可以把这两条分支看作是两条线。

而我们怎么确定哪一条分支是我们当前的分支呢？也是指针，不过是一个指向指针的指针，叫做HEAD。HEAD指针在git中算是个关键字，HEAD就是当前指向当前分支的指针的意思。我们只用`git branch dev`创建分支时，HEAD指针不会做改变，当前分支依然是master分支，当前状态如图所示：

![](/Users/sunzhaopeng/Desktop/WechatIMG28.jpeg)

当我们在当前这种状态下，将暂存区中的内容进行一次提交时，会将时间线向前推进，生成一个commit，然后把当前分支master指向最新的commit。如图所示：

![](/Users/sunzhaopeng/Desktop/WechatIMG29.jpeg)

当切换分支时，我们会使用`git checkout <branch name>`，将HEAD指针指向切换过去的那个分支名。我们是不是见到了老朋友checkout！在前面的暂存区我讲过，checkout作为**检出**操作是将暂存区中的内容同步到工作区的方法。这里的checkout依然是**检出**的意思，只不过检出的对象变成了**相应分支的文件**。其实我们可以把暂存区想象成一个分支，暂存区的内容就是commit的内容，这两种操作的意义就保持一致了，只不过检出分支时HEAD指针会移动。checkout文件**会修改工作区**。所以，执行`git checkout dev`后，分支状态就变成了这样：

![](/Users/sunzhaopeng/Desktop/WechatIMG30.jpeg)

而上面说过，checkout会修改工作区，所以你磁盘上的文件就变成了dev分支上那次commit时的样子。而在创建时提到的`git commit -b <branch name>`则同时完成了「创建分支」和「检出分支」两个操作，方便得很。

使用`git branch -av`就可以查看所有分支的信息了（a是all，v是verbose）。

*涉及操作：*`git branch <branch name>`, `git checkout <branch name>`

#### 合并分支

此时的dev分支与master分支的进度就不一样了，所以需要将dev分支与master分支同步。这里需要的就是合并分支的操作，大家应该都知道用`git merge`或者`git rebase`。

##### git merge

当出现我们上面图中的那种情况时，时间线只有一条，dev分支只不过是落后master分支而已。此时我们在dev分支上执行`git merge maseter`时，git就仅仅会把dev分支指针移动到master分支所在的位置，就变成这样了：

![](/Users/sunzhaopeng/Desktop/WechatIMG27.jpeg)

这种merge的方式叫做「fast-forward」，也是git默认的merge方式。

如果情况改变了，举个例子：我们在开发过程中，一直使用的是master分支，这时出了一个很严重的bug，我们就需要建立一个叫topic的分支来处理这个bug，但主要的功能工期又不能拖，所以master分支与topic分支就同时向前推进，此时时间线如图所示(此图出自git自己的帮助文件，使用命令`git help merge`即可看到。想看其他命令的帮助就`git help <command>`即可)：

![](/Users/sunzhaopeng/Desktop/WechatIMG31.jpeg)

这时候，topic上的bug修改完毕，需要合并回master分支，需要的操作为：切换到master分支`git checkout master`，合并dev`git merge dev`。

注意，这时候的这两条分支是真正的「分支」了，他们在时间线上岔开了，各自分支都有自己独有的东西。因为此时的topic分支的末端并不在master分支的父端，需要把不同的修改同步起来，单纯的指针移动不能完成这一步，fast-forward方式也就不可能实现了。

这时，**git便会将两个分支不同的地方取出，合并成一个commit，然后把master指针指向这个新的commit**（就是在master上生成了一次commit）。这样，topic分支上的修改就同步到master分支上了。此时分支情况如图：

![](/Users/sunzhaopeng/Desktop/WechatIMG32.jpeg)

BTW，能够进行fast-forward的merge情况下，也可以通过增加`--no-ff`命令来强制不使用fast-forward模式。






