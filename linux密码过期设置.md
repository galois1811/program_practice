Linux下对于新添加的用户，用户密码过期时间是从/etc/login.defs中PASS_MAX_DAYS提取的，普通系统默认就是99999，而有些安全操作系统是90。更改此处，只是让新建的用户默认密码过期时间变化，已有用户密码过期时间仍然不变。  
```bash
[root@linuxidc ~]# chage --help
Usage: chage [options] user
  
Options:
  -d, --lastday LAST_DAY        set last password change to LAST_DAY  
  -E, --expiredate EXPIRE_DATE  set account expiration date to EXPIRE_DATE  
  -h, --help                    display this help message and exit  
  -I, --inactive INACTIVE       set password inactive after expiration  
                                to INACTIVE  
  -l, --list                    show account aging information  
  -m, --mindays MIN_DAYS        set minimum number of days before password  
                                change to MIN_DAYS  
  -M, --maxdays MAX_DAYS        set maximim number of days before password  
                                change to MAX_DAYS  
  -W, --warndays WARN_DAYS      set expiration warning days to WARN_DAYS   
```
chage：密码失效是通过此命令来管理的。  
  
　　参数意思：  
　　-m 密码可更改的最小天数。为零时代表任何时候都可以更改密码。  
　　-M 密码保持有效的最大天数。www.linuxidc.com  
　　-W 用户密码到期前，提前收到警告信息的天数。  
　　-E 帐号到期的日期。过了这天，此帐号将不可用。  
　　-d 上一次更改的日期  
　　-i 停滞时期。如果一个密码已过期这些天，那么此帐号将不可用。  
　　-l 例出当前的设置。由非特权用户来确定他们的密码或帐号何时过期。  
```bash
[root@linuxidc ~]# chage -l root
Last password change                                    : Oct 19, 2010  
Password expires                                        : never  
Password inactive                                       : never  
Account expires                                         : never  
Minimum number of days between password change          : 0  
Maximum number of days between password change          : 99999  
Number of days of warning before password expires       : 7  
```
更改用: chage -M 90 root  
```bash  
[root@linuxidc ~]#chage -M 90 root

[root@linuxidc ~]#chage -l root
```
如果以后添加一个用户,那么默认的时间还是没改的,还必须得去/etc/login.defs修改PASS_MAX_DAYS 的默认值.那么如果我直接修改全局/etc/login.defs所在的用户会跟着改变吗? 据我的测试是不会改变的,除非重启后,但我们的服务器不是你想重启的就可以重启的!如果管理严格的地方,重启还得经过很多程序步骤.改完全局时,没有更改的用户,想要让他也同样具备此功能.就得一个个的执行!  
  
你也可以直接用vim 编辑器去编辑PASS_MAX_DAYS 99999  
  
也可以用其它的工具  
```bash
[root@linuxidc ~]#sed -i.bak -e 's/^\(PASS_MAX_DAYS\).*/\1   90/' /etc/login.defs
```
查看一下是否  
```bash
[root@linuxidc ~]#cat /etc/login.defs |grep "PASS_M";  
```
强制用户登陆时修改口令  
```bash
[root@linuxidc ~]#chage -d 0 username（linux）  
[root@linuxidc ~]#passwd -f username(solaris)
```
强制用户下次登陆时修改密码，并且设置密码最低有效期0和最高有限期90，提前15天发警报提示  
```bash
[root@linuxidc ~]#chage -d 0 -m 0 -M 90 -W 15 root（linux）
[root@linuxidc ~]#passwd -f -n 0 -x 90 -w 15 root（solaris）
```
查看某个用户的密码设置情况  
```bash
[root@linuxidc ~]#chage -l username
```
修改密码配置文件  
```bash
[root@linuxidc ~]#vi /etc/login.defs
```
