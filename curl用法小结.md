
### host设置

```bash
curl -u username:password -H 'host:www.baidu.com' "http://47.74.43.242:10086/"

```

### post方法请求

```bash
# post数据(参数在header信息里)
curl -d "user=Summer&passwd=12345678" "http://47.74.43.242:10086/"

# post数据(参数在body信息里)
curl -H "Content-Type:application/json" -X POST --data '{"message": "sunshine"}' http://localhost:8000/

```

### web验证

```bash
[ronny@MacPro2018.local:~]$ curl -u username:password -I http://47.74.43.242:10086/

```


### cookie相关

```bash
# 设置cookie
curl --cookie "name1=value1" http://www.example.com

# 保存cookie
curl --cookie-jar cookie.txt "http://ip:port/pay/callback/deshinew/2701"

```

### 使用代理

```bash
curl -x ip:port http://www.example.com

```

### 输出详细的交互过程

```bash
# ascii码，可读性更强
curl http://www.example.com --trace-ascii /dev/stdout

# 含16进制，看不懂
curl http://www.example.com --trace trace.txt

```

### 监控网页的响应时间

```bash
tee curl-format.txt <<- 'EOF'
time_namelookup: \t%{time_namelookup}\n
time_connect: \t\t%{time_connect}\n
time_appconnect: \t%{time_appconnect}\n
time_redirect: \t\t%{time_redirect}\n
time_pretransfer: \t%{time_pretransfer}\n
time_starttransfer: \t%{time_starttransfer}\n
----------\n
time_total: \t\t%{time_total}\n\n
EOF

# 定义请求目标
DOMAIN="www.qq.com"

# 发送请求
curl -o /dev/null -s -w '@curl-format.txt' $DOMAIN

# 释义（可以在man文档里查看所有的内置变量及其含义）
time_appconnect：SSL 握手耗时

```

### 盗链
```bash
curl -e "mail.yahoo.com" http://47.74.43.242:10086/

```

### 限速

```bash
curl --limit-rate 10K http://47.74.43.242:10086/
curl --limit-rate 1 http://47.74.43.242:10086/

```

## 获取指定的内容
```bash
curl -o /dev/null -sL -w "%{http_code}" "$CheckIP:$Port"
```


