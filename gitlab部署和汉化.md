
## gitlab 登录凭证
地址： http://gitlab.example.com/users/sign_in  
用户： root  
密码： xxxxxx  


## 安装gitlab的依赖项

```bash
## --------------------------------------------------
# 安装gitlab的依赖项
yum install -y curl openssh-server openssh-clients postfix cronie policycoreutils-python

# 修改配置文件/etc/postfix/main.cf
sed -i '/inet_interfaces/{s/localhost/all/}' /etc/postfix/main.cf
grep ^inet_interfaces /etc/postfix/main.cf
chown -R postfix /var/lib/postfix/ /var/spool/postfix/
systemctl start postfix
systemctl enable postfix

```

## 安装gitlab

```bash
# 官方下载地址：https://packages.gitlab.com/gitlab/gitlab-ce/

# 下载-官方站点
wget https://packages.gitlab.com/gitlab/gitlab-ce/packages/el/7/gitlab-ce-12.2.0-ce.0.el7.x86_64.rpm/download.rpm

# 下载-清华站点
wget https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/el7/gitlab-ce-12.2.0-ce.0.el7.x86_64.rpm

# 安装
rpm -ivh download.rpm

# 配置
gitlab-ctl reconfigure

# 配置文件修改，查找 external_url 修改成自己的域名或ip
vim /etc/gitlab/gitlab.rb

```


## 汉化

```bash
# 2.1 安装git
yum install -y git

# 查看gitlab的版本
head -1 /opt/gitlab/version-manifest.txt

# 下载对应gitlab版本的汉化包
# git clone https://gitlab.com/xhang/gitlab.git

#  如果是要下载老版本的汉化包，需要加上老版本的分支
git clone https://gitlab.com/xhang/gitlab.git -b v12.2.0-zh

# 查看该汉化补丁的版本
cat gitlab/VERSION 

# 备份源文件
cp -r /opt/gitlab/embedded/service/gitlab-rails{,.ori}

# 拷贝汉化文件覆盖（此处有2个cannot overwrite报错，可以）
\cp -R gitlab/* /opt/gitlab/embedded/service/gitlab-rails/

# 启动gitlab
gitlab-ctl start

# 重新配置解决502问题
gitlab-ctl reconfigure

```

