#How to Creation SSH
##SSH
SSH是`Source Shell Protocol`的缩写，SSh是为建立在`应用层`和`传输层`基础上的安全协议。SSH是目前较为可靠，专为远程登录会话和其他网络服务提供安全性的协议。利用SSH可以有效防止远程管理过程中的信息泄漏问题。

SSH最初是UNIX系统上的一个程序，后来逐渐扩展到其他操作平台。SSH在正确使用时可以弥补网络中的漏洞，SSH客户端适用于多种平台，几乎所有的UNIX平台，包括HP-UX、Linux、AIX、Solaris等等

传统的网络服务程序，比如ftp、pop和telnet在本质上都是不安全的，因为他们在网络上是用明文进行口令的传输，有一些别有用心的人就可以非常容易的截取这些口令和数据。

通过SSH，可以把所有传输的数据进行加密，而且SSH还有一个额外的好处就是传输的数据是经过加密处理的，所以可以加快传输的速度。SSH还有其他的很多功能，他既可以替代Telnet，又可以为FTP、PoP、甚至是PPP提供一个安全的通道。

## SSH 功能

SSH可以提供两种功能：

1. SSH服务，提供远程登录
2. 类似于FTP的sftp-server，用于基于ssh的文件传输


## SSH工作机制：

他们的工作机制大致是本地客户端发送一个连接请求到远程服务器，服务端检查申请的包和ip地址再发送密钥给ssh的客户端，本地再将密钥发送给服务端，自此建立连接。


## 连接方式

###version 1
ssh主机（Server端）用RSA加密方式产生一个1024bit的RSA-key，用来产生公约与私钥的演算方法：

1. 当ssh daemon（sshd）启动时，就会产生一个768-bit的公钥（或称为Server Key）存放在Server中
2. 当client端的联机请求发送后，Server将公钥发送给client，此时client会对比判断公钥的正确性，判断的依据/etc/ssh/ssh_knownhosts、$HOME/.ssh/known_hosts
3. client端接收768bit的Server Key之后，client自己也随机产生一个256bit的私钥（private key，或host key），并且以加密的方式传送给Server
4. Server与client在此联机过程中，就以一对1024bit的key pair来进行数据的传递


**补充说明：**

Public Key是放在Server上，而Client的软件需要能接收Public Key，计算出Private key，并把二者的组合成一把独一无二的key pair（因为Client每次的256bit的Private key是随机产生的），在Client端的用户默认目录下$HOME/.ssh/known_keys会记录曾经联机过的主机的Public Key，用以确认每次来自该主机的联机是否正确
###version 2
在version 1的联机过程中，当Server接收Client的Private key之后，就不会针对该联机的key pair进行校验，如过黑客传送了恶意代码，由于主机端部会检验联机的正确性，所以可能会接收该程序代码。

在version 2中增加了一个确认联机正确性的diff-hellman机制在每次数据传输中Server都会以这种机制校验资料来源是否正确


## OperSSL软件：
OpenSSL是一种比较常见的SSH服务端和客户端软件。提供了sshd、shell、sftp功能，其默认端口是22，在Redhat、CentOS等Linux操作系统中的默认SSH Daemon就是这个了。

### 下面对这个软件的几种用法加以说明：
**A. Server keys记录：$HOME/.ssh/known_hosts**

当Client端接收来自Server端的Public key之后，会主动的对比这个key的正确性，有下面的两种情况：

1. 没有记录，那么会出现提示，是否接收来自Server的Public Key
2. 对比成功，直接提示输入密码
3. 如果出现了这样的登录提示：** warning: Remote host identification has changed **那么说明上次登录的远程ssh主机的key已经被更改了，原因可能是Server的重启、重装或者更新了相关的软件，这时候的处理方式是将$HOME/.ssh/known_hosts文件中与主机相关的字段删除，重新接收来自Server的Public key

**B. 设置不用输入用户名和密码就能立即登录的目标主机**

原理：将Client端产生的key复制到Server中

在Client向Server登录时，由于两者在ssh要联机的信号传递中，就已经对比过key了，因此可以立即进入数据传输，而不需要输入密码。

1. 在Client端建立Public key和Private key两个密钥，命令为ssh-keygen
2. 将Private-key放在client的默认目录下，$HOME/.ssh，并为该用户可读的状态
3. 把Public key放在任何一个你希望要用来登录主机的Server的某user的默认目录中的认证文件中

##生成 Git SSH-key
当你第一次从git资料库中clone项目时，可能会需要配置SSH-key，因为项目本地端和远程端的项目管理是基于SSH的，下面让我们一起看看SSH-key的配置过程。

###设置Git的username 和 email
在你生成SSH前，需要在本地进行用户名和email的验证

**代码如下：**

```
$ git config --global user.name "youruser.name"
$ git config --global user.email "youremailusername"

```

###生成SSH密钥的过程：

####首先查看你的本地是否已经有ssh密钥

```
$ cd ~/.ssh

```
`如果没有密钥就不会有此文件，如有有悔备份删除`

####生成密钥

```
$ ssh-keygen -t rsa -C "youremailusername"

```
连续按3个回车键，密码为空。

会得到以下消息:

```
Your identification has been saved in /home/tekkub/.ssh/id_rsa.
Your public key has been saved in /home/tekkub/.ssh/id_rsa.pub.
The key fingerprint is:

```
最后会得到两个文件
1. id_ rsa
2. id_rsa.pub

####查询SSH key

你的SSH key存在于生成的`id_rsa.pub`的文件中，

```
$ cat ~/.ssh/id_rsa.pub

```
当执行上条命令的时候，如果已经生成key，则会出现类似
** ssh-rsa  AAAAB3NzaC1yc2EAAAADAQABAAABAQC2Z/MOZdADABODQjvGDK+5j7IBefKFQPOen0aq1GpIzPFHclL62ddhrbjQUZ9IeDhxYwDU7Aq....wisher@wisher.local **
的字符串，这个就是本地生成的SSH key。

