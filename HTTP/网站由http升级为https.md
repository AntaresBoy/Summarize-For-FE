# 网站由http升级为https

## 1.openssl申请ssl证书

使用命令生成MyCertificate.crt代表证书名，MyKey代表密钥名，证书和秘钥名可以自定义：

```shell
openssl req -new -newkey rsa:4096 -x509 -sha256 -days 365 -nodes -out MyCertificate.crt -keyout MyKey.key
```

- -newkey  rsa:4096：创建一个4096位的RSA密钥
- -x509: 创建一个自签名证书-sha256：使用256位的SHA算法生成证书。
- -days：证书的有效期限。
- -nodes：创建的证书不需要密码。如果不加这个参数，每次应用密码证书的时候都需要人为的输入密码。
- -out 生成证书的路径及名称。
- -keyout 生成密钥的路径及名称。

执行以上命令后会出现一些需要填写的信息：

```markdown
.......................................................................++
.....................................................................++
writing new private key to 'MyKey.key'
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:CN
State or Province Name (full name) [Some-State]:GUANGDONG
Locality Name (eg, city) []:SHENZHEN
Organization Name (eg, company) [Internet Widgits Pty Ltd]:LPQ
Organizational Unit Name (eg, section) []:BLOGS
Common Name (e.g. server FQDN or YOUR name) []:39.108.119.239
Email Address []:2531646835@qq.com

openssl req -new -newkey rsa:4096 -x509 -sha256 -days 365 -nodes -out MyCertificate.crt -keyout MyKey.key

openssl -subj "/C=CN/ST=HB/L=WH/O=My Company/OU=EDU/CN=hostname.example.com/emailAddress=xxxxx@qq.com"

```

- Country Name:国家码 (中国CN)
- State or Province Name：省份
- Locality Name：所在城市
- Origanization Name：机构名
- Origanizational Unit Name：机构单位名
- Common Name：需要证书的域名或者IP
- Email Address:邮箱

注：可以使用-subj预先定义好信息，一次性执行：

```shell
openssl -subj "/C=CN/ST=HB/L=WH/O=My Company/OU=EDU/CN=hostname.example.com/emailAddress=2afef@163.com"
```

安全起见，最好只让root用户才能访问，并且是只读的:

```shell
chmod 400 MyKey.key
```

## 2.升级为HTTPS

### 2.1、兼顾http

nginx配置：

```nginx
    server {
            listen       9595;
            listen       443 ssl;
            server_name  39.108.119.239;
            charset      urf-8;
            #ssl         on;
            ssl_certificate /etc/nginx/conf.d/ssl_key/MyCertificate.crt;
            ssl_certificate_key /etc/nginx/conf.d/ssl_key/MyKey.key;
            ssl_session_timeout 10m;
            ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
            ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;
            ssl_prefer_server_ciphers on;
           #return 301 https://$server_name$request_uri;
            location /{
                # root /home/www/BlogUI/dist;
                root   /home/www/dist;
                index index.html index.htm;
                }

    }

```

注：开启ssl on 这个配置项，这会导致无法使用http 访问。所以保证这个被注释掉。

### 2.2、访问http重定向到http

输入http协议下的域名时会自动重定向到https

nginx配置：

```nginx
    server {
        listen 9595;
        server_name  39.108.119.239;
        #告诉浏览器有效期内只准用 https 访问
        add_header Strict-Transport-Security max-age=15768000;
        #永久重定向到 https 站点
        return 301 https://$server_name$request_uri;
    }

```

