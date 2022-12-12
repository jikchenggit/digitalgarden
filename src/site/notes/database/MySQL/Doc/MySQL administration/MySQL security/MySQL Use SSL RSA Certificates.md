---
{"author":"aming","email":"jikcheng@163.com","title":"MySQL Use SSL RSA Certificates","creation_date":"2022-06-27 15:57","Last modified date":"2022-12-12 15:23","tags":"MySQL Use SSL RSA Certificates","File Folder with relative path":"database/MySQL/Doc/MySQL administration/MySQL security","remark":null,"other":null,"dg-publish":true,"permalink":"/database/my-sql/doc/my-sql-administration/my-sql-security/my-sql-use-ssl-rsa-certificates/","dgPassFrontmatter":true}
---






## 使用MySQL创建SSL和RSA证书和密钥

MySQL 提供以下方法创建SSL 和RSA密钥文件,用于加密连接,并在未加密的连接上使用RSA安全密码交换:



使用编译版本的MySQL 发行版时, 服务在启动时自动生成这些文件.


用户可以手动调用 mysql_ssl_rsa_setup 生成文件.


对于RPM 安装后的平台,数据库目录初始化期间 调用mysql_ssl_rsa_setup 生成文件.只要openssl 命令存在,就不需要编译MySQL .

:::alert-warning
mysql_ssl_rsa_setup 使用得时openssl 命令,所以他的使用取决于您得机器上是否安装了openssl.
对于使用编译版本得MySQL  还有另外一种生成密钥文件得方式,请参见<创建和使用SSL 和RSA 认证>
mysql_ssl_rsa_setup 用于方便快捷的产生SSL 的一种工具. 如果想要更加安全的方式,请从注册证书办法机构获得CA 证书.
:::

### 自动产生SSL 和RSA 

对于 msyql 的编译版本.MySQL 启动服务器会自动产生SSL ,RSA 文件.而 auto_generate_certs, sha256_password_auto_generate_rsa_keys, and caching_sha2_password_auto_generate_rsa_keys  这些系统变量控制这些文件. 默认情况下,这两个变量时启用的,他们不能再运行的时候设置.



在启动时,如果启用了 auto_generate_certs  系统变量,服务器将自动在数据目录中生成服务器端和客户端SSL证书和密钥文件,并且不需要指定SSL选项. 更多请见<配置MySQL 服务器使用加密连接>

The server checks the data directory for SSL files with the following names:

1. 服务器将会在数据目录下,检查以下的SSL 文件名称.
```bash
ca.pem
server-cert.pem
server-key.pem
```
2. 如果文件名存在,则不会创建任何文件,如果不存在,创建的文件名如下:

```
ca.pem               Self-signed CA certificate
ca-key.pem           CA private key
server-cert.pem      Server certificate
server-key.pem       Server private key
client-cert.pem      Client certificate
client-key.pem       Client private key
```
3. 如果服务器自动生成了SSL 文件,它将使用ca.pem, server-cert.pem, and server-key.pem 文件赋值给 (ssl_ca, ssl_cert, ssl_key). 这两个系统变量.


在MySQL 服务器启动时 ,服务器会自动产生RSA 私有/共有 密钥对文件 .产生的位置位data 目录.如果所有的条件允许: sha256_password_auto_generate_rsa_keys or caching_sha2_password_auto_generate_rsa_keys system variable 系统变量启用; 
RSA 选项没有被显示指定;
RSA 文件在data 目录中不存在. 
这些密钥对将会使用RSA在未加密的连接中 开启安全密码交换 . 并且通过 sha256_password  或 caching_sha2_password  插件 验证账户.

1. RSA 文件目录名称
```bash
private_key.pem      Private member of private/public key pair
public_key.pem       Public member of private/public key pair
```
2 .如果这些文件名已经存在,则server 将不会创建或者覆盖.
3. 如果服务自动创建了RSA 文件,对应的系统变量是 (sha256_password_private_key_path and sha256_password_public_key_path; caching_sha2_password_private_key_path and caching_sha2_password_public_key_path).
### 手动 使用mysql_ssl_rsa_setup产生 RSA 和SSL文件.
MySQL distributions include a mysql_ssl_rsa_setup utility that can be invoked manually to generate SSL and RSA files. This utility is included with all MySQL distributions (whether compiled using OpenSSL or wolfSSL), but it does require that the openssl command be available. For usage instructions, see Section 4.4.3, “mysql_ssl_rsa_setup — Create SSL/RSA Files”.

mysql 发行版 包括一个mysql_ssl_rsa_setup 程序,这个程序可以手动调用用来产生SSL 和RSA 文件,这个程序在所有的MySQL 的发型版中,(无论使用openSSL 或 wolfSSL) , 都需要openssl 命令可用,对于用法请参见 


#### 1.2.1. SSL 和RSA 的文件特征

SSL and RSA files created automatically by the server or by invoking mysql_ssl_rsa_setup have these characteristics:

是有服务器自动创建或调用 mysql_ssl_rsa_setup  产生的SSL 和RSA 文件有以下特征:

- SSL 和RSA 密钥大小位2048 位.
- SSL 的CA 证书是自签名的.
- SSL 服务和客户端认证是使用的CA 证书和密钥,使用的是sha256WithRSAEncryption签名的算法.
- SSL  证书使用的文件名称是默认名称.

```
ca.pem:         MySQL_Server_suffix_Auto_Generated_CA_Certificate
server-cert.pm: MySQL_Server_suffix_Auto_Generated_Server_Certificate
client-cert.pm: MySQL_Server_suffix_Auto_Generated_Client_Certificate
```


suffix  位后缀值, 对于 mysql_ssl_rsa_setup 生成的文件,可以使用--suffix 选项指定后缀.如果生成的CN 值 超过了64 字符, 则会省略_suffix 部分.

- 使用SSL 可以 国家 (C),州或省(ST) ,组织(O) ,组织单元名称(OU) 和电子邮件都可以为空.
-  SSL files created by the server or by mysql_ssl_rsa_setup are valid for ten years from the time of generation.
-  由服务器 或 mysql_ssl_rsa_setup 创建的SSL 文件 10 年内有校.
- RSA 文件不会过期.
- SSL 文件 对于每个证书/密钥 对 都有不同的序列号(1 代表CA ,2 代表服务器,3代表客户机)
- 服务器自动创建的文件由服务器的管理账号所有.使用 mysql_ssl_rsa_setup 创建文件是由调用程序的用户拥有. 使用`--uid`  可以改变生成的证书所有权限.
-  在unix 和类unix 系统上.证书文件的文件访问模式是644(都可以读) ,关键文件的文件访问模式位600 .

To see the contents of an SSL certificate (for example, to check the range of dates over which it is valid), invoke openssl directly:

如果要查看证书内容 (例如:检查有效期范围) ,请直接调用openssl
```bash
openssl x509 -text -in ca.pem
openssl x509 -text -in server-cert.pem
openssl x509 -text -in client-cert.pem
```

还可以用sql 语句检查:

```sql
mysql> SHOW STATUS LIKE 'Ssl_server_not%';
+-----------------------+--------------------------+
| Variable_name         | Value                    |
+-----------------------+--------------------------+
| Ssl_server_not_after  | Apr 28 14:16:39 2027 GMT |
| Ssl_server_not_before | May  1 14:16:39 2017 GMT |
+-----------------------+--------------------------+
```



### 使用openssl 创建SSL 认证证书和密钥对

如何使用openssl 命令设置SSL 证书和密钥文件. 如何提供给MySQL 服务器和哭护短使用. 第一个示例显示了一个简化的过程,如果从命令行使用,第二个脚本 能够提供希捷的脚本.前两个示例用于unix ,第三个例子描述windows 设置SSL 文件.





无论采取什么样的方式产生认证证书和密钥文件.请确认 Common name值  服务器和客户端的证书/密钥  每个都不同.,否则会出现以下错误.

`ERROR 2026 (HY000): SSL connection error:`
`error:00000001:lib(0):func(0):reason(1)`


e.g.1: 手动生成认证证书和密钥:
```bash

# 2. Create clean environment
rm -rf newcerts
mkdir newcerts && cd newcerts

# 3. Create CA certificate
openssl genrsa 2048 > ca-key.pem
openssl req -new -x509 -nodes -days 3600 \
        -key ca-key.pem -out ca.pem

# 4. Create server certificate, remove passphrase, and sign it
# 5. server-cert.pem = public key, server-key.pem = private key
openssl req -newkey rsa:2048 -days 3600 \
        -nodes -keyout server-key.pem -out server-req.pem
openssl rsa -in server-key.pem -out server-key.pem
openssl x509 -req -in server-req.pem -days 3600 \
        -CA ca.pem -CAkey ca-key.pem -set_serial 01 -out server-cert.pem

# 6. Create client certificate, remove passphrase, and sign it
# 7. client-cert.pem = public key, client-key.pem = private key
openssl req -newkey rsa:2048 -days 3600 \
        -nodes -keyout client-key.pem -out client-req.pem
openssl rsa -in client-key.pem -out client-key.pem
openssl x509 -req -in client-req.pem -days 3600 \
        -CA ca.pem -CAkey ca-key.pem -set_serial 01 -out client-cert.pem
```


验证证书有效性:
```bash
openssl verify -CAfile ca.pem server-cert.pem client-cert.pem
```
查看证书内容和有效期:
```bash

openssl x509 -text -in ca.pem
openssl x509 -text -in server-cert.pem
openssl x509 -text -in client-cert.pem
```




ca.pem: 对应使用`--ssl-ca `参数指定.
server-cert.pem: 使用`--ssl-cert` 和`ssl-key` 参数指定.
client-cert.pem ,client-key.pem :使用客户端使用`--ssl-cert`和 `--ssl-key` 作为参数指定.



eg2:创建SSL 文件使用脚本方式.

```bash
DIR=`pwd`/openssl
PRIV=$DIR/private

mkdir $DIR $PRIV $DIR/newcerts
cp /usr/share/ssl/openssl.cnf $DIR
replace ./demoCA $DIR -- $DIR/openssl.cnf

# 2. Create necessary files: $database, $serial and $new_certs_dir
# 3. directory (optional)

touch $DIR/index.txt
echo "01" > $DIR/serial

#
# 4. Generation of Certificate Authority(CA)
#

openssl req -new -x509 -keyout $PRIV/cakey.pem -out $DIR/ca.pem \
    -days 3600 -config $DIR/openssl.cnf

# 5. Sample output:
# 6. Using configuration from /home/monty/openssl/openssl.cnf
# 7. Generating a 1024 bit RSA private key
# 8. ................++++++
# 9. .........++++++
# 10. writing new private key to '/home/monty/openssl/private/cakey.pem'
# 11. Enter PEM pass phrase:
# 12. Verifying password - Enter PEM pass phrase:
# 13. -----
# 14. You are about to be asked to enter information that will be
# 15. incorporated into your certificate request.
# 16. What you are about to enter is what is called a Distinguished Name
# 17. or a DN.
# 18. There are quite a few fields but you can leave some blank
# 19. For some fields there will be a default value,
# 20. If you enter '.', the field will be left blank.
# 21. -----
# 22. Country Name (2 letter code) [AU]:FI
# 23. State or Province Name (full name) [Some-State]:.
# 24. Locality Name (eg, city) []:
# 25. Organization Name (eg, company) [Internet Widgits Pty Ltd]:MySQL AB
# 26. Organizational Unit Name (eg, section) []:
# 27. Common Name (eg, YOUR name) []:MySQL admin
# 28. Email Address []:

#
# 29. Create server request and key
#
openssl req -new -keyout $DIR/server-key.pem -out \
    $DIR/server-req.pem -days 3600 -config $DIR/openssl.cnf

# 30. Sample output:
# 31. Using configuration from /home/monty/openssl/openssl.cnf
# 32. Generating a 1024 bit RSA private key
# 33. ..++++++
# 34. ..........++++++
# 35. writing new private key to '/home/monty/openssl/server-key.pem'
# 36. Enter PEM pass phrase:
# 37. Verifying password - Enter PEM pass phrase:
# 38. -----
# 39. You are about to be asked to enter information that will be
# 40. incorporated into your certificate request.
# 41. What you are about to enter is what is called a Distinguished Name
# 42. or a DN.
# 43. There are quite a few fields but you can leave some blank
# 44. For some fields there will be a default value,
# 45. If you enter '.', the field will be left blank.
# 46. -----
# 47. Country Name (2 letter code) [AU]:FI
# 48. State or Province Name (full name) [Some-State]:.
# 49. Locality Name (eg, city) []:
# 50. Organization Name (eg, company) [Internet Widgits Pty Ltd]:MySQL AB
# 51. Organizational Unit Name (eg, section) []:
# 52. Common Name (eg, YOUR name) []:MySQL server
# 53. Email Address []:
#
# 54. Please enter the following 'extra' attributes
# 55. to be sent with your certificate request
# 56. A challenge password []:
# 57. An optional company name []:

#
# 58. Remove the passphrase from the key
#
openssl rsa -in $DIR/server-key.pem -out $DIR/server-key.pem

#
# 59. Sign server cert
#
openssl ca -cert $DIR/ca.pem -policy policy_anything \
    -out $DIR/server-cert.pem -config $DIR/openssl.cnf \
    -infiles $DIR/server-req.pem

# 60. Sample output:
# 61. Using configuration from /home/monty/openssl/openssl.cnf
# 62. Enter PEM pass phrase:
# 63. Check that the request matches the signature
# 64. Signature ok
# 65. The Subjects Distinguished Name is as follows
# 66. countryName           :PRINTABLE:'FI'
# 67. organizationName      :PRINTABLE:'MySQL AB'
# 68. commonName            :PRINTABLE:'MySQL admin'
# 69. Certificate is to be certified until Sep 13 14:22:46 2003 GMT
# 70. (365 days)
# 71. Sign the certificate? [y/n]:y
#
#
# 72. 1 out of 1 certificate requests certified, commit? [y/n]y
# 73. Write out database with 1 new entries
# 74. Data Base Updated

#
# 75. Create client request and key
#
openssl req -new -keyout $DIR/client-key.pem -out \
    $DIR/client-req.pem -days 3600 -config $DIR/openssl.cnf

# 76. Sample output:
# 77. Using configuration from /home/monty/openssl/openssl.cnf
# 78. Generating a 1024 bit RSA private key
# 79. .....................................++++++
# 80. .............................................++++++
# 81. writing new private key to '/home/monty/openssl/client-key.pem'
# 82. Enter PEM pass phrase:
# 83. Verifying password - Enter PEM pass phrase:
# 84. -----
# 85. You are about to be asked to enter information that will be
# 86. incorporated into your certificate request.
# 87. What you are about to enter is what is called a Distinguished Name
# 88. or a DN.
# 89. There are quite a few fields but you can leave some blank
# 90. For some fields there will be a default value,
# 91. If you enter '.', the field will be left blank.
# 92. -----
# 93. Country Name (2 letter code) [AU]:FI
# 94. State or Province Name (full name) [Some-State]:.
# 95. Locality Name (eg, city) []:
# 96. Organization Name (eg, company) [Internet Widgits Pty Ltd]:MySQL AB
# 97. Organizational Unit Name (eg, section) []:
# 98. Common Name (eg, YOUR name) []:MySQL user
# 99. Email Address []:
#
# 100. Please enter the following 'extra' attributes
# 101. to be sent with your certificate request
# 102. A challenge password []:
# 103. An optional company name []:

#
# 104. Remove the passphrase from the key
#
openssl rsa -in $DIR/client-key.pem -out $DIR/client-key.pem

#
# 105. Sign client cert
#

openssl ca -cert $DIR/ca.pem -policy policy_anything \
    -out $DIR/client-cert.pem -config $DIR/openssl.cnf \
    -infiles $DIR/client-req.pem

# 106. Sample output:
# 107. Using configuration from /home/monty/openssl/openssl.cnf
# 108. Enter PEM pass phrase:
# 109. Check that the request matches the signature
# 110. Signature ok
# 111. The Subjects Distinguished Name is as follows
# 112. countryName           :PRINTABLE:'FI'
# 113. organizationName      :PRINTABLE:'MySQL AB'
# 114. commonName            :PRINTABLE:'MySQL user'
# 115. Certificate is to be certified until Sep 13 16:45:17 2003 GMT
# 116. (365 days)
# 117. Sign the certificate? [y/n]:y
#
#
# 118. 1 out of 1 certificate requests certified, commit? [y/n]y
# 119. Write out database with 1 new entries
# 120. Data Base Updated

#
# 121. Create a my.cnf file that you can use to test the certificates
#

cat <<EOF > $DIR/my.cnf
[client]
ssl-ca=$DIR/ca.pem
ssl-cert=$DIR/client-cert.pem
ssl-key=$DIR/client-key.pem
[mysqld]
ssl-ca=$DIR/ca.pem
ssl-cert=$DIR/server-cert.pem
ssl-key=$DIR/server-key.pem
EOF
```
eg3: windows 上创建SSL 文件
略
### 使用opnessl 创建RSA 密钥
如何使用openssl 命令设置RSA 密钥文件.使MySQL 能够支持通过sha256_password和caching_sha2_password 的 插件的密码交换功能.
```bash

openssl genrsa -out private_key.pem 2048
openssl rsa -in private_key.pem -pubout -out public_key.pem
```
上面的命令创建2048 位密钥,如果要创建更强奸的键,请使用更大的值.

设置密钥的 访问哦是.私钥只能有服务器读取:而公钥可以公开发放给客户端用户.
```bash
chmod 400 private_key.pem
chmod 444 public_key.pem
```
###  wolfSSL 生成密钥
略