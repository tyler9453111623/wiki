


## 一、安装 OpenVPN

```bash
# 更新yum为最新版
sudo yum update -y

# 安装所需环境
sudo yum install epel-release -y

# 安装openvpn,以及wget网络下载工具
sudo yum install -y openvpn wget

# 下载easy-rsa秘钥生成工具
wget -O /tmp/easyrsa https://github.com/OpenVPN/easy-rsa-old/archive/2.3.3.tar.gz

# 解压easy-rsa秘钥生成工具
tar xfz /tmp/easyrsa

# 新建一个easy-rsa 文件夹
sudo mkdir /etc/openvpn/easy-rsa

# 将解压的文件都拷贝到自建的easy-rsa中
sudo cp -rf easy-rsa-old-2.3.3/easy-rsa/2.0/* /etc/openvpn/easy-rsa

# 给 ops 用户授权,如果您是root可以忽略本步
sudo chown ops:ops /etc/openvpn/easy-rsa/

```


## 二、配置 OpenVPN

```bash

# 拷贝server.conf(安装openvpn会带示例)示例到/etc/openvpn下,接下来要对其进行配置
sudo cp /usr/share/doc/openvpn-2.4.8/sample/sample-config-files/server.conf /etc/openvpn

# 使用vim(如果没有vim,可以用 yum install vim -y 来安装,也可以用vi来代替)配置server.conf文件
sudo vim /etc/openvpn/server.conf

------------------------- /etc/openvpn/server.conf -----------------------------------
local 172.31.156.123 # 内网ip
port 1194
proto tcp
dev tun
ca /etc/openvpn/certs/ca.crt
cert /etc/openvpn/certs/server.crt
key /etc/openvpn/certs/server.key  # This file should be kept secret
dh /etc/openvpn/certs/dh2048.pem
server 10.8.0.0 255.255.255.0
ifconfig-pool-persist ipp.txt
push "route 172.31.0.0 255.255.0.0"
push "dhcp-option DNS 100.100.2.136"
push "dhcp-option DNS 100.100.2.138"
client-to-client
keepalive 10 120
tls-auth /etc/openvpn/certs/ta.key 0 # This file is secret
cipher AES-256-CBC
comp-lzo
user nobody
group nobody
persist-key
persist-tun
status openvpn-status.log
verb 3
----------------------------- /etc/openvpn/server.conf -------------------------------


# 生成所需要的认证， 新建一个keys文件夹,放认证的文件
sudo mkdir /etc/openvpn/easy-rsa/keys

# 编辑vars文件（改自定义信息，也可以用默认）
sudo vim /etc/openvpn/easy-rsa/vars

# 切换到easy-rsa
cd /etc/openvpn/easy-rsa

# 清空认证
source ./vars
./clean-all

# 建立认证（一直回车就好）
./build-ca

# 设置建立的服务器的key（前10部回车，最后2步，输入y）
./build-key-server server

# 建立 dh协议
./build-dh

# 生成ta.key
openvpn --genkey --secret /etc/openvpn/easy-rsa/keys/ta.key
sudo cp /etc/openvpn/easy-rsa/keys/ta.key /etc/openvpn/certs/

# 将认证的文件拷贝到keys中
cd /etc/openvpn/easy-rsa/keys
sudo cp dh2048.pem ca.crt server.crt server.key /etc/openvpn/certs

# 切换到easy-rsa并建立客户端的key（前10部回车，最后2步，输入y）
cd /etc/openvpn/easy-rsa
./build-key client

# 拷贝openssl配置文件
cp /etc/openvpn/easy-rsa/openssl-1.0.0.cnf /etc/openvpn/easy-rsa/openssl.cnf

# 关闭防火墙
sudo service iptables stop

# 开启openvpn
sudo systemctl start openvpn@server.service

# 查看openvpn
sudo systemctl status openvpn@server.service

# 关闭vpn
sudo systemctl stop openvpn@server.service


```


## 三、配置客户端(CentOS 7 为例)

```bash

# 依赖安装
sudo yum -y install epel-release
sudo yum -y install pam-devel openssl-devel lzo-devel

# 编译安装openvpn
wget https://build.openvpn.net/downloads/releases/openvpn-2.4.8.tar.gz
tar xvf openvpn-2.4.8.tar.gz
cd openvpn-2.4.8 && ./configure && make && make install
sudo ldconfig

# 证书准备
sudo mkdir -pv /etc/openvpn/certs

# 从服务端拷贝证书文件
scp 服务端ip:/etc/openvpn/easy-rsa/keys/client.crt /etc/openvpn/certs
scp 服务端ip:/etc/openvpn/easy-rsa/keys/client.key /etc/openvpn/certs
scp 服务端ip:/etc/openvpn/easy-rsa/keys/ca.crt /etc/openvpn/certs
scp 服务端ip:/etc/openvpn/easy-rsa/keys/ta.key /etc/openvpn/certs

# 客户端配置文件内容（请根据实际情况配置服务端外网ip和证书文件路径）
cat /etc/openvpn/client.opvn
-------------------- client.opvn --------------------
client
dev tun
proto tcp
remote 外网ip 1194
resolv-retry infinite
nobind
persist-key
persist-tun
ca /etc/openvpn/certs/ca.crt
cert /etc/openvpn/certs/client.crt
key /etc/openvpn/certs/client.key
tls-auth /etc/openvpn/certs/ta.key 1
user nobody
group nobody
comp-lzo
verb 3
-------------------- client.opvn --------------------

# 启动
nohup sudo /usr/local/sbin/openvpn /etc/openvpn/client.opvn &

```
