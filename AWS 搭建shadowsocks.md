亚马逊AWS免费套餐EC2安装Amzon Linux连接登录并创建root并搭建shadowsocks
---
注册亚马逊AWS免费套餐ec2

---
前言：
	

刚开始使用亚马逊的AWS的免费套餐EC2，由于个人习惯使用centos系统，所以果断安装，但是AWS为了安全性，默认禁止用户使用root账户，导致安装配置环境各种问题。所以我把从安好系统后遇到的一些基本问题整理出来，方便大家更快的入手！

---
涉及到的问题：
1. 如何在aws上安装centos系统 
2. 如何删除创建好的vps服务器 
3. 安装的centos系统后，如何在windows上远程登录ec2 
4. 在ec2上创建root账户，并且使用root账户登录


第1块.aws上怎么安装centos之类的系统:
---
1. 准备工具一张visa或者万事达的信用卡（可在某宝购买注册虚拟卡）
2. 注册完成后可能会有24小时的审核时间
3. 创建ec2实例。可在右上角选择数据中心
4. 如果实例创建好后，报出没有权限的错误，请发邮件给aws-verification@amazon.com，向其说明要求使用你所选择区域的数据中心的使用权

第2块：vps服务器创建好后如何删除
---
也是挺简单的，服务器上”右键“，点”终止“，看图说话吧。有的朋友估计会说删了半天还在，我只能说多等会吧，在等的过程中你可以重新创建，一个月750小时的配额，按31天算才744小时，偶尔搞下没问题，不扣费！

第3块：创建好系统后如何登录服务器
---
当时我的心情可以用一个词形容”蛋疼“，很疼，本来就是半把刷子，普通的root登录还行，一下搞个publickey，完全蒙掉了
方法其实很简单，官方在你创建完成后会给中文教程，用putty工具远程连接，但是看起来很麻烦。我用的xshell登录。
这里要说下，不同系统的登录账户是不一样的，大部分是ec2-user，centos的登录账户名字就是centos。
他会提示下载密钥文件，保管好密钥文件，用xshell登陆主机时可以ec2上创建root账户，切换登陆


接上边，登录进系统后，开始开始操作
---
```markdown

1、创建root的密码，输入如下命令：

	sudo passwd root

2、切换到root账号，命令如下：

	su root

3、编辑亚马逊aws-ec2主机的ssh登录方式，查找PasswordAuthentication no，把no改为yes，命令如下：（如果你要说不会用vi编辑，我也没办法了！）

	vi /etc/ssh/sshd_config

4、重启sshd服务，看命令

	sudo /sbin/service sshd restart

5、切换root账号

	su root

6、给账户centos添加密码（其它系统的是ec2-user）

	passwd centos

```

搭建shadowsocks很简单，本次使用的是libev版本，Centos7环境，32位和64位都可以.获得root权限后，root登陆服务器。
```markdown
一行一行按以下代码运行

	wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-libev.sh

	chmod +x shadowsocks-libev.sh
		
//注意此代码运行时会提示需不需要更改端口号，加密方式和密码。有需要可以进行更改

	./shadowsocks-libev.sh 2>&1 | tee shadowsocks-libev.log
		

卸载方法：

	./shadowsocks-libev.sh uninstall

启动：

	/etc/init.d/shadowsocks start

停止：

	/etc/init.d/shadowsocks stop 

重启：

	/etc/init.d/shadowsocks restart 

查看状态：

	/etc/init.d/shadowsocks status
	
修改配置文件

	vi /etc/shadowsocks-libev/config.json

```

至此结束
---
有什么问题大家可以email，我知道的会解答的。
此教程由Fancy整理原文作者Aren。

