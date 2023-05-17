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

### 1.8 excution control
* for
* if / if else
* while
* xargs :大数据
* case
### 1.9 数据处理流程
1. fetch
2. select
3. process
4. sumirize
5. report
例子：程序员一周产量最高的日子