## welcome to use git ##
**git** 分布式版本控制系统。以前只使用过vss、svn等集中式版本控制系统。集中式版本控制系统是集中存放在服务器的。需要编辑的时候,首先从服务器获取最新版本。在本地编辑完毕之后，再推送给服务器。如果服务器停止工作了。那么整个工作就无法开展了。而*分布式*系统则不存在这一问题。*分布式*版本控制系统，不存在集中控制，可以本地编辑、提交完毕之后，再推送到远端,比如(**github**)。两相比较，*分布式*版本控制系统的优点十分明显。

作者在windows(win7 64位)上使用**git**，要使用git，需要先安装(貌似废话)

![软件界面截图](http://ww3.sinaimg.cn/large/40c685fcjw1ezxx5118j8j20fe030wf5.jpg  "软件界面截图")
上图是我本机的截图。

完成安装后，需要进行设置，在终端输入：

$ git config --global user.name 'your name'  
$ git config --global user.email 'myemail.com'

*git*是分布式版本控制系统，需要每个机器都自报家门：

注意git config命令的--global参数，使用了这个参数，表示本机所有的仓库都会使用这个配置。

##版本库##
版本库，又叫仓库，英文名repository,可以简单理解为一个文件夹。然后你创建的东西，都可以放到这里。

仓库创建后，就可以使用了。如  
$ cd /d  进入d盘

$ makedir learngit 创建learngit目录

$ cd learngit 
然后，通过git init命令 就可以创建仓库了

如果创建成功会有提示
Initialized empty Git respository in D:/learngit

这样即表示仓库创建成功了。此时，在此目录下你会看到一个.git的目录
1.把文件添加到版本库
  例如，我们创建一个readme.txt的文件 注意编码使用utf-8无bom格式
作者使用notepad++创建。添加如下内容
Git is a version control system.
Git is free software.
  使用如下命令，将文件添加到仓库。
$ git add readme.txt
执行此命令，终端没有任何显示。
注释：在使用add命令时 可以一次添加多个文件 中间使用空格分开

2.将文件提交到版本库
  使用如下命令，将文件提交到仓库
$ git commit -m 'commit a file'
然后系统会有反馈信息。

解释：-m 后面输入的是提交版本的注释信息 建议使用有意义的描述。这样如果在查找版本时，便于快速理清，每次提交都进行了哪些操作

###查看版本信息###

当我们成功添加并提交了文件之后，可以使用git status命令查看结果

git status命令 可以让我们清楚地查看仓库的当前状态 

可以知道 哪些文件被修改了 英文提示
<pre>
-- changes not staged for commit 文件尚未添加到暂存区

-- Changes to be commited        文件未提交
    
-- nothing to commit             文件均已提交   
</pre>

git status命令只告知文件的修改或提交的错略信息。

如需详细了解文档的变化 可以使用命令 git diff(顾名思义就是查看不同的意思) 如
$ git diff readme.txt
然后终端会列出详细的修改信息。

###版本回退###

使用git log 命令 可以查看版本的提交信息
注释： 
<pre>
1. 此命令列出的提交信息是按倒序排列的
2. 如果加上 --pretty=oneline 一行模式 可以只列出提交的版本号和注释信息 
3. f06eb54ed61698d13914da1b786fb34da506c432类似这样的一串代码 即为commit ID.
</pre>

然后 再使用git reset命令 就可以进行版本回退了
$ git reset --hard f06eb54ed6
即可回退至相应版本 
注释：
commit ID 不必完全写全 只要前几位 有区分度就可以了 **git**会自己去找 

另外 git reflog 可以查看命令历史。
####工作区和暂存区####

   工作区就是你能在电脑上看到的文件夹。比如用例中的learngit文件夹。
   工作区中的.git目录 才是版本库。版本库里存了很多东西，比较重要的有两个 一个是被称为stage(或叫index)的暂存区 还有git为我们自动创建的第一个分支master 以及指向master的指针head 如图

![结构图](http://ww1.sinaimg.cn/large/40c685fcjw1ezyssjqv4mj20g208k3z7.jpg  "结构图")

我们把文件往git版本库里添加的时候 是分两步执行的：

第一步： git add把文件添加进去，实际上是把文件修改添加到暂存区。

第二部： git commit提交更改， 实际上是把暂存区的所有内容(注意:提交时 所有文件一块提交)提交到当前分支。

由于我们创建git版本库时， git为我们自动创建了master分支， 所以 当我们使用 git commit 命令时 就是向master分支进行提交。

再讲了暂存区的概念之后 需要指出 当我们使用 git commit 命令时,只会讲已经被 git add 过的即进入到暂存区的文件 进行提交。也就是说， 如果我们只在工作区修改了文件 而没有进行git add,那么当我们进行时git commit操作时 实际上是无法提交那些只在工作区做了修改的文件的。

简言之 每次工作区修改 如果不进行git add到暂存区 则无法进入到commit队列。


###撤销修改###

使用命令 git checkout --<file>

分为两种情况：

[1]. 文件在工作区修改后还没有被添加到暂存区 那么撤销修改就会回到和版本库一致的状态。

[2]. 文件在工作区已经修改并且已被添加暂存区 如果此时想撤销操作 那么需要进行两步操作：<pre>
  (1) 使用git reset head <file> 将暂存区的文件进行一步回退
  (2) 使用git checkout <file> 使用版本库的文件覆盖工作区的文件
  </pre>


###删除文件###
在git中， 删除也是一个修改操作。

如果 我们已经提交了某个文件 并且在工作区使用 rm 命令 将文件删除了
那么我们可以使用 git rm <file> 命令将文件从版本库删除 并且commit.
如果是本地误删除 我们可以使用checkout 命令将文件恢复。


## 远程仓库 ##

只要注册一个**github**账号， 就可以免费获得git远程仓库。

由于本地git仓库和github仓库之前传输是是通过SSH加密的， 所以首先需要进行一些设置。
[1]. 创建SSH Key。<pre>
     使用命令 ssh-keygen -t -C "myemail@aa.com"
     </pre>
可以一路回车 免密码设置。如果设置完成 可以在用户主目录里找到.ssh目录。里面有id_rsa和id_rsa.pub两个文件。这两个是SSH Key对应的密钥对。id_isa是私钥， id_rsa.pub是公钥，可以告知任何人、网站。

[2]. 登录github,点击如图所示的操作(站点UI等内容可能会发生变化)

![github_add_ssh](http://ww4.sinaimg.cn/large/40c685fcjw1ezyuxklowmj20a80bbdgp.jpg "github_add_ssh")

然后再点击SSH keys--> Add SSH key

然后填写Title框 和Key文本框即可
Title框的内容 可自行填写
Key文本框的内容 即为刚才id_rsa.pub文件里的所有内容(可以使用notepad++打开) 粘贴即可。

注释： github需要识别推送内容的作者，防止冒充。github免费托管git库，任何人都可以看到(仅有授权用户可以更改)。

### 添加远程仓库 

如图所示

   ![github_add_respo](http://ww1.sinaimg.cn/large/40c685fcjw1ezyvaag19nj208m049jrq.jpg      "github_add_respo")

在**Repository name**处填入库名即可。
最后点击 Create respository 完成远程库的创建。

假定 我们在刚才创建了learngit的远程仓库。那么就可以向远程仓库进行关联并推送了。

可以使用如下命令进行关联：

$ git remote add origin git@github.com:github账户名/learngit.git

添加后 ， 远程库的名字就是origin ， 这是git的默认叫法。

关联后， 使用git push -u origin master 进行推送(如无问题，那么github上就会有本地库的内容了，是为完成) 。

注释： 在进行第一次使用git的clone或者push命令时 会得到一个警告

The authenticity of host 'github.com (xx.xx.xx.xx)' can't be established.
RSA key fingerprint is xx.xx.xx.xx.xx.
Are you sure you want to continue connecting (yes/no)?

指纹信息(fingerprint) 如担心冒充 可以自行比对[github网站的RSA Key的指纹信息](https://help.github.com/articles/what-are-github-s-ssh-key-fingerprints/ "github_rsa_fingerprint")

这是因为ssh连接在第一次验证github网站的key时，需要你确认github的key来自github服务器。如验证无误 输入yes即可

这时 git会输出一个警告，告诉你已经把github的key添加到本机的一个信任列表里了。

次警告只出现一次，后面的操作就不会再次警告了。

暂时记录到这里。



