#环境
* git服务器:centos7.1
* git客户端:win7
* 服务git版本:1.8.3.1
* 客户端git版本:git version 2.13.1.windows.2
#搭建
##1.服务器上安装git
```
yum install git
```
客户端安装比较简单,不再记录

##2.服务器创建git用户

查看服务器上是否存在git用户
```
id git
```

如果不存在创建git用户
```
useradd git
```
设置git用户密码
```
passwd git
```
##3.服务器上创建git仓库
创建工作目录
```
mkdir -p /home/data/git/gittest.git
```
初始化仓库
```
git init --bare /home/data/git/gittest.git/
```
修改仓库的owner
```
chown -R git:git  /home/data/git/gittest.git/
```
##4.客户端远程克隆仓库
先ssh git服务器端,检查连通性,注意,spring cloud目前只支持rsa加密方式,不支持ECDSA.该过程会在用户目录下生成known_hosts文件,该文件用作加密检验(第一登录的时候返回校验值,以后每次登录都会返回一个校验值和该校验值对比)
```
$ ssh -o HostKeyAlgorithms=ssh-rsa  git@10.187.112.151
The authenticity of host '10.187.112.151 (10.187.112.151)' can't be established.
ECDSA key fingerprint is SHA256:mx2lep7+ggp0Wnbwjjf6EB9PjvOQzoZ4EozX4YY4aYA.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '10.187.112.151' (ECDSA) to the list of known hosts.
git@10.187.112.151's password:
-sh-4.2$ exit
logout
Connection to 10.187.112.151 closed.

```
克隆仓库
```
$ git clone git@10.187.112.151:/home/data/git/gittest.git
Cloning into 'gittest'...
git@10.187.112.151's password:
warning: You appear to have cloned an empty repository.
```
检查目录,发现仓库从远程仓库克隆成功

#5.创建SSH无密登录
客户端生成秘钥
```
$ ssh-keygen.exe -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/CP_zhaozhiqiang/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/CP_zhaozhiqiang/.ssh/id_rsa.
Your public key has been saved in /c/Users/CP_zhaozhiqiang/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:PbP/ZkbjqCmq3vTVTUUqVT93zo3UT6bUmumxpPBNYCA CP_zhaozhiqiang@VM-905
The key's randomart image is:
+---[RSA 2048]----+
|        E .   ..o|
|         . . . =.|
|            + +.X|
|         . . = %*|
|        S =   @ =|
|           B Ooo |
|      .   o ++=. |
|     o ... o. =  |
|   .o.o...o..=.  |
+----[SHA256]-----+
```
此时 C:\Users\用户名\.ssh 下会多出两个文件 id_rsa 和 id_rsa.pub

复制公钥到服务器端git用户的工作目录下面
```
$ ssh-copy-id git@10.187.112.151
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/c/Users/CP_zhaozhiqiang/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
git@10.187.112.151's password:

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'git@10.187.112.151'"
and check to make sure that only the key(s) you wanted were added.
```

#验证免密登录
```
ssh git@10.187.112.151
```
如果不需要输入密码,代表成功.

现在,创建一个空目录,再次克隆项目,发现不需要输入密码.

#6.禁止 git 用户 ssh 登录服务器
git用户只需要管理git仓库就可以,不需要登录使用bash,所以要关闭.

编辑 /etc/passwd

找到：
```
git:x:502:504::/home/git:/bin/bash
```
修改为
```
git:x:502:504::/home/git:/bin/git-shell
```

#7.其他问题
spring cloud config如果出现下面问题
```
com.jcraft.jsch.JSchException: reject HostKey: 10.187.112.151
```
请确认./ssh目录下面存在known_hosts文件,并且编码格式是ssh-rsa