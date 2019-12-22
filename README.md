
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


### step7 下载和上传命令简述
    1. rsync 增量上传和下载

    rsync -azv /tmp root@192.168.11.66:/usr/local/my_content/
    rsync -azv root@192.168.11.66:/usr/local/my_content /tmp/

    2. scp 全量上传和下载
    scp -r /tmp root@192.168.11.66:/usr/local/my_content/
    scp -r root@192.168.11.66:/usr/local/my_content /tmp/

    3. curl 上传和下载
    curl -T tieba1.JPG -u 用户名:密码 ftp://www.baidu.com/img/ 
    curl -C -O http://www.baidu.com/tieba1.JPG
    
    大文件分块下载
    有时候下载的东西会比较大，这个时候我们可以分段下载

    # curl -r 0-100 -o tieba1_part1.JPG http://www.baidu.com/tieba1.JPG

    # curl -r 100-200 -o tieba1_part2.JPG http://www.baidu.com/tieba1.JPG

    # curl -r 200- -o tieba1_part3.JPG http://www.baidu.com/tieba1.JPG

    # cat tieba1_part* > tieba1.JPG
    



### 更多linux 网络攻防资料请联系我

<img src="my.jpg" width="50%" height="50%"/>
