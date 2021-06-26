### Linu常用的命令

> 1）、cd : 改变目录。
>
> 2）、cd . . 回退到上一个目录，直接cd进入默认目录
>
> 3）、pwd : 显示当前所在的目录路径。
>
> 4）、ls(ll):  都是列出当前目录中的所有文件，只不过ll(两个ll)列出的内容更为详细。
>
> 5）、touch : 新建一个文件 如 touch index.js 就会在当前目录下新建一个index.js文件。
>
> 6）、rm:  删除一个文件, rm index.js 就会把index.js文件删除。
>
> 7）、mkdir:  新建一个目录,就是新建一个文件夹。
>
> 8）、rm -r :  删除一个文件夹, rm -r src 删除src目录
>
> ```
> rm -rf / 切勿在Linux中尝试！删除电脑中全部文件！
> ```
>
> 9）、mv 移动文件, mv index.html src index.html 是我们要移动的文件, src 是目标文件夹,当然, 这样写,必须保证文件和目标文件夹在同一目录下。
>
> 10）、reset 重新初始化终端/清屏。
>
> 11）、clear 清屏。
>
> 12）、history 查看命令历史。
>
> 13）、help 帮助。
>
> 14）、exit 退出。
>
> 15）、#表示注释

### git 的配置

> **用户配置**
>
> > 当你安装Git后首先要做的事情是设置你的用户名称和e-mail地址。这是非常重要的，因为每次Git提交都会使用该信息。它被永远的嵌入到了你的提交中：(**其本质上是在写入了一个文件中**)
> >
> > ```bash
> > git config --global user.name "kuangshen"  #名称git config --global user.email 24736743@qq.com   #邮箱
> > ```
>
> **系统配置**
>
> > 查看不同级别的配置文件：
> >
> > ```bash
> > #查看系统configgit config --system --list　　#查看当前用户（global）配置git config --global  --list
> > ```

**git的工作原理**

> Git本地有三个工作区域：工作目录（Working Directory）、暂存区(Stage/Index)、资源库(Repository或Git Directory)。如果在加上远程的git仓库(Remote Directory)就可以分为四个工作区域。文件在这四个区域之间的转换关系如下：
>
> > ![image-20201223224509287](C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20201223224509287.png)
> >
> > - Workspace：工作区，就是你平时存放项目代码的地方
> > - Index / Stage：暂存区，用于临时存放你的改动，事实上它只是一个文件，保存即将提交到文件列表信息
> > - Repository：仓库区（或本地仓库），就是安全存放数据的位置，这里面有你提交到所有版本的数据。其中HEAD指向最新放入仓库的版本
> > - Remote：远程仓库，托管代码的服务器，可以简单的认为是你项目组中的一台电脑用于远程数据交换

**git的工作流程**

> git的工作流程一般是这样的：
>
> １、在工作目录中添加、修改文件；
>
> ２、将需要进行版本管理的文件放入暂存区域；
>
> ３、将暂存区域的文件提交到git仓库。
>
> 因此，git管理的文件有三种状态：已修改（modified）,已暂存（staged）,已提交(committed)
>
> > ![图片](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7Ksu8UlITwMlbX3kMGtZ9p09iaOhl0dACfLrMwNbDzucGQ30s3HnsiaczfcR6dC9OehicuwibKuHjRlzg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

### **克隆仓库的搭建**

> ```
> # 克隆一个项目和它的整个代码历史(版本信息)
> $ git clone [url]  # https://gitee.com/kuangstudy/openclass.git
> ```
>
> 这样就可以下载克隆的代码仓库

**文件状态**

> ```bash
> #查看指定文件状态git status [filename]
> #查看所有文件状态git status
> # git add .                  添加所有文件到暂存区
> # git commit -m "消息内容"    提交暂存区中的内容到本地仓库 -m 提交信息
> ```

**忽略文件**

> 有些时候我们不想把某些文件纳入版本控制中，比如数据库文件，临时文件，设计文件等
>
> 在主目录下建立".gitignore"文件，此文件有如下规则：
>
> 1. 忽略文件中的空行或以井号（#）开始的行将会被忽略。
> 2. 可以使用Linux通配符。例如：星号（*）代表任意多个字符，问号（？）代表一个字符，方括号（[abc]）代表可选字符范围，大括号（{string1,string2,...}）代表可选的字符串等。
> 3. 如果名称的最前面有一个感叹号（!），表示例外规则，将不被忽略。
> 4. 如果名称的最前面是一个路径分隔符（/），表示要忽略的文件在此目录下，而子目录中的文件不忽略。
> 5. 如果名称的最后面是一个路径分隔符（/），表示要忽略的是此目录下该名称的子目录，而非文件（默认文件或目录都忽略）。
>
> ```bash
> #为注释*.txt        #忽略所有 .txt结尾的文件,这样的话上传就不会被选中！!lib.txt     #但lib.txt除外/temp        #仅忽略项目根目录下的TODO文件,不包括其它目录tempbuild/       #忽略build/目录下的所有文件doc/*.txt    #会忽略 doc/notes.txt 但不包括 doc/server/arch.txt
> ```

**配置SSH公钥**

> 一、设置本机绑定SSH公钥，实现免密码登录！（免密码登录，这一步挺重要的，码云是远程仓库，我们是平时工作在本地仓库！)
>
>  ![image-20201224225918494](C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20201224225918494.png)
>
> 二、在码云上配置ssh秘钥设置免密码登录提交

**git集成到idea**

> 在idea的version contrl里面配置git，点击test就会产生git文件
>
> 修改文件，使用IDEA操作git。
>
> - 添加到暂存区（git add . ） 
> - commit 提交
> - push到远程仓库

