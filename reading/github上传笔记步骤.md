先装一下Typora和git

注册一个GitHub账号

新建一个 New repository  （new-notes）

本地创建一个公钥

在设置里面找到SSH and GPG keys

在终端新建一个key

命令：ssh-keygen -o

找到 id_rsa.pub 复制里面的ssh加到GitHub上的SSH keys

进到new-notes仓库 复制SSH git@github.com:xinxian1/new-notes.git

然后去终端 

git clone git@github.com:xinxian1/new-notes.git

cd new-notes

git status

然后创建需要的笔记

上传到远端 git status

增加所有刚刚新增的文件 git add .

git commit -m "add all"

然后可能会出现错误 把邮箱和名字存一下就可以了

Author identity unknown                 作者身份不明

*** Please tell me who you are.          请告诉我你是谁。

Run

  git config --global user.email "you@example.com" 
  git config --global user.name "Your Name"

to set your account's default identity.
Omit --global to set the identity only in this repository.

fatal: unable to auto-detect email address (got '0VJ.(none)')

出现这个就输入一下就行

git config --global user.email "123423213@qq.com"

git config --global user.name "xinxian1"

重新输入一次  git commit -m "add all"

git push

就传上去了