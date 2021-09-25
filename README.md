# 系统重置后的初步操作

### 添加密钥 关闭密码登录 开启BBR


- 先检查是不是root账户

- 如果不是roor账户，执行 sudo -i

- 更新及安装组件

  apt update && apt upgrade

  apt update && apt install curl wget -y


- 创建 SSH 认证文件并打开文本编辑器

  mkdir -p ~/.ssh && nano ~/.ssh/authorized_keys

- 添加完密钥后，按 Ctrl+X 退出 nano编辑器

- 为了能正常登陆，需要给文件设置相应的权限：
  
  chmod 700 ~/.ssh && chmod 600 ~/.ssh/authorized_keys

- 注意：
  - 用密钥登录VPS，看是否成功。

  - 使用密钥能正常登录后，再进行下面的操作。
    
    不然密钥不能正确登录，再关闭密码登录，就彻底被拒之门外了


- 禁用密码登录

  sed -i '/PasswordAuthentication /c\PasswordAuthentication no' /etc/ssh/sshd_config

- 重启 sshd 服务，使配置生效
  
  service sshd restart



### 开启系统自带BBR

- 查看是否开启BBR

  lsmod | grep bbr

- 开启BBR

  echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
  
  echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
  
  sysctl -p

- 查看BBR状态

  sysctl net.ipv4.tcp_available_congestion_control
  
  lsmod | grep bbr
