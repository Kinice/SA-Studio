# 本博客阿里云配置过程（二）Node线上环境配置篇

上一篇咱们说到了服务器购买和备案，做完那一套就可以拥有一台自己的服务器啦！哈哈哈哈哈

可惜仅有一台服务器并没有什么卵用，如果不是买的配置好的镜像，就得装一下运行环境。我博客的运行环境是Node＋MongoDB，所以要安一下这俩。先说Node。

我这里默认读者的linux水平为入门，完全不懂的请学习下基本的再去买服务器，大神请略过部分解释内容～

## Node安装

由于我选择的是CentOS系统，没有Win/Mac上相应的安装包，需要下载源代码进行编译安装，或者直接下载编译好的文件，调整位置。

注意：node运行环境需求c++，编译需求g++ 4.8以上以及python 2.6或2.7，先保证这几个东西安装好，不然会出n多麻烦。比如我当时选择的是centOS6.5，yum的默认g++版本最高到4.2，编译一直不通过，yum安装不上去，只能手动安装4.8的g++以及g++依赖，特别麻烦。导致我直接换了centOS7。

### 下载安装

几种方式，我采用的是第二种：

1. 在你自己的电脑上下载好，然后用filezilla之类的工具传过去。如果你的应用、资源都需要使用filezilla之类的工具，下载好之后看方法2和3的后面。

2. 进入到你想存储Node的目录，使用`wget`命令下载源代码source code：

       $ wget https://nodejs.org/dist/v6.2.0/node-v6.2.0.tar.gz  #这是当前的最新版，请自动替换为自己想要的版本
下载好之后，解压并安装：

       $ tar xvf node-v6.2.0.tar.gz  #解压
       $ cd node-v6.2.0              #进入解压后的文件夹
       $ make                        #编译
       $ make install				 #安装
这么安装完以后应该是不能把node当做一个命令来用的，使之进化：

      $ cp /usr/local/bin/node /usr/sbin/   #第一个地址是安装完成后node在系统中的地址，第二个是系统命令的存储目录，此操作会将node作为命令添加到系统
copy命令完成以后你的node就进化成`node`了～
试一下：

      $ node -v
    
      v6.2.0
完美！

3. 你也可以不下载源代码编译，选择下载编译好的文件，直接进入目录，就可以直接运行：
       
       $ cd node-v6.2.0-linux-x64/bin/
       $ ./node -v
       v6.2.0
当然，也可以配置全局命令的：

       $ ln -s /home/kinice/node-v6.2.0-linux-x64/bin/node /usr/local/bin/node
       $ ln -s /home/kinice/node-v6.2.0-linux-x64/bin/npm /usr/local/bin/npm

由于新版的Node已经集成了npm，直接试试`npm -v`看是否安装成功即可。

这样，Node环境就配置ok啦。
    