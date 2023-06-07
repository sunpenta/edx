输入命令：
```bash
curl -v -k "https://www.wikidata.org/w/api.php?action=wbgetentities&format=json&titles=Moon&sites=enwiki" | 
jq .
```
-v: 输出详细信息
-k:不使用安全证书验证
错误：
在Cygwin上：
```bash
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0*
  Trying 103.102.166.224:443...
* Connected to www.wikidata.org (103.102.166.224) port 443 (#0)
* schannel: disabled automatic use of client certificate
* ALPN: offers http/1.1
  0     0    0     0    0     0      0      0 --:--:--  0:00:27 --:--:--     0*
Recv failure: Connection was reset
* schannel: failed to receive handshake, SSL/TLS connection failed
  0     0    0     0    0     0      0      0 --:--:--  0:00:27 --:--:--     0
* Closing connection 0
* schannel: shutting down SSL/TLS connection with www.wikidata.org port 443
* Send failure: Connection was reset
* schannel: failed to send close msg: Failed sending data to the peer (bytes wri
tten: -1)
curl: (35) Recv failure: Connection was reset
```
在WSL2上：
```bash
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0*   Trying 103.102.166.224:443...
* Connected to www.wikidata.org (103.102.166.224) port 443 (#0)
* ALPN, offering h2
* ALPN, offering http/1.1
* TLSv1.0 (OUT), TLS header, Certificate Status (22):
} [5 bytes data]
* TLSv1.3 (OUT), TLS handshake, Client hello (1):
} [512 bytes data]
  0     0    0     0    0     0      0      0 --:--:--  0:01:59 --:--:--     0* TLSv1.0 (OUT), TLS header, Unknown (21):
} [5 bytes data]
* TLSv1.3 (OUT), TLS alert, decode error (562):
} [2 bytes data]
* error:0A000126:SSL routines::unexpected eof while reading
  0     0    0     0    0     0      0      0 --:--:--  0:02:00 --:--:--     0
* Closing connection 0
curl: (35) error:0A000126:SSL routines::unexpected eof while reading
```
升级openssl版本
1. `openssl version`查看版本
2. 下载最新版
在[官网](https://www.openssl.org/source/old/)查看最新版本。
下载：
```bash
wget https://www.openssl.org/source/openssl-3.1.0.tar.gz
```
3. 解压
```
tar -xzvf openssl-3.1.0.tar.gz
```
4. 配置
``` 
cd openssl-3.1.0
./config
```
5. 编译，安装
```
make sudo make install
```
6. 查看
在当前窗口查看版本还是旧版本，打开新窗口显示新版本。





