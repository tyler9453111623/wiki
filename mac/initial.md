
## ssh连接配置
cat >> /etc/ssh/ssh_config << EOF
Host *
    #SendEnv LANG LC_*
    ServerAliveInterval 50
EOF

## 安装brew

## 办公软件
manico
iterm2
copyless
