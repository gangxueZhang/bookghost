# Centos命令行发送邮件
    msmtp是一个开源的SMTP客户端，它负责传输邮件到SMTP服务器。
    mutt是一款功能强大的基于文字界面的E-Mail Client程序，可以用它来读写、回复、保存邮件，当然也可以在邮件中添加附件，它需要和msmtp配合使用。
## 安装配置msmtp

* **安装**

```shell
$ yum install msmtp -y 
```

* **配置**

  * 创建配置文件

  ```shell
  # 系统配置文件
  $ mkdir /usr/local/msmtp/etc
  $ touch /usr/local/msmtp/etc/msmtprc
  # 用户配置文件
  $ touch ~/.msmtprc
  # 设置配置文件权限
  $ chmod 600 ~/.msmtprc
  $ chmod 600 /usr/local/msmtp/etc/msmtprc
  ```

  * 修改配置文件（二选一）

  ```shell
  # 设置默认值
  defaults
  # 指定日志文件
  logfile /usr/local/msmtp/msmtp.log
  # SMTP服务账号
  account yourmail
  account default : yourmail@yeah.net
  # SMTP邮件服务器地址
  host smtp.yeah.net
  # 指定认证方式
  auth login
  # 关闭tls认证
  tls off
  # 邮件服务器登录账号
  user yourmail
  # 发送的邮件Email
  from yourmail@yeah.net
  # 邮件服务器登陆密码(SMTP的授权码)
  password password
  ```

## 安装配置mutt

* 安装

```shell
$ yum install mutt -y 
```

* 配置

  * 创建配置文件

  ```shell
  # 系统配置文件
  $ touch /etc/Muttrc
  # 用户配置文件
  $ touch ~/.muttrc
  # 设置配置文件权限
  $ chmod 600 ~/.muttrc
  $ chmod 600 /etc/Muttrc
  ```

  * 修改配置文件

  ```shell
  # 设置msmtp命令路径和配置
  set sendmail="/usr/bin/msmtp -C /usr/local/msmtp/etc/msmtprc"
  set use_from=yes
  #发信人
  set realname="yourmail" 
  #发信邮箱
  set from=yourmail@yeah.net 
  set editor="vim"
  ```

## 邮件发送命令

```shell
# Mutt版本
Mutt 1.5.21 (2010-09-15)
# Mutt语法
mutt [<options>] [-z] [-f <file> | -yZ]
mutt [<options>] [-x] [-Hi <file>] [-s <subj>] [-bc <addr>] [-a <file> [...] --] <addr> [...]
mutt [<options>] [-x] [-s <subj>] [-bc <addr>] [-a <file> [...] --] <addr> [...] < message
mutt [<options>] -p
mutt [<options>] -A <alias> [...]
mutt [<options>] -Q <query> [...]
mutt [<options>] -D
mutt -v[v]

# 使用案例
# 主题是"测试",邮件内容是log.log的信息
$ mutt -s "测试"  yourmail@163.com  < log.log
# 主题是"测试",邮件内容是log.log的信息去掉空行的内容
$ cat log.log | grep -v -E "^$" | mutt -s "测试" yourmail@163.com

# 选项说明
  -A <alias>            扩展给定的别名
  -a <file> [...] --    将文件附加到消息文件列表必须以“--”序列终止
  -b <address>          指定密件抄送 (BCC) 地址
  -c <address>          指定抄送 (CC) 地址
  -D                    将所有变量的值打印到标准输出
  -e <command>          指定初始化后要执行的命令
  -f <file>             指定要读取的邮箱
  -F <file>             指定一个备用的 muttrc 文件
  -H <file>             指定要从中读取标题和正文的草稿文件
  -i <file>             指定 Mutt 应该包含在正文中的文件
  -m <type>             指定默认邮箱类型
  -n                    导致 Mutt 不读取系统 Muttrc
  -p                    召回推迟的消息
  -Q <variable>         查询一个配置变量
  -R                    以只读模式打开邮箱
  -s <subj>             指定主题（如果有空格必须用引号引起来）
  -v                    显示版本和编译时定义
  -x                    模拟mailx发送模式
  -y                    选择在您的“邮箱”列表中指定的邮箱
  -z                    如果邮箱中没有消息则立即退出
  -Z                    用新消息打开第一个文件夹，如果没有则立即退出
  -h                    mutt帮助信息
```





