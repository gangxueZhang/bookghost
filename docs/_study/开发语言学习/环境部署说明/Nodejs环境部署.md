# Nodejs环境部署

## 安装npm命令

npm命令是node.js的npm 插件管理器，也就是下载插件安装插件的管理器。

```shell
$ yum install nodejs
```

* **npm命令**

```shell
# 查看npm版本
$ npm -v
# 升级npm命令
$ sudo npm install npm -g
# 使用淘宝镜像命令
$ npm install -g cnpm --registry=https://registry.npmmirror.com
$ cnpm install <Module Name>
# npm安装Node.js模块语法
$ npm install <Module Name>
# 卸载Node.js模块语法
$ npm uninstall <Module Name>
# 设置代理服务地址
$ npm config set proxy null
# 查看全局安装的模块
$ npm list -g
# 清空本地缓存
$ npm cache clear
```

## 安装多版本管理

node有一个模块叫n，是专门用来管理node.js的版本的。

```shell
# 安装n模块
$ npm install -g n
# 切换或者安装新版本node(有就切换，没有就安装)
$ n <Version>

```

## 附录A

### urlgrabber-ext-down

yum运行报错：File "/usr/libexec/urlgrabber-ext-down", line 28

![img](https://img2018.cnblogs.com/blog/1505947/201908/1505947-20190809171324928-711393837.png)

```shell
$ vim /usr/libexec/urlgrabber-ext-down
```

![img](https://img2018.cnblogs.com/blog/1505947/201908/1505947-20190809171334639-1098013813.png)





