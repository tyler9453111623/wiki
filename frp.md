
## 参考
https://www.cnblogs.com/zhanggaoxing/p/9221705.html


## 下载
https://github.com/fatedier/frp/releases


配置文件

```bash
[root@frp01:~/frp_0.13.0_linux_amd64]# cat frpc.ini
[common]
bind_port = 7000
vhost_http_port = 28088
subdomain_host = frp.example.com
# 这里是为了安全的考虑，加入一个身份认证的 token 配置，这里只要服务端和客户端配置一致即可
#privilege_token = token_string

dashboard_port = 7500
dashboard_user = admin
dashboard_pwd = admin

log_file = ./frps.log

log_level = info
log_max_days = 3

```

当前服务端的版本为0.13

## 注意
服务端和客户端的版本最好一致

