About FTP on linux for manjaro
# 安装vsftpd
```
sudo pacman -S vsftpd //安装
```
```
/srv/ftp/        //一般情况下这是默认匿名用户ftp的目录
~/username/     //这是本地用户的ftp目录
```

# 配置匿名用户upload和download
```
cd /srv/ftp/
sudo mkdir pub  #新建立pub目录做为 匿名用户的上传目录
sudo chown ftp:ftp pub #为pub目录改其所有者组为 ftp
sudo chmod -R 777 pub #为pub目录设置权限为全部可读可写可执行
```
# 配置vsftpd配置文件
```
sudo vim /etc/vsftpd.conf
```
```
anonymous_enable=YES		//设置匿名用户可用
local_enable=YES		//设置本地用户可用
write_enable=YES		//以启用任何形式的 FTP 写入命令
local_umask=000			//修改本地用户的掩码（权限） e.g.Defaul是022文件权限就是077
anon_umask=000			//修改匿名用户的掩码（权限） 同上
anon_upload_enable=YES		//修改匿名用户可上传
anon_mkdir_write_enable=YES	//修改匿名用户可创建文件夹
dirmessage_enable=YES
xferlog_enable=YES		//激活上传/下载记录
listen=YES
pam_service_name=vsftpd		//用于系统包的 vsftpd
allow_writeable_chroot=YES	//允许写入chroot
anon_other_write_enable=YES	//允许匿名用户或者其他用户写入
anon_root=~/chihlin/ftp	//修改匿名用户默认ftp目录
```
完成后保存退出```:wq```
重启vsftpd
```
systemctl restart vsftpd
```
# 使用ftp
查看本地ip地址
```
ifconfig
```
在Terminal输入
```
ftp 0.0.0.0     //查询到的ip地址
```
## 本地用户登录
```
Connected to 0.0.0.0
220 (vsFTPd 3.0.3)
Name (0.0.0.0:username):      //login system username
331 Please specify the password.
Password:  #login system password
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
```
## 匿名用户登录
```
Connected to 0.0.0.0
220 (vsFTPd 3.0.3)
Name (0.0.0.0:username):      //anonymous
331 Please specify the password.
Password:                     //just press enter key
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
```

### 查看远程服务器目录```ls```
### 切换目录```cd```
### 下载远程文件到本地```get 1.txt```
### 上传本地文件到远程服务器```pub 0.txt ```
### 新建目录```mkdir test```
