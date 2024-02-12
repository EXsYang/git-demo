## 1 设置ssh代理（终极解决方案）


https代理存在一个局限，那就是没有办法做身份验证，每次拉取私库或者推送代码时，都需要输入github的账号和密码，非常痛苦。
设置ssh代理前，请确保你已经设置ssh key。可以参考[在 github 上添加 SSH key](https://link.zhihu.com/?target=https%3A//tjfish.github.io/posts/%E5%9C%A8github%E4%B8%8A%E6%B7%BB%E5%8A%A0SSH-key/) 完成设置
更进一步是设置ssh代理。只需要配置一个config就可以了。

```bash
# Linux、MacOS
vi ~/.ssh/config
# Windows 
到C:\Users\your_user_name\.ssh目录下，新建一个config文件（无后缀名）
```


将下面内容加到config文件中即可

对于windows用户，代理会用到connect.exe，你如果安装了Git都会自带connect.exe，如我的路径为C:\APP\Git\mingw64\bin\connect

```text
#Windows用户，注意替换你的端口号和connect.exe的路径
ProxyCommand "D:\Java_developer_tools\Git\Git\mingw64\bin\connect" -S 127.0.0.1:7890 -a none %h %p

#MacOS用户用下方这条命令，注意替换你的端口号
#ProxyCommand nc -v -x 127.0.0.1:7890 %h %p

Host github.com
  User git
  Port 443
  Hostname ssh.github.com
  # 注意修改路径为你的路径
  IdentityFile "C:\Users\yangd\.ssh\github01_id_rsa"
  TCPKeepAlive yes

Host ssh.github.com
  User git
  Port 443
  Hostname ssh.github.com
  # 注意修改路径为你的路径
  IdentityFile "C:\Users\yangd\.ssh\github01_id_rsa"
  TCPKeepAlive yes
```


保存后文件后测试方法如下，返回successful之类的就成功了。

```text
# 测试是否设置成功
ssh -T git@github.com
```

测试成功如下：

~~~shell
xx@F2 MINGW64 /d/Java_developer_tools/GithubRepository/git-demo (master)
$ ssh -T git@github.com
The authenticity of host '[ssh.github.com]:443 (<no hostip for proxy command>)' can't be established.
ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[ssh.github.com]:443' (ED25519) to the list of known hosts.
Hi EXsYang! You've successfully authenticated, but GitHub does not provide shell access.

~~~



之后都推荐走ssh拉取代码，再github 上选择clone地址时，选择ssh地址，入下图。这样`git push` 与 `git clone` 都可以直接走代理了，并且不需要输入密码。

![img](https://raw.githubusercontent.com/EXsYang/PicGo-images-hosting/main/images/v2-0e58a44f513be7005919b320222f2985_720w.webp)

###  原理部分


代理服务器就是你的电脑和互联网的中介。当您访问外网时（如[http://google.com](https://link.zhihu.com/?target=http%3A//google.com)) , 你的请求首先转发到代理服务器，然后代理服务器替你访问外网，并将结果原封不动的给你的电脑，这样你的电脑就可以看到外网的内容啦。
路径如下：

> 你的电脑->代理服务器->外网
> 外网->代理服务器->你的电脑


很多朋友配置代理之后，可以正常访问github 网页了，但是发现在本地克隆github仓库（git clone xxx）时还是报网络错误。那是因为git clone 没有走你的代理，所以需要设置git走你的代理才行。







## 2 Git 配置多个 SSH Key

链接地址：https://help.gitee.com/enterprise/code-manage/%E6%9D%83%E9%99%90%E4%B8%8E%E8%AE%BE%E7%BD%AE/%E9%83%A8%E7%BD%B2%E5%85%AC%E9%92%A5%E7%AE%A1%E7%90%86/Git%E9%85%8D%E7%BD%AE%E5%A4%9A%E4%B8%AASSH-Key

### 背景[](https://help.gitee.com/enterprise/code-manage/权限与设置/部署公钥管理/Git配置多个SSH-Key#背景)

同时使用两个 Gitee 帐号，需要为两个帐号配置不同的 SSH Key：

- 帐号 A 用于公司；
- 帐号 B 用于个人。



### 解决方法[](https://help.gitee.com/enterprise/code-manage/权限与设置/部署公钥管理/Git配置多个SSH-Key#解决方法)

1. 生成帐号 A 的 SSH Key，并在帐号 A 的 Gitee 设置页面添加 SSH 公钥：

```bash
ssh-keygen -t ed25519 -C "Gitee User A" -f ~/.ssh/gitee_user_a_ed25519
```



1. 生成帐号 B 的 SSH-Key，并在帐号 B 的 Gitee 设置页面添加 SSH 公钥：

```bash
ssh-keygen -t ed25519 -C "Gitee User B" -f ~/.ssh/gitee_user_b_ed25519
```



1. 创建或者修改文件 `~/.ssh/config`，添加如下内容：

```bash
Host gt_a
    User git
    Hostname gitee.com
    Port 22
    IdentityFile ~/.ssh/gitee_user_a_ed25519
Host gt_b
    User git
    Hostname gitee.com
    Port 22
    IdentityFile ~/.ssh/gitee_user_b_ed25519
```



1. 用 ssh 命令分别测试两个 SSH Key：

```text
$ ssh -T gt_a
Hi Gitee User A! You've successfully authenticated, but GITEE.COM does not provide shell access.

$ ssh -T gt_b
Hi Gitee User B! You've successfully authenticated, but GITEE.COM does not provide shell access.
```



1. 拉取代码：

将 `git@gitee.com` 替换为 SSH 配置文件中对应的 `Host`，如原仓库 SSH 链接为：

```text
git@gitee.com:owner/repo.git
```



使用帐号 A 推拉仓库时，需要将连接修改为：

```text
gt_a:owner/repo.git
```



## 3 github ssh 设置



要想使用ssh免密连接到GitHub，需要修改配置文件如下：

位置在 `C:\Users\yangd\.ssh\config`



~~~
#config文件配置
#Windows用户，注意替换你的端口号和connect.exe的路径
#，下面这行在使用代理的情况下，如github时需要打开
#，在使用gitee时需要注销掉
ProxyCommand "D:\Java_developer_tools\Git\Git\mingw64\bin\connect" -S 127.0.0.1:7890 -a none %h %p

#MacOS用户用下方这条命令，注意替换你的端口号
#ProxyCommand nc -v -x 127.0.0.1:7890 %h %p

Host github.com
  User git
  Port 443
  Hostname ssh.github.com
  # 注意修改路径为你的路径
  IdentityFile "C:\Users\yangd\.ssh\github01_id_rsa"
  TCPKeepAlive yes


Host ssh.github.com
  User git
  Port 443
  Hostname ssh.github.com
  # 注意修改路径为你的路径
  IdentityFile "C:\Users\yangd\.ssh\github01_id_rsa"
  TCPKeepAlive yes
~~~



## 4 gitee ssh 设置

~~~
#Windows用户，注意替换你的端口号和connect.exe的路径
#，下面这行在使用代理的情况下，如github时需要打开
#，在使用gitee时需要注销掉
#ProxyCommand "D:\Java_developer_tools\Git\Git\mingw64\bin\connect" -S 127.0.0.1:7890 -a none %h %p

#MacOS用户用下方这条命令，注意替换你的端口号
#ProxyCommand nc -v -x 127.0.0.1:7890 %h %p

Host github.com
  User git
  Port 443
  Hostname ssh.github.com
  # 注意修改路径为你的路径
  IdentityFile "C:\Users\yangd\.ssh\github01_id_rsa"
  TCPKeepAlive yes


Host ssh.github.com
  User git
  Port 443
  Hostname ssh.github.com
  # 注意修改路径为你的路径
  IdentityFile "C:\Users\yangd\.ssh\github01_id_rsa"
  TCPKeepAlive yes
  
Host gt_a
    User git
    Hostname gitee.com
    Port 22
    IdentityFile ~/.ssh/gitee_user_a_ed25519
	
~~~



需要配置好

![image-20240212104004645](https://raw.githubusercontent.com/EXsYang/PicGo-images-hosting/main/images/image-20240212104004645.png)



![image-20240212103938206](https://raw.githubusercontent.com/EXsYang/PicGo-images-hosting/main/images/image-20240212103938206.png)

测试ssh免密登录是否成功如下：

~~~
yangda@F2 MINGW64 /d/Java_developer_tools/GiteeRepository/gitee_hsp_java/新建文件夹/daqiao-java-project-001 (master)
$ ssh -T gt_a
Hi CodeYang(@kapaiya)! You've successfully authenticated, but GITEE.COM does not provide shell access.

~~~

注意：使用gitee免密登录不可以使用代理，同时需要把生成的公钥配置的gitee

即使开着clash也可以使用gitee ssh免密登录，注销上面这一行，同时需要在该文件中配置过gitee的Host，

​								，如下面这种样式的

~~~
Host gt_a
    User git
    Hostname gitee.com
    Port 22
    IdentityFile ~/.ssh/gitee_user_a_ed25519
~~~

![image-20240212105007023](https://raw.githubusercontent.com/EXsYang/PicGo-images-hosting/main/images/image-20240212105007023.png)





## 5 C:\Users\yangd\.ssh\config 文件的最终配置

~~~
#Windows用户，注意替换你的端口号和connect.exe的路径
#，下面这行在使用代理的情况下，如github时需要打开
#，在使用gitee时需要注销掉
#，最好使用github时将下面这行打开才好，但是不打开也行，clash使用TUN模式，偶尔也能ssh连接上
#ProxyCommand "D:\Java_developer_tools\Git\Git\mingw64\bin\connect" -S 127.0.0.1:7890 -a none %h %p


#MacOS用户用下方这条命令，注意替换你的端口号
#ProxyCommand nc -v -x 127.0.0.1:7890 %h %p

Host github.com
  User git
  Port 443
  Hostname ssh.github.com
  # 注意修改路径为你的路径
  IdentityFile "C:\Users\yangd\.ssh\github01_id_rsa"
  TCPKeepAlive yes


Host ssh.github.com
  User git
  Port 443
  Hostname ssh.github.com
  # 注意修改路径为你的路径
  IdentityFile "C:\Users\yangd\.ssh\github01_id_rsa"
  TCPKeepAlive yes
  
Host gt_a
    User git
    Hostname gitee.com
    Port 22
    IdentityFile ~/.ssh/gitee_user_a_ed25519
	
~~~

