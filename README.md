# DST_Server

> 本教程只针对CentOS7.x系统，适用于32bit和64bit

## 搭建服务器

1. 更新系统包

   `sudo yum update	`

2. 升级curl版本

   + 安装repo

     `rpm -Uvh  http://www.city-fan.org/ftp/contrib/yum-repo/rhel6/x86_64/city-fan.org-release-2-1.rhel6.noarch.rpm`

   + 修改该repo的enable为1

     `vi /etc/yum.repos.d/city-fan.org.repo`

     ```
     [city-fan.org]
     
     name=city-fan.org repository for Red Hat Enterprise Linux (and clones) $releasever ($basearch)
     
     #baseurl=http://mirror.city-fan.org/ftp/contrib/yum-repo/rhel$releasever/$basearch
     
     mirrorlist=http://mirror.city-fan.org/ftp/contrib/yum-repo/mirrorlist-rhel$releasever
     
     enabled=1
     
     gpgcheck=1
     
     gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-city-fan.org
     ```

     + 更新 curl libcurl

     `yum install libcurl curl`

3. 开启SWAP

     > swap 简单的说就是将硬盘当内存用，用于解决内存容量不足的情况。

     + 查看当前系统是否设置了swap

       `free -m`

        swap栏都是 0 0 0 表示没有设置

     + 创建 Swap 文件

       ```
       #创建swap文件
       sudo dd if=/dev/zero of=/swapfile count=4096 bs=1M
       #查看swap文件是否创建完成
       ls / | grep swapfile
       ```

     + 激活 Swap 文件

       ```
       sudo chmod 600 /swapfile
       sudo mkswap /swapfile
       ```
       
     + 开启 Swap
     
       `sudo swapon /swapfile`
     
     + 设置系统启动时自动开启 Swap
     
       ```
       #编辑fsta文件
       sudo vim /etc/fstab
       
       #在最后添加
       /swapfile none swap sw 0 0
       
       #保存文件
       ```
     
4. 安装依赖库

     `sudo yum -y install glibc.i686 libstdc++.i686 libcurl4-gnutls-dev.i686 libcurl.i686 screen`

5. 安装SteamCMD

     一条一条复制到服务器执行，Linux只要不报错就是成功

     ```
     cd /home && mkdir steamcmd && cd steamcmd
     
     wget https://steamcdn-a.akamaihd.net/client/installer/steamcmd_linux.tar.gz
     
     tar -xvzf steamcmd_linux.tar.gz
     ```

6. 启动SteamCMD

     `./steamcmd.sh`

     

     ![image-20220213150011440](https://gitee.com/seasontong/image/raw/master/img/image-20220213150011440.png)

     当看到上图这种以 Steam> 开头的就代表进入了SteamCMD了，接着执行以下代码:

     ```
     login anonymous
     
     force_install_dir /home/dstserver
     
     app_update 343050 validate
     ```

     这一步是下载饥荒服务器到/home/dstserver目录下，执行完代码后服务器会自己下载，等待下载完后输入`quit`或`exit`退出SteamCMD，至此服务器已经下载好了，接下来就是配置服务器。

7. 创建软连接

     CentOS7.x下饥荒的服务器似乎需要的组件跟现在的组件产生了名字上的差错导致启动服务器会显示缺少关键的组件libcurl-gnutls.so.4，因此需要执行下面的命令来解决

     `ln -s /usr/lib/libcurl.so.4 /home/dstserver/bin/lib32/libcurl-gnutls.so.4`

8. 运行服务器

     ```
     cd /home/dstserver/bin
     
     echo "./dontstarve_dedicated_server_nullrenderer -console -persistent_storage_root /home/dstsave -conf_dir dst -cluster World1 -shard Master" > master_start.sh
     
     echo "./dontstarve_dedicated_server_nullrenderer -console -persistent_storage_root /home/dstsave -conf_dir dst -cluster World1 -shard Caves" > cave_start.sh
     
     chmod +x master_start.sh cave_start.sh
     ```

     输入`./master_start.sh`启动主世界服务器

     ![image-20220213150711721](https://gitee.com/seasontong/image/raw/master/img/image-20220213150711721.png)

     当看到上图标明服务器启动但还未配置所以显示未正常启动。

     但这是成功的标志，之后按下`Ctrl + C`正常关闭服务器。

     同样的输入` ./cave_start.sh` 启动洞穴服务器并正常关闭。

     ps. 经过上述服务器初次启动，在 `/home/dstsave/dst/World1/` 文件夹下就会自动生成默认的配置文件，这个配置文件就是饥荒服务器的配置文件了.接下来有两种方式，一种是自己修改配置，这种要求较高，另一种就是现在自己本地电脑上创建一个服务器，然后将配置文件复制到Linux服务器上。推荐使用第二种，简单，准确，这里也只描述第二种

## 配置服务器

1. 本地创建服务器

   新档 / 旧档均可

2. 本地上传配置

   + 本地找到存档路径（如图所示）

     ![image-20220213151652547](https://gitee.com/seasontong/image/raw/master/img/image-20220213151652547.png)

     这里面Cluster_*代表联机版创建的房间

     ![image-20220213152516232](https://gitee.com/seasontong/image/raw/master/img/image-20220213152516232.png)

   + 找到自己想上传的服务器
