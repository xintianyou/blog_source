---
title: 从零搭建vue+jenkins超详细教程
date: 2020-10-16 12:44:40
tags: 
    - 前端架构
    - JS
    - VUE
categories: 
    - web前端
---

> 相信vue很多人都已经很熟悉了，利用脚手架很容易搭建一个vue项目
> 但项目多了以后每次部署测试环境就相当麻烦，还容易出错
> 所以趁这两天不忙，研究一下jenkins，也总算是入门了
![在这里插入图片描述](https://img-blog.csdnimg.cn/img_convert/65d24c7bb1d58c01d6e16fc4f8ff54cc.png#pic_center)

jenkins官网[传送门](https://www.jenkins.io/zh/doc/)

初步了解了jenkins是干什么的以后，直接开干
步骤：

## 0.服务器安装java，并配置环境变量

 1. 下载

打开[oracle官网](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201015165008364.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpblRpYW5vdTEyMw==,size_16,color_FFFFFF,t_70#pic_center)
我一般习惯新建一个文件夹专门用于存放安装包文件（以个人喜好为准，可忽略）

```bash
cd /
// 创建并切换至安装包文件夹
mkdir java-package && cd java-package
```

```bash
// 下载源文件
wget https://download.oracle.com/otn/java/jdk/8u261-b12/a4634525489241b9a9e1aa73d9e118e6/jdk-8u261-linux-x64.tar.gz?AuthParam=1602751770_7c097e4bf112ac61ba04b7a40aa7a988
```
> 由于该下载链接会失效，请自行去官网下载获取下载链接
> 提示：wgwt下载jdk有坑（我是下载次数多了，后面直接无法不让我下载了），建议下载到本地再上传服务器

 1. 安装
```bash
// 创建安装目录
mkdir /usr/local/java 
// 解压至安装目录
tar -zxvf jdk-8u261-linux-x64.tar.gz -C /usr/local
cd /usr/local
// 重命名
mv jdk-8u261-linux-x64 java 
```
 3. 设置环境变量

- 打开文件
```bash
vim /etc/profile
```
- 添加你的路径（别忘了保存）
```bash
# set for java
export JAVA_HOME=/usr/local/java
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH
```

- 执行命令使环境变量生效

```bash
source /etc/profile
```

- 添加软链接

```bash
ln -s /usr/local/java/bin/java /usr/bin/java
```
- 检查

```bash
java -version
```
我这里使已经安装过的
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201015171728555.png#pic_center)

## 1.服务器安装jenkins

 1. 安装
 
	[各版本下载地址](https://pkg.jenkins.io/)
```bash
cd /
mkdir jenkins-package && cd jenkins-package
wget https://pkg.jenkins.io/redhat/jenkins-2.156-1.1.noarch.rpm
rpm -ivh jenkins-2.156-1.1.noarch.rpm
```
![下载中...](https://img-blog.csdnimg.cn/20201015174820606.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpblRpYW5vdTEyMw==,size_16,color_FFFFFF,t_70#pic_center)

![经过漫长等待，终于下好了](https://img-blog.csdnimg.cn/20201015174751498.png#pic_center)

 2. 配置
修改监听端口
 

```bash
vim /etc/sysconfig/jenkins
# 监听端口
JENKINS_PORT="8080"
```

 3. 权限
 
使用root权限，避免后面出现权限不足问题
```bash
JENKINS_USER="root"
```

## 2.启动jenkins服务
```bash
systemctl start jenkins
```
> 我第一次搭建的时候没有java环境，一直报错还不明所以
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201015181222848.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpblRpYW5vdTEyMw==,size_16,color_FFFFFF,t_70#pic_center)

> 如果第一步java环境变量没配置好，此处会报错：
```bash
Starting Jenkins bash: /usr/bin/java: No such file or directory
```
浏览器输入http:<ip或者域名>:8080访问jenkins
如果无法访问，请检查防火墙、安全组是否放开

 1. 检查jenkins运行状态

```bash
// 查看jenkins运行状态
systemctl status jenkins
```
可以看到jenkins是正常运行的
![jenkins是正常运行的](https://img-blog.csdnimg.cn/20201016110948876.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpblRpYW5vdTEyMw==,size_16,color_FFFFFF,t_70#pic_center)

 2. 检查防火墙
```bash
systemctl status firewalld
```
防火墙是开着的
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201016111234616.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpblRpYW5vdTEyMw==,size_16,color_FFFFFF,t_70#pic_center)

```bash
// 查看是否开放8080端口
firewall-cmd --list-ports
// 如果没有，配置8080端口
firewall-cmd --permanent --zone=public --add-port=8080/tcp
// 重启防火墙
systemctl reload firewalld
```
再访问我们的8080，终于看到了这个界面**解锁jenkins**，按提示在服务器上找到初始密码，继续下一步
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201016111845695.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpblRpYW5vdTEyMw==,size_16,color_FFFFFF,t_70#pic_center)

 3. 此处可能会出现一直卡在SetupWiazrd阶段，页面一直 loading 或者白屏，经过各种baidu，google，发现可能原因是jenkins需要更新安装一些组件，但是请求得不到相应。
解决办法：浏览器访问http:<ip或者域名>:8080/pluginManager/advanced
如[http://localhost:8080/pluginManager/advanced](http://localhost:8080/pluginManager/advanced)
![在这里插入图片描述](https://img-blog.csdnimg.cn/202010161142012.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpblRpYW5vdTEyMw==,size_16,color_FFFFFF,t_70#pic_center)
即```http://updates.jenkins.io/update-center.json```，点击submit。然后重启jenkins```service jenkins restart```，再来访问我们的8080端口，解决了

此处我选择推荐的插件进行安装
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201016114513421.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpblRpYW5vdTEyMw==,size_16,color_FFFFFF,t_70#pic_center)
有些可能会安装失败，先不用管，后面有需要再安装
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020101611465970.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpblRpYW5vdTEyMw==,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201016114913222.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpblRpYW5vdTEyMw==,size_16,color_FFFFFF,t_70#pic_center)
这里还有两个步骤没有截图，按提示走即可

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201016115017851.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpblRpYW5vdTEyMw==,size_16,color_FFFFFF,t_70#pic_center)

## 3.服务器安装nodejs
> 由于我们要部署vue项目，那肯定少不了node环境
 - 下载nodejs
 [nodejs下载地址](https://nodejs.org/zh-cn/download/)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201015173730515.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpblRpYW5vdTEyMw==,size_16,color_FFFFFF,t_70#pic_center)

```bash
cd /
mkdir node-package && cd node-package
// 下载最新版nodejs
wget https://nodejs.org/dist/v12.19.0/node-v12.19.0-linux-x64.tar.xz
```

```bash
// 解压至/usr/local/nodejs
tar -xvf node-v12.19.0-linux-x64.tar.xz -C /usr/local/nodejs
```

```bash
// 创建软连接
ln -s /usr/local/nodejs/bin/node /usr/local/bin/
ln -s /usr/local/nodejs/bin/npm /usr/local/bin/
```

```bash
// 检查
node -v
npm -v
```

## 4.jenkins安装git，node插件，并在系统设置里应用node

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020101611530528.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpblRpYW5vdTEyMw==,size_16,color_FFFFFF,t_70#pic_center)
可能会看到很多报错，没关系，这些都是之前安装失败的，我们可以升级为最新版本
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201016115402828.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpblRpYW5vdTEyMw==,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201016115656574.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpblRpYW5vdTEyMw==,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201016115707756.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpblRpYW5vdTEyMw==,size_16,color_FFFFFF,t_70#pic_center)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201016115737648.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpblRpYW5vdTEyMw==,size_16,color_FFFFFF,t_70#pic_center)
由于安装了新的版本，再次登录时如果刚才设置的密码不对，就去找到服务器中的初始密码，后面也可以在界面中修改密码
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201016120210826.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpblRpYW5vdTEyMw==,size_16,color_FFFFFF,t_70#pic_center)
点击系统管理-插件管理-可选插件-输入git，看你的源代码是用什么管理的，我这里勾选了GitLab，和Gitlab Authentication，直接安装
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201016120447345.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpblRpYW5vdTEyMw==,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201016120809760.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpblRpYW5vdTEyMw==,size_16,color_FFFFFF,t_70#pic_center)
全局node插件配置
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201016121028298.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpblRpYW5vdTEyMw==,size_16,color_FFFFFF,t_70#pic_center)

**关于汉化：直接搜索插件Localization: Chinese进行安装**
## 5.jenkins创建项目，配置项目git源，配置构建脚本

> 注意：你的服务器需要安装git，用于拉代码

点击新建，选择构建自由风格的项目
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201016121123383.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpblRpYW5vdTEyMw==,size_16,color_FFFFFF,t_70#pic_center)
源码管理-选择git
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201016121359477.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpblRpYW5vdTEyMw==,size_16,color_FFFFFF,t_70#pic_center)
这里需添加	Credentials，输入你的git用户名和密码
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201016121624102.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpblRpYW5vdTEyMw==,size_16,color_FFFFFF,t_70#pic_center)
然后选择一个Credentials，并选择要构建的分支
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201016121758444.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpblRpYW5vdTEyMw==,size_16,color_FFFFFF,t_70#pic_center)
构建环境
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201016122054790.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpblRpYW5vdTEyMw==,size_16,color_FFFFFF,t_70#pic_center)
添加构建命令
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201016122351678.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpblRpYW5vdTEyMw==,size_16,color_FFFFFF,t_70#pic_center)
最后别忘了保存

尝试一次构建
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201016122610686.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpblRpYW5vdTEyMw==,size_16,color_FFFFFF,t_70#pic_center)
你可以点进度条-控制台输出
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201016122851825.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpblRpYW5vdTEyMw==,size_16,color_FFFFFF,t_70#pic_center)
每个项目的第一次构建由于需要下载依赖，过程会比较慢，后面就快很多了
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201016123347376.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpblRpYW5vdTEyMw==,size_16,color_FFFFFF,t_70#pic_center)
可以看到success，打包成功，再去服务器查看打包文件
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201016123244442.png#pic_center)

注意：打包时可能会遇到报错```permission denied```权限问题
解决方案：

 1. 建立全局文件夹配置
	

```bash
mkdir ~/.npm-global
 
npm config set prefix '~/.npm-global'
```

 2. 修改环境变量

```bash
vi /etc/profile 
# nodejs 配置
export PATH=~/.npm-global/bin:$PATH
// 激活环境变量
source /etc/profile
```
再次构建
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201016123937124.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpblRpYW5vdTEyMw==,size_16,color_FFFFFF,t_70#pic_center)
可以看到第一次构建和第二次构建的速度差异

至此：第一次折腾jenkins到此告一段落，可见过程并不复杂