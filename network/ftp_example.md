# ftp\_example

## Run in the foreground to keep the container running:

background=NO

## Allow anonymous FTP? \(Beware - allowed by default if you comment this out\).

anonymous\_enable=NO

## Uncomment this to allow local users to log in.

local\_enable=YES

### Enable virtual users

guest\_enable=NO

## Uncomment this to enable any form of FTP write command.

write\_enable=YES

local\_umask=022

passwd\_chroot\_enable=YES \#리눅스 격리기술로, FTP에서 사용자들의 활동범위를 제한하는데 사용한다.

## enable passive mode \#액티브는 서버가 클라이언트에게 파일을 보내고, 패시브는 클라이언트가 서버에서 직접 파일을 가져감.

pasv\_enable=YES  
pasv\_min\_port=30020 pasv\_max\_port=30021 \#ftp에 쓰이는게 30020~30029까지 포트임. pasv\_address=192.168.99.107 \#dumy 파일, 이따가 sed로 바꿔줄 것.

## You may specify an explicit list of local users to chroot\(\) to their home

## directory. If chroot\_local\_user is YES, then this list becomes a list of

## users to NOT chroot\(\).

chroot\_local\_user=YES

## Workaround chroot check.

## See [https://www.benscobie.com/fixing-500-oops-vsftpd-refusing-to-run-with-writable-root-inside-chroot/](https://www.benscobie.com/fixing-500-oops-vsftpd-refusing-to-run-with-writable-root-inside-chroot/)

## and [http://serverfault.com/questions/362619/why-is-the-chroot-local-user-of-vsftpd-insecure](http://serverfault.com/questions/362619/why-is-the-chroot-local-user-of-vsftpd-insecure)

allow\_writeable\_chroot=YES

### Hide ids from user

hide\_ids=YES

### Enable logging

xferlog\_enable=YES xferlog\_file=/var/log/vsftpd.log xferlog\_std\_format=No log\_ftp\_protocol=YES

### Enable active mode

port\_enable=YES connect\_from\_port\_20=YES ftp\_data\_port=20

### Disable seccomp filter sanboxing

seccomp\_sandbox=NO

## SSL SETTINGS

force\_local\_data\_ssl=YES force\_local\_logins\_ssl=YES rsa\_cert\_file=/etc/vsftpd/vsftpd.crt rsa\_private\_key\_file=/etc/vsftpd/vsftpd.key ssl\_enable=YES

require\_ssl\_reuse=YES ssl\_ciphers=HIGH

ssl\_tlsv1=NO ssl\_sslv2=NO ssl\_sslv3=NO

virtual\_use\_local\_privs=YES listen\_port=21

[참고](http://vsftpd.beasts.org/vsftpd_conf.html)

