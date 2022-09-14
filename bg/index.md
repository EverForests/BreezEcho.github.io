# My Blog Story


## 1. 本地配置

### 1.1 git相关资源下载
下载git

安装相关包管理器，例如npm和yum，包管理器中含有某些博客主题需要的依赖

设置git账号：global、local



### 1.2 hugo下载

上官方网站进行下载，详情请见[hugo release](https://github.com/gohugoio/hugo/releases)，注意下载hugo-extended版，适配性更强。

下载完毕后，将文件路径/bin加入环境变量，重启电脑，打开命令行输入命令`hugo version`，若产生hugo版本号则证明安装完成。



### 1.3 博客的配置

主流的个人博客搭建引擎包括hexo和hugo，这两个文件的主要差别在于主题配置的读取上，hexo博客根目录下的config.yaml文件比theme主题内置的config.yaml文件优先级更低，如果在根目录配置下指明了主题文件信息，那么引擎就会优先渲染后者；hugo与此相反，更侧重于主题文件的更新维护，所以一般需要将主题文件的配置拷贝到根目录下。具体的差异可以参考[hexo与hugo的异同](https://cloud.tencent.com/developer/article/1578634)。

我的博客用的是hugo-lovelt主题，主题配置参考[lovelt官方文档](https://hugoloveit.com/zh-cn/theme-documentation-basics/#26-%E6%9E%84%E5%BB%BA%E7%BD%91%E7%AB%99)即可，根目录主题配置记得参照主题文档examplesite下的.conf文件。

> 在博客的主题的选择上，我踩了很多坑，一些主题文档的维护相当不好。在选择一款喜欢的主题时，一定要关注官方文档的质量哦。



## 2. 云服务器配置

### 2.1 云主机购买与配置

云主机的选择：阿里云、腾讯云、华为云，这三款是主流。

关于云主机的购买，具体可参考acwing-linux相关课程：

[云服务器的购买](https://www.acwing.com/video/3489/)

云主机的配置：

我购买的是腾讯云的轻量级服务器，系统为linux下的ubuntu。在系统分配好云主机后，我们的初始化登录是一个叫ubuntu的账户。与一般的云服务器不同，腾讯云初始化没有root用户，但ubuntu拥有sudo权限。注意sudo权限与root权限有一定差异。具体的差异详见[sudo与root](https://zhuanlan.zhihu.com/p/533703523)。

然后我们可以进行的一些操作：

#### 2.1.1 创建acs用户

建立：

```she
# ubuntu根目录~下
adduser acs  # 创建用户acs
usermod -aG sudo acs  # 给用户acs分配sudo权限
```

查看是否成功：

![image-20220912173534927](https://raw.githubusercontent.com/huansong-dev/PicGo/master/image-20220912173534927.png)

注意当我们`cd acs`或ubuntu时会进入到它们的根目录，但这个电脑下的系统设置文件在/下，也就是说，我们需要cd /才能看到到这台主机下的所有配置文件。同时，我们之后下载的很多文件都会被保存在/etc中。

#### 2.1.2 安装tmux和docker

**tmux安装教程**

```shell
sudo apt-get update  # linux从服务器获取软件更新情况
sudo apt-get install tmux
```

在安装完成后，可以通过scp将一些tmux配置文件上传至云端，启用tmux，然后在命令行进行些许配置即可完成对tmux的美化。

```shell
tmux
# ctrl + b : 进入tmux命令行模式
source-file ~/.tmux.conf # 让tmux配置生效
```

**docker安装教程**

在用户（有sudo权限，比如ubuntu）根目录下，按照[docker-ubuntu 安装说明](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository)中的"Install using the repository"进行安装即可，注意一条条cv执行命令。



### 2.2 域名的购买与ssl证书申请

购买方面，挑选自己喜欢的域名进行购买即可。

在购买完域名后，需要经过实名验证、域名审核等流程。

之后可以申请ssl安全证书，一切按照腾讯云指引操作即可。

在申请完成后，可以对证书进行下载，会得到五个文件，我们只需要将其中的密钥对文件进行移动就行，后缀分别为.crt和.key，目标位置为/etc/nginx

为了网站长期的运行，我们需要对域名进行备案，这是国内的法律要求。



### 2.3 nginx的下载与配置

#### 2.3.1 nginx下载与基础配置

下载方面，可以参照[ubuntu安装nginx](https://blog.csdn.net/qq_41744950/article/details/124259698)，内容非常详细，特别注意不同文件的安装位置，由于root用户初始化一般不存在，所以在安装时应该在sudo权限用户下使用`sudo apt-get install nginx`进行安装。

在安装完成后，我们进行一些基础设置：

```shell
# 设置开机启动nginx
sudo systemctl enable nginx

# 启动nginx
sudo systemctl start nginx

# 查看是否启动nginx
sudo systemctl status nginx

# 逐行执行
```

在浏览器搜索框输入服务器公网ip地址，如果会出现nginx的页面，说明可以导航到服务器，即nginx安装成功。为了使外部用户可以访问web服务器，需要开放防火墙，主要是80端口和443端口，可以在服务器的控制台手动设置，同时也可以命令行设置：

```shell
sudo firewall-cmd --permanent --zone=public --add-service=http
sudo firewall-cmd --permanent --zone=public --add-service=https
sudo firewall-cmd --reload
```



#### 2.3.2 nginx高级配置

仿照[hugo-txcloud-nginx](https://blog.csdn.net/Xuyiming564445/article/details/121973109)中的`nginx.conf`进行配置即可。



## 3. 建立本地与云端的联系

### 3.1 初步连接

使用ssh工具进行远程访问

`ssh username@server_ip`

交互窗口会提醒你输入密码



### 3.2 配置免密登录

在完成过远程访问后，就可以配置免密登录。

在本地进入 ~/.ssh

`ssh-keygen -t rsa `生成密钥，按照指引为密钥添加文件名

> 密钥生成教程可访问 https://blog.csdn.net/qq_40932679/article/details/117487540

然后按照下述命令将公钥添加到远程服务器，最终会被添加在.ssh中的authorized_key文件内

```shell
ssh-agent bash  # 启动ssh高速缓存
ssh-add -i ~/.ssh/txcloud.pub username@server_ip  # 将公钥添加到目标用户端下
```

> 关于ssh-agent可以参考 https://blog.csdn.net/weixin_43972437/article/details/114578337

接着，在.ssh/查看有无config文件，若没有就新建一个

打开config文件，按照如下格式进行配置

```shell
# 腾讯云
Host server1
HostName 43.143.88.173
User acs
IdentityFile ~/.ssh/txcloud

Host server0
HostName 43.143.88.173
User ubuntu
IdentityFile ~/.ssh/txcloud
```

这样我们的免密登录就配置好了，注意到，对不同用户要进行分别配置，但它们所使用的公密钥可以相同。



### 3.3 使用winscp与云端进行图形化文件交互

[winscp下载](https://winscp.net/eng/download.php)

如果连接用户并非root用户，参考[winscp修复远程连接](https://blog.csdn.net/weixin_44532671/article/details/114653203)进行修改



## 4. 建立本地与github的联系

### 4.1 git账号管理

使用邮箱注册github账号，并登录

在本地使用命令`ssh-keygen -t rsa -C "username@server_ip"`生成注释

为“username@server_ip”的密钥对，在生成的过程中可以为密钥对命名。然后将公钥.pub文件的内容进行复制，在github用户setting下的`SSH and GPG keys`模块新建ssh key，key name任意取，将公钥内容添加到下方加密区，完成添加。

回到本地，

如果git存在多账号，那么需要按如下方式进行管理，打开config文件:

```shell
# github user
Host X
Hostname github.com
User BreezEcho
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa_x

# github user
Host Y
Hostname github.com
User huansong-dev
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa_y
```

> 关于git多账号配置详情请见 https://www.cnblogs.com/wanan-happy/p/14588504.html

然后将私钥加入ssh-agent进行管理：`ssh-add id_rsa_x`	、`ssh-add id_rsa_y`

在配置完成后，可以使用`ssh -T git@X`来检验是否完成了连接

为了使用便利，我们可以将常用的账号设置为全局账号

```shell
git config --global user.name username
git config --global user.email useremail
```

如果在某个仓库需要使用特殊账号，可以进行如下操作

```shell
cd base  # 进入该仓库
git config --local user.name username
git config --local user.email useremail
```



### 4.2 建立本地仓库与远程仓库的联系

远程新建名为为`user_id.src`的私人仓库，用于管理博客项目

本地新建仓库hblog，如果是hugo site，执行`hugo new site hblog`

在设置好仓库账号后，进入hblog，并按照如下操作进行关联

```shell
git init  # 初始化为git本地仓库
git remote -v  # 查看关联情况
git remote add origin git@X:username/username.github.io.git  # 建立联系
```



### 4.3 博客项目文件推送

```shell
# 进入博客根目录
touch .nojekyll  # 默认不使用github pages自带的jekyll主题，空文件
git add .  # 为所有文件添加追踪，之后的一切变化都可以通过git status进行查看
git commit -m "ud"  # 将追踪文件的变化上传到本地仓库
git push -u origin master  # 首次推送信息，之后可以将-u省去
```

之后如果远程仓库有变化，需要先`git pull origin master`，可能会出现git merge提醒，这个时候允许合并，然后再执行上述步骤。



## 5. 使用github actions将博客展示文件部署到github pages与云端服务器

> 主要参考：
>
> [hugo双端部署](https://www.liuvv.com/p/35554ffc.html)，给我最大灵感的文章。
>
> [hugo+github action 少数派](https://sspai.com/post/73512)

### 5.1 github actions + public -> github pages

#### 5.1.1 赋予项目仓库访问本账号下其它仓库的特权

新建名为user_id.github.io的公共仓库，注意user_id为你的用户名，这样的名称格式相对固定。这个仓库用来展示我们的博客内容。

进入github用户界面的setting下:

![image-20220913002411963](https://raw.githubusercontent.com/huansong-dev/PicGo/master/image-20220913002411963.png)

选择右上角新建个人密钥，选中repo和workflow，然后点击生成。注意密钥只会出现一次，记得复制。将这里得到的密钥添加到我们项目仓库的actions secrets，并命名为PERSONAL_TOKEN

![image-20220913002757402](https://raw.githubusercontent.com/huansong-dev/PicGo/master/image-20220913002757402.png)

#### 5.1.2 建立github actions工作流

首先，我们将项目仓库的默认分支改为master(setting中)，然后进入项目actions模块，新建工作流文件gh-pages.yml，配置参考如下，可按需求进行修改：

```yaml
name: deploy

on:
    push:
    workflow_dispatch:
    schedule:
        # Runs everyday at 8:00 AM
        - cron: "0 0 * * *"

jobs:
    build:
        runs-on: ubuntu-latest
        steps:
            - name: Checkout
              uses: actions/checkout@v2
              with:
                  submodules: true
                  fetch-depth: 0

            - name: Setup Hugo
              uses: peaceiris/actions-hugo@v2
              with:
                  hugo-version: "latest"

            - name: Build Web
              run: hugo

            - name: Deploy Web
              uses: peaceiris/actions-gh-pages@v3
              with:
                  PERSONAL_TOKEN: ${{ secrets.PERSONAL_TOKEN }}
                  EXTERNAL_REPOSITORY: pseudoyu/pseudoyu.github.io
                  PUBLISH_BRANCH: master
                  PUBLISH_DIR: ./public
                  commit_message: ${{ github.event.head_commit.message }}
```

然后等待一段时间，就可以看到工作流正常，进入博客展示仓库，可以发现多了一个master分支。

一段时间后，访问站点user_id.github.io就可以看到博客内容啦。



### 5.2 github actions + public -> txcloud

#### 5.2.1 给予项目仓库访问云服务器的特权

与本地密钥访问云服务器类似，我们需要先得到一个密钥对，然后分别将公钥和密钥置入服务端与用户端。

在本地`ssh-keygen -t rsa `创建密钥对，参考上述步骤将公钥放入云端用户的authorized_key中，将私钥放入项目仓库的actions secrets内。

> 这个操作告诉我们密钥在哪里产生不重要，重要的是公钥与私钥的位置，就像锁与匙。



#### 5.2.2 维护文件权限

在云主机相应用户目录下新建public空文件夹，通过`ls -al`命令我们发现文件的所有人以及组内用户都是合理的。如果之前public文件位置与此不符，就需要对nginx配置文件进行调整。



#### 5.2.3 更新项目仓库actions文件

在gh-pages.yml文件中添加以下模块，并根据实际情况做一些修改，注意yaml文件的格式。

```yaml
- name: Deploy Tencent Cloud
  uses: wlixcc/SFTP-Deploy-Action@v1.2.4 
  with:  
      username: 'root'   #ssh user name
      server: '${{ secrets.SERVER_IP }}' #引用之前创建好的secret
      ssh_private_key: ${{ secrets.SSH_PRIVATE_KEY }} #引用之前创建好的secret
      local_path: './public'  # 对应我们项目public的文件夹路径，注意不能访问内部文件，因为此处没有令牌
      remote_path: '/home/username/' # 对应云上的目录
```

等待片刻，观察工作流正常，进入云主机对应文件夹，发现文件已更新，同时权限正确。至此，github actions云部署完成。



## 6. 结语

这次经历让我意识到，弯路，也是成长的一部分；同时，在项目中的学习效率是非常高的，当然还需要不断更新信息搜集算法。这篇文章会在后面慢慢丰富，形成一篇合格的教程。


