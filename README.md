
## 1. Centos 7 修改SSH端口号

step1 、修改/etc/ssh/sshd_config

vi /etc/ssh/sshd_config

#Port 22         //这行去掉#号

Port 20000      //下面添加这一行


step2 、修改SELinux
使用以下命令查看当前SElinux 允许的ssh端口：

semanage port -l | grep ssh

添加20000端口到 SELinux

semanage port -a -t ssh_port_t -p tcp 20000

然后确认一下是否添加进去

semanage port -l | grep ssh

如果成功会输出
ssh_port_t                    tcp    20000, 22


step3 、重启ssh

systemctl restart sshd.service




