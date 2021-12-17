# Linux命令集

## curl命令

```shell
# 语法
$ curl [options...] <url>

# 使用案例
## 使用cookie
### 保存http的response里面的cookie信息
$ curl -b cookiec.txt http://www.linux.com
## 模仿浏览器
$ curl -A "Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 5.0)" http://www.linux.com
## 伪造referer(盗链)
$ curl -e "www.linux.com" http://mail.linux.com
## 指定proxy服务器以及其端口
$ curl -x 192.168.100.100:1080 http://www.linux.com
## Post带参数请求
$ curl -H "Content-Type:application/json" -H "Data_Type:msg" -X POST --data '{"name": "离歌", "age": 20}' http://127.0.0.1:5000/service


# 参数选项 (H)表示仅HTTP/HTTPS，(F)表示仅FTP
     --anyauth    选择“任何”身份验证方法(H)
 -a, --append    上传时附加到目标文件(F/SFTP)
     --basic    使用HTTP基本身份验证(H)
     --cacert FILE    CA证书以验证对等方（SSL）
     --capath DIR    CA目录以验证对等(SSL)
 -E, --cert CERT[:PASSWD]    客户端证书文件和密码(SSL)
     --cert-type TYPE    证书文件类型(DER/PEM/ENG)(SSL)
     --ciphers    列出要使用的SSL密码(SSL)
     --compressed    请求压缩响应(使用deflate或gzip)
 -K, --config FILE    指定要读取的配置文件
     --connect-timeout SECONDS    允许连接的最长时间
 -C, --continue-at OFFSET    恢复传输偏移
 -b, --cookie STRING/FILE    从(H)读取cookie的字符串或文件
 -c, --cookie-jar FILE    操作后将cookies写入该文件(H)
     --create-dirs    创建必要的本地目录层次结构
     --crlf    在上传时将LF转换为CRLF
     --crlfile FILE    从给定文件中获取PEM格式的CRL列表
 -d, --data DATA     HTTP POST数据(H)
     --data-ascii DATA    HTTP POST ASCII数据(H)
     --data-binary DATA    HTTP POST二进制数据(H)
     --data-urlencode DATA    HTTP POST数据url编码(H)
     --delegation STRING GSS-API    委托权限
     --digest     使用HTTP摘要认证(H)
     --disable-eprt    禁止使用EPRT或LPRT(F)
     --disable-epsv    禁止使用EPSV(F)
 -D, --dump-header FILE    将头文件写入此文件
     --egd-file FILE    随机数据的EGD套接字路径(SSL)
     --engine ENGINGE    加密引擎(SSL)。 “--engine list”用于列表
 -f, --fail     在HTTP错误(H)上静默失败(根本没有输出)
 -F, --form CONTENT     指定HTTP多部分POST数据(H)
     --form-string STRING    指定HTTP多部分POST数据(H)
     --ftp-account DATA    账户数据字符串(F)
     --ftp-alternative-to-user COMMAND    字符串替换“USER[name]”(F)
     --ftp-create-dirs    如果不存在则创建远程目录(F)
     --ftp-method [MULTICWD/NOCWD/SINGLECWD]     控制CWD使用(F)
     --ftp-pasv     使用PASV/EPSV而不是PORT(F)
 -P, --ftp-port ADR     使用具有给定地址的PORT而不是PASV(F)
     --ftp-skip-pasv-ip     跳过PASV的IP地址(F)
     --ftp-pret     在PASV之前发送PRET(for drftpd)(F)
     --ftp-ssl-ccc     认证后发送CCC(F)
     --ftp-ssl-ccc-mode ACTIVE/PASSIVE     设置CCC模式(F)
     --ftp-ssl-control     需要SSL/TLS进行ftp登录，清除传输(F)
 -G, --get     使用HTTP GET(H)发送-d数据
 -g, --globoff     使用{}和[]禁用URL序列和范围
 -H, --header LINE     传递给服务器的自定义标头(H)
 -I, --head     仅显示文档信息
 -h, --help     这个帮助文本
     --hostpubmd5 MD5     主机公钥的十六进制编码MD5字符串(SSH)
 -0, --http1.0    使用HTTP 1.0(H)
     --ignore-content-length    忽略HTTP Content-Length头
 -i, --include    在输出中包含协议头(H/F)
 -k, --insecure    允许连接到没有证书的SSL站点(H)
     --interface INTERFACE    指定要使用的网络接口/地址
 -4, --ipv4    将名称解析为IPv4地址
 -6, --ipv6    将名称解析为IPv6地址
 -j, --junk-session-cookies    忽略从文件中读取的会话cookie(H)
     --keepalive-time SECONDS    keepalive探测之间的间隔
     --key KEY    私钥文件名(SSL/SSH)
     --key-type TYPE    私钥文件类型(DER/PEM/ENG)(SSL)
     --krb LEVEL    启用具有指定安全级别的Kerberos(F)
     --libcurl FILE    转储此命令行的libcurl等效代码
     --limit-rate RATE    将传输速度限制为此速率
 -l, --list-only    仅列出FTP目录的名称(F)
     --local-port RANGE    强制使用这些本地端口号
 -L, --location    跟随重定向(H)
     --location-trusted    像--location并将身份验证发送到其他主机(H)
 -M, --manual    显示完整的手册
     --mail-from FROM    来自这个地址的邮件
     --mail-rcpt TO    邮件给这个接收者
     --mail-auth AUTH    原始邮件的发件人地址
     --max-filesize BYTES    要下载的最大文件大小(H/F)
     --max-redirs NUM    允许的最大重定向数(H)
 -m, --max-time SECONDS    允许传输的最长时间
     --metalink    将给定的URL处理为 metalink XML 文件
     --negotiate    使用HTTP协商身份验证(H)
 -n, --netrc    必须读取.netrc以获取用户名和密码
     --netrc-optional    使用.netrc或URL；覆盖-n
     --netrc-file FILE    设置要使用的netrc文件名
 -N, --no-buffer    禁用输出流的缓冲
     --no-keepalive    在连接上禁用keepalive
     --no-sessionid    禁用SSL会话ID重用(SSL)
     --noproxy    不使用代理的主机列表
     --ntlm    使用HTTP NTLM身份验证(H)
 -o, --output FILE    将输出写入<file>而不是stdout
     --pass PASS    私钥的密码短语(SSL/SSH)
     --post301    跟随301重定向后不要切换到 GET(H)
     --post302    跟随302重定向后不要切换到 GET(H)
     --post303    跟随303重定向后不要切换到 GET(H)
 -#, --progress-bar    将传输进度显示为进度条
     --proto PROTOCOLS    启用/禁用指定的协议
     --proto-redir PROTOCOLS    在重定向时启用/禁用指定的协议
 -x, --proxy    [PROTOCOL://]HOST[:PORT]在给定端口上使用代理
     --proxy-anyauth    选择“任何”代理身份验证方法(H)
     --proxy-basic    在代理上使用基本身份验证(H)
     --proxy-digest    在代理上使用摘要式身份验证(H)
     --proxy-negotiate    在代理上使用协商身份验证(H)
     --proxy-ntlm    在代理上使用 NTLM 身份验证(H)
 -U, --proxy-user USER[:PASSWORD]    代理用户和密码
     --proxy1.0    HOST[:PORT]在给定端口上使用 HTTP/1.0 代理
 -p, --proxytunnel    通过HTTP 代理隧道操作(使用 CONNECT)
     --pubkey KEY    公钥文件名(SSH)
 -Q, --quote CMD    传输前向服务器发送命令(F/SFTP)
     --random-file FILE    用于从(SSL)读取随机数据的文件
 -r, --range RANGE    只检索范围内的字节
     --raw    做HTTP"raw"，没有任何传输解码(H)
 -e, --referer    引用 URL(H)
 -J, --remote-header-name    使用标头提供的文件名(H)
 -O, --remote-name    将输出写入名为远程文件的文件
     --remote-name-all    为所有URL使用远程文件名
 -R, --remote-time    在本地输出上设置远程文件的时间
 -X, --request COMMAND    指定要使用的请求命令
     --resolve HOST:PORT:ADDRESS    强制将HOST:PORT解析为ADDRESS
     --retry NUM    如果出现暂时性问题，则重试请求NUM次
     --retry-delay SECONDS    重试时，在每次重试之间等待几秒
     --retry-max-time SECONDS    仅在此期间重试
 -S, --show-error    显示错误。使用-s，使curl在发生错误时显示错误
 -s, --silent    静默模式。不要输出任何东西
     --socks4 HOST[:PORT]    给定主机+端口上的SOCKS4代理
     --socks4a HOST[:PORT]    给定主机+端口上的SOCKS4a代理
     --socks5 HOST[:PORT]    给定主机+端口上的SOCKS5代理
     --socks5-basic    为SOCKS5代理启用用户名/密码身份验证
     --socks5-gssapi    为SOCKS5代理启用GSS-API身份验证
     --socks5-hostname HOST[:PORT]    SOCKS5代理，将主机名传递给代理
     --socks5-gssapi-service NAME    gssapi的SOCKS5代理服务名称
     --socks5-gssapi-nec    与NEC SOCKS5服务器的兼容性
 -Y, --speed-limit RATE    在“速度时间”秒内停止低于速度限制的传输
 -y, --speed-time SECONDS    触发限速中止的时间。默认为 30
     --ssl    尝试 SSL/TLS（FTP、IMAP、POP3、SMTP）
     --ssl-reqd    需要 SSL/TLS（FTP、IMAP、POP3、SMTP）
 -2, --sslv2    使用SSLv2(SSL)
 -3, --sslv3    使用SSLv3(SSL)
     --ssl-allow-beast    允许安全漏洞改进互操作(SSL)
     --stderr FILE    重定向stderr的位置。-表示标准输出
     --tcp-nodelay    使用TCP_NODELAY选项
 -t, --telnet-option OPT=VAL    设置telnet选项
     --tftp-blksize VALUE    设置TFTP BLKSIZE选项(必须>512)
 -z, --time-cond TIME    基于时间条件的传输
 -1, --tlsv1    使用=>TLSv1(SSL)
     --tlsv1.0    使用TLSv1.0(SSL)
     --tlsv1.1    使用TLSv1.1(SSL)
     --tlsv1.2    使用TLSv1.2(SSL)
     --trace FILE    将调试跟踪写入给定文件
     --trace-ascii FILE    类似--trace但没有十六进制输出
     --trace-time    将时间戳添加到跟踪/详细输出
     --tr-encoding    请求压缩传输编码(H)
 -T, --upload-file FILE    将文件传输到目的地
     --url URL    要使用的URL
 -B, --use-ascii    使用ASCII/文本传输
 -u, --user USER[:PASSWORD]    服务器用户和密码
     --tlsuser    用户TLS用户名
     --tlspassword STRING    TLS密码
     --tlsauthtype STRING    TLS身份验证类型(默认SRP)
     --unix-socket FILE    通过这个UNIX域套接字连接
 -A, --user-agent STRING    发送到服务器的用户代理(H)
 -v, --verbose    让操作更健谈
 -V, --version    显示版本号并退出
 -w, --write-out FORMAT    完成后输出什么
     --xattr    将元数据存储在扩展文件属性中
 -q    如果用作第一个参数禁用.curlrc 
```



## df命令

`df`命令作用是列出文件系统的整体磁盘空间使用情况。可以用来查看磁盘已被使用多少空间和还剩余多少空间。
`df`命令显示系统中包含每个文件名参数的磁盘使用情况，如果没有文件名参数，则显示所有当前已挂载文件系统的磁盘空间使用情况，

```shell
# 语法
$ df [选项] [文件名]

# 使用案例
# 指定一个文件夹，查看该文件夹所在磁盘的使用情况
$ df /home
#指定一个文件
$ df /bin/ls   

# 参数选项
-a：--all，显示所有的文件系统，包括虚拟文件系统，参考示例2。
-B：--block-size，指定单位大小。比如1k，1m等，参考示例3。
-h：--human-readable，以人们易读的GB、MB、KB等格式显示，参考示例4。
-H：--si，和-h参数一样，但是不是以1024，而是1000，即1k=1000，而不是1k=1024。
-i：--inodes，不用硬盘容量，而是以inode的数量来显示，参考示例5。
-k：以KB的容量显示各文件系统，相当于--block-size=1k。
-m：以KB的容量显示各文件系统，相当于--block-size=1m。
-l：--local，只显示本地文件系统。
--no-sync：在统计使用信息之前不调用sync命令(默认)。
-sync：在统计使用信息之前调用sync命令。
-P：--portability，使用POSIX格式显示，参考示例6。
-t：--type=TYPE，只显示指定类型的文件系统，参考示例7。
-T：--print-type，显示文件系统类型，参考示例8。
-x：--exclude-type=TYPE，不显示指定类型的文件系统。
--help：显示帮助信息。
--version：显示版本信息。
```

## du命令

Linux du （英文全拼：disk usage）命令用于显示目录或文件的大小。du 会显示指定的目录或文件所占用的磁盘空间。

```shell
# 语法
$ du [-abcDhHklmsSx][-L <符号连接>][-X <文件>][--block-size][--exclude=<目录或文件>][--max-depth=<目录层数>][--help][--version][目录或文件]

# 使用案例
# 方便阅读的格式显示test目录所占空间情况：
$ du -h test

# 参数选项
-a或-all 显示目录中个别文件的大小。
-b或-bytes 显示目录或文件大小时，以byte为单位。
-c或--total 除了显示个别目录或文件的大小外，同时也显示所有目录或文件的总和。
-D或--dereference-args 显示指定符号连接的源文件大小。
-h或--human-readable 以K，M，G为单位，提高信息的可读性。
-H或--si 与-h参数相同，但是K，M，G是以1000为换算单位。
-k或--kilobytes 以1024 bytes为单位。
-l或--count-links 重复计算硬件连接的文件。
-L<符号连接>或--dereference<符号连接> 显示选项中所指定符号连接的源文件大小。
-m或--megabytes 以1MB为单位。
-s或--summarize 仅显示总计。
-S或--separate-dirs 显示个别目录的大小时，并不含其子目录的大小。
-x或--one-file-xystem 以一开始处理时的文件系统为准，若遇上其它不同的文件系统目录则略过。
-X<文件>或--exclude-from=<文件> 在<文件>指定目录或文件。
--exclude=<目录或文件> 略过指定的目录或文件。
--max-depth=<目录层数> 超过指定层数的目录后，予以忽略。
--help 显示帮助。
--version 显示版本信息。
```

## egrep命令

        egrep执行效果与"grep-E"相似，使用的语法及参数可参照grep指令，与grep的不同点在于解读字符串的方法。egrep是用extended regular expression语法来解读的，而grep则用basic regular expression语法解读，extended regular expression比basic regular expression的表达更规范。

```shell
# 语法
$ egrep [范本模式] [文件或目录] 

# 使用案例
显示文件中符合条件的字符。查找当前目录下所有文件中包含字符串"Linux"的文件
$ egrep Linux *

# 参数选项
参考grep参数
```

## fgrep命令

本指令相当于执行 grep 指令加上参数 **-F**，详见[grep命令](https://www.runoob.com/linux/linux-comm-grep.html)说明。Linux fgrep命令用于查找文件里符合条件的字符串。

```shell
# 语法
$ fgrep [范本样式][文件或目录...]

# 使用案例

# 参数选项
```

## find命令

        Linux find 命令用来在指定目录下查找文件。任何位于参数之前的字符串都将被视为欲查找的目录名。如果使用该命令时，不设置任何参数，则 find 命令将在当前目录下查找子目录与文件。并且将查找到的子目录和文件全部进行显示。

```shell
# 语法
find [-H | -L | -P] [-EXdsx] [-f path] path ... [expression]
find [-H | -L | -P] [-EXdsx] -f path [path ...] [expression]

# 使用案例
# *表示通配任意个字符 ？表示通配单个字符
find /home  -name "*.t?t"   
# 查找文件更新日时在距现在时刻4天以内的文件
find   /usr   -mtime  -4   
# 查找文件更新日时在距现在时刻5天以上的文件
find   /usr  -mtime  +4    
# 查找文件更新日时在距现在时刻4天以上5天以内的文件
find   /usr   -mtime   4   
# 查找一下/var/log目录下内容更改时间为40天前并且大于100K的文件，并列出他们的路径。
find /var/log   -mtime +40 -a -size +100k -exec ls -lh {} \;
# 查找一下/var/log目录下内容更改时间为30天前并且不属于root群组的文件，并提示是否删除他们。
find /var/log -type f -a  -mtime  +30 -not -group root -ok rm -rf {} \;
# 用grep命令在当前目录下的普通文件中搜索hostname这个词
find ./ -name -type f -print | xargs grep "hostnames"
# 删除满足条件的文件夹
find . -type d -name "[a-z]*" | xargs rm -rf


# 参数选项
-amin<分钟>：查找在指定时间曾被存取过的文件或目录，单位以分钟计算；
-anewer<参考文件或目录>：查找其存取时间较指定文件或目录的存取时间更接近现在的文件或目录；
-atime<24小时数>：查找在指定时间曾被存取过的文件或目录，单位以24小时计算；
-cmin<分钟>：查找在指定时间之时被更改过的文件或目录；
-cnewer<参考文件或目录>查找其更改时间较指定文件或目录的更改时间更接近现在的文件或目录；
-ctime<24小时数>：查找在指定时间之时被更改的文件或目录，单位以24小时计算；
-daystart：从本日开始计算时间；
-depth：从指定目录下最深层的子目录开始查找；
-expty：寻找文件大小为0 Byte的文件，或目录下没有任何子目录或文件的空目录；
-exec<执行指令>：假设find指令的回传值为True，就执行该指令；
-false：将find指令的回传值皆设为False；
-fls<列表文件>：此参数的效果和指定“-ls”参数类似，但会把结果保存为指定的列表文件；
-follow：排除符号连接；
-fprint<列表文件>：此参数的效果和指定“-print”参数类似，但会把结果保存成指定的列表文件；
-fprint0<列表文件>：此参数的效果和指定“-print0”参数类似，但会把结果保存成指定的列表文件；
-fprintf<列表文件><输出格式>：此参数的效果和指定“-printf”参数类似，但会把结果保存成指定的列表文件；
-fstype<文件系统类型>：只寻找该文件系统类型下的文件或目录；
-gid<群组识别码>：查找符合指定之群组识别码的文件或目录；
-group<群组名称>：查找符合指定之群组名称的文件或目录；
-help或——help：在线帮助；
-ilname<范本样式>：此参数的效果和指定“-lname”参数类似，但忽略字符大小写的差别；
-iname<范本样式>：此参数的效果和指定“-name”参数类似，但忽略字符大小写的差别；
-inum<inode编号>：查找符合指定的inode编号的文件或目录；
-ipath<范本样式>：此参数的效果和指定“-path”参数类似，但忽略字符大小写的差别；
-iregex<范本样式>：此参数的效果和指定“-regexe”参数类似，但忽略字符大小写的差别；
-links<连接数目>：查找符合指定的硬连接数目的文件或目录；
-iname<范本样式>：指定字符串作为寻找符号连接的范本样式；
-ls：假设find指令的回传值为Ture，就将文件或目录名称列出到标准输出；
-maxdepth<目录层级>：设置最大目录层级；
-mindepth<目录层级>：设置最小目录层级；
-mmin<分钟>：查找在指定时间曾被更改过的文件或目录，单位以分钟计算；
-mount：此参数的效果和指定“-xdev”相同；
-mtime<24小时数>：查找在指定时间曾被更改过的文件或目录，单位以24小时计算；
-name<范本样式>：指定字符串作为寻找文件或目录的范本样式；
-newer<参考文件或目录>：查找其更改时间较指定文件或目录的更改时间更接近现在的文件或目录；
-nogroup：找出不属于本地主机群组识别码的文件或目录；
-noleaf：不去考虑目录至少需拥有两个硬连接存在；
-nouser：找出不属于本地主机用户识别码的文件或目录；
-ok<执行指令>：此参数的效果和指定“-exec”类似，但在执行指令之前会先询问用户，若回答“y”或“Y”，则放弃执行命令；
-path<范本样式>：指定字符串作为寻找目录的范本样式；
-perm<权限数值>：查找符合指定的权限数值的文件或目录；
-print：假设find指令的回传值为Ture，就将文件或目录名称列出到标准输出。格式为每列一个名称，每个名称前皆有“./”字符串；
-print0：假设find指令的回传值为Ture，就将文件或目录名称列出到标准输出。格式为全部的名称皆在同一行；
-printf<输出格式>：假设find指令的回传值为Ture，就将文件或目录名称列出到标准输出。格式可以自行指定；
-prune：不寻找字符串作为寻找文件或目录的范本样式;
-regex<范本样式>：指定字符串作为寻找文件或目录的范本样式；
-size<文件大小>：查找符合指定的文件大小的文件；
-true：将find指令的回传值皆设为True；
-typ<文件类型>：只寻找符合指定的文件类型的文件；
-uid<用户识别码>：查找符合指定的用户识别码的文件或目录；
-used<日数>：查找文件或目录被更改之后在指定时间曾被存取过的文件或目录，单位以日计算；
-user<拥有者名称>：查找符和指定的拥有者名称的文件或目录；
-version或——version：显示版本信息；
-xdev：将范围局限在先行的文件系统中；
-xtype<文件类型>：此参数的效果和指定“-type”参数类似，差别在于它针对符号连接检查。
```

## grep命令

    grep 指令用于查找内容包含指定的范本样式的文件，如果发现某文件的内容符合所指定的范本样式，预设 grep 指令会把含有范本样式的那一列显示出来。若不指定任何文件名称，或是所给予的文件名为 **-**，则 grep 指令会从标准输入设备读取数据。

```shell
# 语法
$ grep [-abcEFGhHilLnqrsvVwxy][-A<显示行数>][-B<显示列数>][-C<显示列数>][-d<进行动作>][-e<范本样式>][-f<范本文件>][--help][范本样式][文件或目录...]

# 使用案例
# 在当前目录中，查找后缀有file字样的文件中包含test字符串的文件，并打印出该字符串的行
$ grep test *file 
# 以递归的方式查找符合条件的文件。查找目录/etc/acpi及其子目录下所有文件中包含字符串update的文件，并打印出该字符串所在行的内容
$ grep -r update /etc/acpi 
# 反向查找。前面各个例子是查找并打印出符合条件的行，通过"-v"参数可以打印出不符合条件行的内容。
$ grep -v test *test*

# 参数选项
-a 或 --text : 不要忽略二进制的数据。
-A<显示行数> 或 --after-context=<显示行数> : 除了显示符合范本样式的那一列之外，并显示该行之后的内容。
-b 或 --byte-offset : 在显示符合样式的那一行之前，标示出该行第一个字符的编号。
-B<显示行数> 或 --before-context=<显示行数> : 除了显示符合样式的那一行之外，并显示该行之前的内容。
-c 或 --count : 计算符合样式的列数。
-C<显示行数> 或 --context=<显示行数>或-<显示行数> : 除了显示符合样式的那一行之外，并显示该行之前后的内容。
-d <动作> 或 --directories=<动作> : 当指定要查找的是目录而非文件时，必须使用这项参数，否则grep指令将回报信息并停止动作。
-e<范本样式> 或 --regexp=<范本样式> : 指定字符串做为查找文件内容的样式。
-E 或 --extended-regexp : 将样式为延伸的正则表达式来使用。
-f<规则文件> 或 --file=<规则文件> : 指定规则文件，其内容含有一个或多个规则样式，让grep查找符合规则条件的文件内容，格式为每行一个规则样式。
-F 或 --fixed-regexp : 将样式视为固定字符串的列表。
-G 或 --basic-regexp : 将样式视为普通的表示法来使用。
-h 或 --no-filename : 在显示符合样式的那一行之前，不标示该行所属的文件名称。
-H 或 --with-filename : 在显示符合样式的那一行之前，表示该行所属的文件名称。
-i 或 --ignore-case : 忽略字符大小写的差别。
-l 或 --file-with-matches : 列出文件内容符合指定的样式的文件名称。
-L 或 --files-without-match : 列出文件内容不符合指定的样式的文件名称。
-n 或 --line-number : 在显示符合样式的那一行之前，标示出该行的列数编号。
-o 或 --only-matching : 只显示匹配PATTERN 部分。
-q 或 --quiet或--silent : 不显示任何信息。
-r 或 --recursive : 此参数的效果和指定"-d recurse"参数相同。
-s 或 --no-messages : 不显示错误信息。
-v 或 --invert-match : 显示不包含匹配文本的所有行。
-V 或 --version : 显示版本信息。
-w 或 --word-regexp : 只显示全字符合的列。
-x --line-regexp : 只显示全列符合的列。
-y : 此参数的效果和指定"-i"参数相同。
```

## nc命令

nc是netcat的简写，有着网络界的瑞士军刀美誉。因为它短小精悍、功能实用，被设计为一个简单、可靠的网络工具

```shell
# nc的作用
1、实现任意TCP/UDP端口的侦听，nc可以作为server以TCP或UDP方式侦听指定端口
2、端口的扫描，nc可以作为client发起TCP或UDP连接
3、机器之间传输文件
4、机器之间网络测速   

# 语法
nc [-hlnruz][-g<网关...>][-G<指向器数目>][-i<延迟秒数>][-o<输出文件>][-p<通信端口>][-s<来源位址>][-v...][-w<超时秒数>][主机名称][通信端口...]

# 案例说明
# 扫描TCP端口: 扫描192.168.0.3 的端口 范围是 1-100
$ nc -v -z -w2 192.168.0.3 1-100 
# 扫描UDP端口: 扫描192.168.0.3 的端口 范围是 1-1000
$ nc -u -z -w2 192.168.0.3 1-1000 
# 扫描指定端口: 扫描 80端口
$ nc -nvv 192.168.0.1 80
# 监听9999端口
$ nc -lk 9999

# 参数选项
-g<网关> 设置路由器跃程通信网关，最多可设置8个。
-k 强制 nc在其当前连接完成后继续侦听另一个连接。只能和 -l选项一起使用。
-G<指向器数目> 设置来源路由指向器，其数值为4的倍数。
-h 在线帮助。
-i<延迟秒数> 设置时间间隔，以便传送信息及扫描通信端口。
-l 使用监听模式，管控传入的资料。
-n 直接使用IP地址，而不通过域名服务器。
-o<输出文件> 指定文件名称，把往来传输的数据以16进制字码倾倒成该文件保存。
-p<通信端口> 设置本地主机使用的通信端口。
-r 乱数指定本地与远端主机的通信端口。
-s<来源位址> 设置本地主机送出数据包的IP地址。
-u 使用UDP传输协议。
-v 显示指令执行过程。
-w<超时秒数> 设置等待连线的时间。
-z 使用0输入/输出模式，只在扫描通信端口时使用。
```

##  Sed命令

Linux sed 命令是利用脚本来处理文本文件。

sed 可依照脚本的指令来处理、编辑文本文件。

Sed 主要用来自动编辑一个或多个文件、简化对文件的反复操作、编写转换程序等

```shell
# 语法
$ sed [-hnV][-e<script>][-f<script文件>][文本文件]

# 使用案例
# 在testfile文件的第四行后添加一行，并将结果输出到标准输出：
sed -e 4a\newLine testfile 
# 将当前目录下包含"qwe"串的文件中的"qwe"字符串替换为"abc"
sed -i "s/qwe/abc/g" `grep "qwe" -rl ./`
# 将某个文件中的"qwe"字符串替换为"abc"
sed -i "s/qwe/abc/g" test.txt
# 如果将某个文件中以"qwe"开头的字符串修改为“abc”
sed -i "s/qwe*/abc/g" test.txt

# 参数选项
-e<script>或--expression=<script>  以选项中指定的script来处理输入的文本文件。
-f<script文件>或--file=<script文件>  以选项中指定的script文件来处理输入的文本文件。
-h或--help  显示帮助。
-n或--quiet或--silent  仅显示script处理后的结果。
-V或--version  显示版本信息。

# 动作说明
a ：新增， a 的后面可以接字串，而这些字串会在新的一行出现(目前的下一行)～
c ：取代， c 的后面可以接字串，这些字串可以取代 n1,n2 之间的行！
d ：删除， 因为是删除啊，所以 d 后面通常不接任何咚咚；
i ：插入， i 的后面可以接字串，而这些字串会在新的一行出现(目前的上一行)；
p ：打印，亦即将某个选择的数据印出。通常 p 会与参数 sed -n 一起运行～
s ：取代，可以直接进行取代的工作哩！通常这个 s 的动作可以搭配正规表示法！例如 1,20s/old/new/g 就是啦！
```

# 附录A

## 参考文档地址

[Linux命令大全](https://www.runoob.com/linux/linux-command-manual.html)















