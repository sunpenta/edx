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