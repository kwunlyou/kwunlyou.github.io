---
layout: post
title:  "利用Amazon EC2和Shadowscoks科学上网"
categories: life
tags: web 
excerpt: "回国过年上网诸多不便，原因你懂的，于是就开始研究科学上网。Amazon EC2对新用户有免费试用的优惠，于是就想着用Amazon EC2当服务器来帮助科学上网。" 
---

回国过年上网诸多不便，原因你懂的，于是就开始研究科学上网。[Amazon EC2]对新用户有免费试用的优惠，就打算用[Amazon EC2]当服务器来帮助科学上网。最先尝试用tinyproxy配置代理服务器不work，但是在英国的时候测试过timyproxy是可行的，深深感受到了功夫墙的威猛！据说，功夫墙现在都开始丧心病狂的用machine learning相关的技术进行甄别了，我擦！！！于是又尝试用PPTP设置VPN服务器，还是不行。最后辗转到了[Shadowsocks]，终于成功，而且移动设备也可以使用。步骤如下：

1. 启动一个Amazon EC2的instance，关于这个网上的教程很多，一些参数如下
	*  Image用的Amazon Linux （注意：如果选择ubuntu的话，第2步的使用的脚本shadowsocks.sh需要做相应修改）
	*  Instance的类型是t2.micro (免费)
	*  Security group设置中将TCP的端口全部开放（实际上仅开放8989即可）
2. SSH到配置好的服务器上，sudo依次运行 (具体参看这个[Post])
	*  
	```bash 
	wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks.sh
	```
	*  
	```bash 
	chmod +x shadowsocks.sh
	```
	*  
	```bash 
	./shadowsocks.sh 2>&1 | tee shadowsocks.log
	```
3. 运行
   ```bash
   /etc/init.d/shadowsocks start
   ```	
   启动服务
4. 给本地机器或者移动端下载相应客户端，简单配置下，具体也参看这个[Post]   

完成！开始无限制上网。BTW，我去年买了个表，功夫墙！

[Amazon EC2]: http://aws.amazon.com/ec2/
[Shadowsocks]:	https://www.shadowsocks.com/
[Post]: http://teddysun.com/342.html