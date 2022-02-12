---
  title: nuxt项目部署心得
  date: 2021-06-30 15:04:00
  tags: 
    - VUE
    - NUXT
  categories: 
    - web前端
---

昨晚部署一个nuxt项目的官网花了三个小时，实在是汗颜，赶紧写个博客记录一下，以免再犯

## 我的部署流程
##### 运行环境：linux服务器，pm2进程守护，nginx反向代理
##### 0.服务器安装nginx和pm2
- 安装nginx已有众多大神写过相当详细的教程
- 安装pm2
   确保服务器上装有node环境
   

	```cpp
	npm i pm2 -g
	```
	
##### 1.项目根目录新建process.json文件

```cpp
// process.json 简单版本
{
  "apps": {
    "name": "xx-test", // 项目名称
    "script": "npm run start", // 要执行的脚本
    "watch": [ // 添加受监听的文件，以便自动更新
      ".nuxt"
    ],
    "ignore_watch": [ // 不监听的文件
      "node_modules"
    ],
    "watch_options": { // 监听配置 http://pm2.keymetrics.io/docs/usage/watch-and-restart/
      "persistent": true,
      "ignoreInitial" : true
    },
    "env": {
        "NODE_ENV": "production",
    }
  }
}

// 如有多个环境
// 用数组作为 apps 的值
{
	"apps": [
		{
			"name": "xxx1",
			...
		},
		{
			"name": "xxx2",
			...
		}
	]
}
```

这一步是为了方便管理多个进程，也为了简化命令，如不需要，可直接使用

```cpp
pm2 start npm --name "project-name" -- run start
```
"project-name" 是package.json中的name
##### 2.提交代码，npm run build
这一步也有很多文章写道：
> 项目在本地完成后，npm run build 打包应用，打包完成后，我们将
> *.nuxt*
> *static*
> *nuxt.config.js*
> *package.json*
> 传到服务器空间里
> 然后在服务器上部署运行
> 1.运行npm install 安装package里的依赖
> 2.运行npm start 就可以运行起来nuxt的服务渲染了

但是我的项目使用了i18n做国际化，除了这四个文件/文件夹，还要上传i18n相关文件，尝试多次失败(不知道上传哪个文件，试过上传zh.json和en.json，发到线上报错)
所以我直接在本地提交所有代码（忽略node_modules）到git，然后在服务器*git clone*下来
```
git clone "xxxxxx"
npm i
npm run build
pm2 start process.json
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021063014342393.png)
看到这个说明pm2启动成功，我这里启动了两个进程

##### 3.我们的浏览器上如何访问
nginx配置

```cpp
upstream project_name {
   	server 127.0.0.1:3000; # nuxt项目默认监听3000端口
	keepalive 64;
}
server {
    listen 80;
	server_name xx.com;
	location / {
       		proxy_http_version 1.1;
    		proxy_set_header X-Nginx-Proxy true;
   			proxy_cache_bypass $http_upgrade;
   			proxy_pass http://project_name/; // 这里是上面的upstream名
    		proxy_redirect off;
	}
	# 文件缓存
	location ~ .*\.(png|css|mp4|mov)(.*) {
		#default_type 'image/png';
		root "/home/project/static/"; // 静态文件路径
		expires 30d;
		access_log off;
    }
	location ~ .*\.(mp4)(.*) {
		#default_type 'media/mp4';
		root "/home/project/static/mp4/"; // 静态文件路径
		expires 30d;
		access_log off;
    }
	location ~ .*\.(ioc)(.*) {
        root "/home/project/static/";
        expires 30d;
        access_log off;
    }
}
```

```
nginx -s reload
```
在浏览器上输入 xx.com 访问，到这里服务端渲染部署完成

## 等等，还没完
想想昨晚是为什么加班三个小时，由于几个月之前上线过一版，最近改版上线测试环境，我拉了一个test分支，一顿操作，访问502，检查下来是3000端口被占用，这怎么办呢？原来是因为我的代码里没有指定端口，默认是3000，于是修改测试分支nuxt.config.js
在head同级添加
```cpp
server: {
  port: 3001, // 默认3000 也可以改成其它的端口号
  // host: '0.0.0.0', // 默认localhost,使用nginx反向代理就不用指定
},
```
修改nginx.conf

```cpp
upstream project_name {
   	server 127.0.0.1:3001;
	keepalive 64;
}
```
```
nginx -s reload
```
一直以为默认的3000端口是pm2的行为，各种baidu google无果，蠢哭