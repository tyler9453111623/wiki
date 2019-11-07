
```bash
cat >> .bashrc << EOF
# proxy setting
alias goproxy='export http_proxy=socks5://127.0.0.1:1080 https_proxy=socks5://127.0.0.1:1080'
alias disproxy='unset http_proxy https_proxy'

EOF

```

## ssh over socks5 proxy
refer：https://ieevee.com/tech/2017/10/19/ssh-over-socks5.html

ssh -o ProxyCommand='nc -x 127.0.0.1:1080 %h %p' user@awshost -p 22

