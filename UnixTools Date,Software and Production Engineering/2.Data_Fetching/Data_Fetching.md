# 二 数据抓取
## 0. 介绍
&emsp;在第一部分已学习使用Unix Shell的基础，下面开始深入学习数据处理管道(pipeline)的第一部分，即从不同数据来源抓取和产生数据。
&emsp;该部分内容包含从网络抓取数据，从数据库检索数据，从远端主机发送和接收数据，检查配置管理记录中的历史和贡献者，定位满足特定标准的文件，检查可执行文件和库，检查和操控档案，从用户图形界面得到数据，如何产生数字序列和数据块。
## 1. 远端服务
数据来源：internet上的静态或动态数据，相关数据库
* 从web 抓取文件命令：
curl -s --cpmpressed url |
head 
wc
s: 静默 cpmpressed:压缩
* 从web抓取数据
curl -s "" |
jq .
* json 数据
WGE='url' # API endpoint
curl -s "WGE&<>" |
jq -r ''
* query 相关数据库
echo 'SELECT ' | 
mysql -ughtorrent -p <>
* 其他数据库 
sqlplus # Oracle
osql # microsoft SQL Server
sqlite3 # SQLite engineer
psql # PostgreSQL
odbc # Any ODBC source
* put it together
echo 'select url from' |
mysql - ughtorrent -p <>
while
cur |
jq -r
done;
* query LDAP 数据
LDAP:轻量型客户端-服务端协议。不遵循http协议，遵循ldap
参考材料：
curl: [wiki](https://en.wikipedia.org/wiki/CURL)
jq: [jq官网](https://jqlang.github.io/jq/)
练习题：
1. What is the length (in characters) of the file located at https://www.spinellis.gr/unix/data/hier.txt?
使用命令：
```bash
curl -s --compressed https://www.spinellis.gr/unix/data/hier.txt | wc
```
第一个数是行数，第2个数是单词个数，第3个数是字节数。
答案：25642
2. What is the approximate mass (in yottagrams) of the planet identified in wikidata with the identifier Q111?
```bash
curl -v -k "https://www.wikidata.org/w/api.php?action=wbgetentities&format=json&ids=Q111" |
jq -r .entities.Q111.claims.P2067[].mainsnak.datavalue.value.amount
```
3. What is the easiest way to obtain data from a relational database for further processing with Unix shell tools?
通过数据库的命令行界面。
4. Which of the following protocols are supported by curl?
通过 `curl --version` 得到：
 dict file ftp ftps gopher gophers http https imap imaps ldap ldaps mqtt pop3 pop3s rtmp rtsp scp sftp smb smbs smtp smtps telnet tftp
 ## 2.2 归档文件
 arhives：多个文件存在一个文件里，size不变（区别压缩文件compressed file，size变小）。
 * 列出归档文件（解压）
 cur -Ls url.tar.gz |  # fetch
 tar -tzvf - | # t:列出archive的内容 z:该archive通过gzip压缩，v:不只是列出条目的名字，输出多的信息, f:从一个特定位置检索该文件，而不是从连接主机的tap archive。这个位置就是curl的输出的位置，即tar的标准输入。-:归档文件的标准输入。
 head -10 # 查看10个条目
 经过tar之后，归档文件里的内容不再是一个整体。
 * 提取文件
 curl -Ls url.tar.gz |
 tar -xzf - # x:提取archive的内容
 ls
 du -sh <name> # 得到<name>大小
 * 创建归档文件
 tar -cf <name.tar> <dir/name> # c:从dir创建归档文件
 tar -cf - <dir/name> | # -: 发送输出到标准输出
 gzip -c >name.tar.gz # c: 压缩标准输入，重定向到name.tar.gz
 ls -lh # 压缩文件的size
 tar直接创建压缩文件:
 tar -czf name.tar.gz <dir/name>
 * 连接归档命令
 复制predir到dir
 mkdir \<dir>
 (
cd \<predir>
tar -cf - .
 ) | (
cd \<dir>
tar -xf -
 )
 C：直接标记tar将转向的当前目录
 上面的命令等价于：
 tar -C \<predir> -cf - . | tar -C \<dir> -xf -
 * 列出库的内容
 ar tv <dir.a> |
 head
 * 列出jar 文件内容
 jar tvf <dir.jar> |
 head
 * windows
 windows上的格式：zip
 unzip -v <.zip> |
 * 压缩数据
 bzip2, gzip工具
 gzip -dc <.tar.gz | bzip -c >.tar.bz2
 ls -lh .tar.bz2 # 文件大小
练习题：
1. 以下哪个命令是解压存储多个文件的压缩包命令：
ar，arch，gunzip，jar，tar，unzip，xargs
答案：ar,jar,tar,unzip
ar: 目标文件，jar: java类文件, tar: 通用, unzip: windows上的zip文件 
arch: 展示处理器的架构，gunzip: 解压压缩文件，当不打开存储多个文件的归档文件, xargs: 作为标准输入参数用于特定命令中
2. 以下哪个命令可以copy the directory /home/mary/run to /home/mary/work?

tar -C /home/mary -cf - run | tar -C /home/mary/work -xf -  ✅

tar -C /home/mary -xf - run | tar -C /home/mary/work -cf -

tar -cf - /home/mary/run | tar -C /home/mary/work -xf -  🏁？

tar -cf - /home/mary/run | tar -C /home/mary -xf -

(cd /home/mary ; tar -cf - run) | (cd /home/mary/work ; tar -xf -) ✅

(cd /home/mary/run ; tar -cf - .) |
(mkdir /home/mary/work/run ; cd /home/mary/work/run ; tar -xf -) ✅

(cd /home/mary ; tar -cf - run) |
(mkdir /home/mary/work/run ; cd /home/mary/work/run ; tar -xf -)

(cd /home/mary ; tar -cf - run) | (tar -C /home/mary/work -xf -) ✅

tar -C /home/mary -cf /tmp/run.tar run ; tar -C /home/mary/work -xf /tmp/run.tar ✅
归档文件的创建必须在run目录下或者它的父目录里，必须提取到work目录或相应的run目录。
目录的改变通过-C或这cd 来实现。
3.  该archive https://www.spinellis.gr/unix/data/x.tar.gz包含多少条目？
curl -Ls "https://www.spinellis.gr/unix/data/x.tar.gz" |
tar -tzf - |
wc -l
输出即条目个数：512。
4. 用`ls`命令的输出来创建压缩文件
ls -lR /home/joe | gzip -c >ls.out.gz ✅ 

ls -lR /home/joe | gzip -c ls.out.gz

ls -lR /home/joe >ls.out ; gzip ls.out ✅

ls -lR /home/joe <ls.out ; gzip >ls.out

ls -lR /home/joe | bzip2 -c >ls.out.bz2 ✅

ls -lR /home/joe | bzip2 -c ls.out.bz2

ls -lR /home/joe >ls.out ; bzip2 ls.out ✅

ls -lR /home/joe ; bzip2 -c >ls.out.bz2
1. 使用-c 选项需要重定向到目标文件.gz/.bz2；
2. 把ls的输出重定向到临时文件，然后作为参数传递给gzip/bzip2
## 3. 远程主机
1. 创建私钥/公钥对
mkdir .ssh # 存储密钥对
ssh-keygen
当为远程访问获取创建公/私密钥对，私钥存储在本地主机，公钥存储在远程主机。
2. 用私钥登录
mkdir -p .ssh
ssh hostname command
3. 配置
ssh -i 
4. 使用ssh
hostname # 输出所在主机
ssh hostname # 登录主机
ssh hostname command 
5. pipe 
tar -czf - 
ssh server dd of= bs=1M
              if=
6. 穿越防火墙
ssh -f -L(指定本机端口)
ps x # 查看进程
kill 杀死进程
7. 同步文件
tar,ssh 浪费时间和精力
两个主机目录内容同步
rsync -av :
在一个主机删除
练习题：
1. 下面哪个命令利用公私密钥对访问远程主机？
cp ：本地复制

exec ：本地执行

git ：利用ssh访问远程主机

rcp

rexec

rsh

rsync ✅

scp ✅

ssh ✅

telnet

uucp

rcp, rexec, rsh, telnet, uucp 是遗留的远程主机访问命令，不支持私钥授权。
2. 以下哪个命令可以在远程主机trantor上创建一个本地文件a的副本？
scp a trantor: ✅

scp ./a trantor:a ✅

scp a trantor

cp a trantor:

tar -cf - a | ssh trantor tar -xf - ✅

ssh trantor tar -cxf - <a

rsync a trantor

rsync ./a trantor: ✅

ssh trantor dd of=a <a ✅