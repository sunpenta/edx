### 1.4 Files and directories
* 文件和目录
命令：
pwd:查看
mkdir：创建
cd：切换
路径 / /
ls
root dev lib home usr ... 常见目录
* 操作文件
命令：
echo > ：输出重定向
touch ：创建文件
ls
mv ：移动
rm ：删除文件
mkdir
cp ：copy
rmdir 
* 递归:删除内容不为空的文件、目录
-r
-R
-rf
### 1.6 Command grouping
1. 和以下命令输出相同的是
l s /etc | cat -n
ls /etc >/tmp/ls ; cat -n </tmp/ls
2. 执行以下命令，
{ ls /tmp/x && rm /tmp/x ; } 
{ ls /tmp/x || touch /tmp/x ; }
如果tmp/x存在,旧的tmp/x被删除; 新的tmp/x总会被创建
3. { echo -n 'hello '; echo world ; } >out
out的内容: hello world
### 1.7 Scripting
* 创建脚本
 #! hashbang
chmod
* 传递参数
* shell函数，模块化

执行shell script,需要的的步骤:
1.第一行是 #!/bin/sh
2.把命令所在路径添加PATH环境变量
3.给命令执行权限

### 1.8 Excution control
* for / 嵌套for 例子接1.7
* if / if else
* while
* xargs :大数据
* case
1. 以下命令
if a; then 
    b
fi
相当于
a && b
2. echo a | xargs ls 相当于
echo a | while read i; do ls $i; done
ls a
### 1.9 数据处理流程
1. 抓取fetch
git log
2. 选择select
cut -d, -f1 :-d:设置字段分隔符<tab> -f1:?
3. 处理process
sort
4. 总结summarize
uniq
5. 报告report
sort -rn
例子：程序员一周产量最高的日子