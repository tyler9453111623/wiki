
## frp server
47.244.78.102


## 参考
https://www.cnblogs.com/zhanggaoxing/p/9221705.html


配置文件

```bash
[root@frp01:~/frp_0.13.0_linux_amd64]# cat frpc.ini
[common]
bind_port = 7000
vhost_http_port = 28088
subdomain_host = frp.godeng.com
# 这里是为了安全的考虑，加入一个身份认证的 token 配置，这里只要服务端和客户端配置一致即可
#privilege_token = tony666

dashboard_port = 7500
dashboard_user = admin
dashboard_pwd = admin

log_file = ./frps.log

log_level = info
log_max_days = 3

```


