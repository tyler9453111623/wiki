
```bash
cat >> .bashrc << EOF
# proxy setting
alias goproxy='export http_proxy=socks5://127.0.0.1:1080 https_proxy=socks5://127.0.0.1:1080'
alias disproxy='unset http_proxy https_proxy'

EOF

```