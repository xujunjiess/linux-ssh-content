
## Centos 7 修改SSH端口号

### step1 、修改/etc/ssh/sshd_config
```bash
vi /etc/ssh/sshd_config

#Port 22         //这行去掉#号

Port 20000      //下面添加这一行

```

### step2 、修改SELinux

```bash
使用以下命令查看当前SElinux 允许的ssh端口：

semanage port -l | grep ssh

添加20000端口到 SELinux

semanage port -a -t ssh_port_t -p tcp 20000

然后确认一下是否添加进去

semanage port -l | grep ssh

如果成功会输出
ssh_port_t                    tcp    20000, 22

```

### step3 、重启ssh
```bash
systemctl restart sshd.service

```
### step4、连接ssh
```bash
ssh root@192.168.11.237 -p 20000
```

### step5、本地主机免密登陆远程主机
    #本地A主机
    #ssh-keygen -t rsa
    #ssh-copy-id root@192.168.11.251

    #远程251主机
    目录 /root/.ssh 会多一个 authorized_keys 文件

注意：针对个别发行版linux 禁用ssh root账号处理方案
ssh配置是否对root进行特殊设置，修改/etc/ssh/sshd_config文件中
PermitRootLogin without-password将 without-password改为yes；

### step6、mac 系统设置支持ssh 远程登录
    1. 进入系统偏好设置--共享--勾选远程登录
    2. 也可以设置限定的用户登录
    3. 获取mac系统root 权限  #sudo passwd root  #输入登录密码 #重新输入root密码

## 自定义命令 vi /root/.bashrc

alias 42="ssh root@testxiu.xiu.com"



### 更多linux 网络攻防资料请联系我

<img src="my.jpg" width="50%" height="50%"/>
