# Docker setup tips

[Docker Forum](https://forums.docker.com/)

[Install Docker ToolBox on Windows](https://docs.docker.com/toolbox/toolbox_install_windows/)
[Get Docker Toolbox for Windows](https://download.docker.com/win/stable/DockerToolbox.exe)

[jerry.hub.docker.com](https://hub.docker.com/r/jerry4docker/ubuntu/)

[完整记录在 windows7 下使用 docker 的过程](https://www.jianshu.com/p/d809971b1fc1)

[docker~在centos容器中安装新程序](https://www.cnblogs.com/lori/p/6703174.html)

## 如何修改Windows上Docker的镜像源

[如何修改Windows上Docker的镜像源](https://blog.csdn.net/wangdandong/article/details/68958210)

    docker-machine create --engine-registry-mirror=https://xxxxxxxx.mirror.aliyuncs.com -d virtualbox default

    EXTRA_ARGS='
    --registry-mirror=https://hub.docker.com/r
    --label provider=virtualbox

    '
    CACERT=/var/lib/boot2docker/ca.pem
    DOCKER_HOST='-H tcp://0.0.0.0:2376'
    DOCKER_STORAGE=aufs
    DOCKER_TLS=auto
    SERVERKEY=/var/lib/boot2docker/server-key.pem
    SERVERCERT=/var/lib/boot2docker/server.pem

    export "NO_PROXY=192.168.99.100"

## Win7 修改为阿里云镜像加速
### 1. 在阿里云查看给个人分配的镜像源地址: [俊杰的阿里云](https://cr.console.aliyun.com/cn-hangzhou/mirrors)
### 2. docker-machine ssh default
    sudo mkdir -p /etc/docker
    sudo tee /etc/docker/daemon.json <<-'EOF'
    {
      "registry-mirrors": ["https://r19oqqqv.mirror.aliyuncs.com"]
    }
    EOF 

## Docker: How to enable/disable HTTP Proxy in Toolbox

### step 1. 
    docker-machine ssh default

### step 2.
    sudo vi /var/lib/boot2docker/profile

### step 3.
    # replace with your office's proxy environment
    export "HTTP_PROXY=http://PROXY:PORT"
    export "HTTPS_PROXY=http://PROXY:PORT"
    # you can add more no_proxy with your environment.
### Please make sure disable NO_PROXY line in the /var/lib/boot2docker/profile
    if not, the docker could not connect to docker hub as docker ip is 192.168.*.*
    #export "NO_PROXY=192.168.99.*,*.local,169.254/16,*.example.com,192.168.59.*"

### step 4.
    sudo /etc/init.d/docker restart

### step 5. 
    exit

## After modify profile, for any docker command, there is:
    error during connect: Get https://192.168.99.100:2376/v1.37/info: dial tcp 192.168.99.100:2376: connectex: No connection could be made because the target machine actively refused it.
### in Win10, Please follow
    https://blog.csdn.net/pangdongh/article/details/80203103
### in Win7, Please follow


    docker-machine create -d virtualbox --engine-env HTTP_PROXY=http://xxx.xxx.xxx.xxx:8000 --engine-env HTTPS_PROXY=http://xxx.xxx.xxx.xxx:8000 --engine-env NO_PROXY=192.168.99.100 default

## Error response from daemon: error parsing HTTP 404 response body
![Error response from daemon: error parsing HTTP 404 response body](pic/pic_1.png)

## Cannot perform an interactive login from a non TTY device
![Cannot perform an interactive login from a non TTY device](pic/pic_1.png)

[Docker Hub 仓库使用，及搭建 Docker Registry](https://segmentfault.com/a/1190000012662268)

[docker 学习笔记21：docker连接网络的设置](https://www.cnblogs.com/51kata/p/5268951.html)
    可以修改 /etc/default/docker 配置文件

    If you need Docker to use an HTTP proxy, it can also be specified here.
    export http_proxy="http://127.0.0.1:3128/"
    export http_proxy="http://代理地址:端口"
    docker run -it --rm ubuntu bash

### the input device is not a TTY.  If you are using mintty, try prefixing the command with 'winpty'

    switch from git bash to powershell

# ISSUE
    PS C:\Users\jzhan107> docker run jerry4docker/ubuntu4cplus
    Unable to find image 'jerry4docker/ubuntu4cplus:latest' locally
    D:\Program Files\Docker Toolbox\docker.exe: Error response from daemon: manifest for jerry4docker/ubuntu4cplus:latest not found.
    See 'D:\Program Files\Docker Toolbox\docker.exe run --help'.
    PS C:\Users\jzhan107> docker run -it --rm ubuntu4cplusplus /bin/bash
    Unable to find image 'ubuntu4cplusplus:latest' locally
    D:\Program Files\Docker Toolbox\docker.exe: Error response from daemon: pull access denied for ubuntu4cplusplus, repository does not exist or may require 'docker login'.
    See 'D:\Program Files\Docker Toolbox\docker.exe run --help'.
    PS C:\Users\jzhan107> docker login
    Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
    Username (jerry4docker): jerry4docker
    Password:
    Login Succeeded
    PS C:\Users\jzhan107> docker run -it --rm ubuntu4cplusplus /bin/bash
    Unable to find image 'ubuntu4cplusplus:latest' locally
    D:\Program Files\Docker Toolbox\docker.exe: Error response from daemon: pull access denied for ubuntu4cplusplus, repository does not exist or may require 'docker login'.
    See 'D:\Program Files\Docker Toolbox\docker.exe run --help'.
# Command 
    docker-machine ip
    docker info
    docker run -i -t ubuntu /bin/bash
    apt-get update && apt-get install vim
    docker ps -a
    docker stop XX
    docker rm `docker ps -a -q`

## the input device is not a TTY.  If you are using mintty, try prefixing the command with 'winpty'
    jzhan107@CV0030804N0 MINGW64 ~
    $ docker attach e9a932d44224
    the input device is not a TTY.  If you are using mintty, try prefixing the command with 'winpty'

    jzhan107@CV0030804N0 MINGW64 ~
    $ winpty docker attach e9a932d44224
    root@e9a932d44224:/#

##  Unable to locate package vim-gtk
    root@e9a932d44224:/# apt-get install vim-gtk
    Reading package lists... Done
    Building dependency tree
    Reading state information... Done
    E: Unable to locate package vim-gtk
### first, 
    apt-get update
### then,
    apt-get install vim-gtk

### apt-get update连接不上
    进入 vi /etc/apt/sources.list 
    将网址修改为如下之一即可。

    163源

    deb http://mirrors.163.com/debian/ jessie main non-free contrib 
    deb http://mirrors.163.com/debian/ jessie-updates main non-free contrib 
    deb http://mirrors.163.com/debian/ jessie-backports main non-free contrib 
    deb-src http://mirrors.163.com/debian/ jessie main non-free contrib 
    deb-src http://mirrors.163.com/debian/ jessie-updates main non-free contrib 
    deb-src http://mirrors.163.com/debian/ jessie-backports main non-free contrib 
    deb http://mirrors.163.com/debian-security/ jessie/updates main non-free contrib 
    deb-src http://mirrors.163.com/debian-security/ jessie/updates main non-free contrib

    中科大源

    deb http://mirrors.ustc.edu.cn/debian jessie main contrib non-free 
    deb-src http://mirrors.ustc.edu.cn/debian jessie main contrib non-free 
    deb http://mirrors.ustc.edu.cn/debian jessie-proposed-updates main contrib non-free 
    deb-src http://mirrors.ustc.edu.cn/debian jessie-proposed-updates main contrib non-free 
    deb http://mirrors.ustc.edu.cn/debian jessie-updates main contrib non-free 
    deb-src http://mirrors.ustc.edu.cn/debian jessie-updates main contrib non-free

### windows下装的docker,pull下来的ubuntu:last没有vi这种命令啊，怎么办？vim 
    用 “docker attach 容器id” 进入容器后apt install vim就可以了。
    不过国内访问archive.ubuntu.com特别的慢，所以建议在建立容器时 docker run命令跟一个参数 
    -v 宿主机地址:容器内地址 这样的方式建立一个挂载关系
    如：docker run -it -p 3306:3306 --name=ubuntu -v /User/tester/dockerVolumns:/tmp
    这样就建立了自己主机上/User/tester/dockerVolumns目录和容器内tmp目录的绑定关系，那么tmp里面有任何的改动都会在主机的绑定目录下有反应，这个时候可以把/etc/apt/source.list复制出来，到／tmp里面去再 在主机上打开编辑器编辑好镜像仓库，再复制回/etc/apt这个目录里面去，最后apt update一下就可以使用国内的镜像仓库了。
    然后按照刚才说的 apt install vim就可以安装了。
    Linux的经典文本编辑器vi的使用,	基本的文件内容查看命令

### win下Docker默认存储位置修改
[win下Docker默认存储位置修改](https://chybeta.github.io/2017/02/14/win%E4%B8%8BDocker%E9%BB%98%E8%AE%A4%E5%AD%98%E5%82%A8%E4%BD%8D%E7%BD%AE%E4%BF%AE%E6%94%B9/)


 docker commit -m="new Linux system with gcc g++ ping openssl make vim installed" 49efe8bedaf4 jerry4docker/jerryubuntu:first



 ### ISSUE: fatal error: Python.h: No such file or directory
    refer to https://stackoverflow.com/questions/21530577/fatal-error-python-h-no-such-file-or-directory

    should :
    apt-get install python-dev   # for python2.x installs
    apt-get install python3-dev  # for python3.x installs


### pip install Scrapy in ubuntu
    
    first need to
                |
                +---apt-get install python-dev


# docker images 存在哪里
[refer link](https://codeday.me/bug/20180702/187031.html)
    docker-machine ssh default
    sudo -i
    [docker在本地如何管理image?](https://segmentfault.com/a/1190000009730986)
    [Docker 基础 : 镜像](https://www.cnblogs.com/sparkdev/p/8901728.html)
    [docker 在本地如何管理 image（镜像）?](https://www.yangcs.net/posts/how-manage-image/)
    root@default:/mnt/sda1/var/lib/docker/aufs/diff# du -sh * |sort -u
    12.0K   1045a6c132a3c7c772d1249612a9eb3710309fc44981e4fa5eb0afa523d16b8a
    12.0K   1f90896d958f90bf56443c5f35e8029159d051a040c2024274bf06e9bb176779
    12.0K   9110403b0b36b8d46a9b1ab9551cc823f5af555b5c29a8f624094989c49cbfa8
    12.0K   93ce86261a50fbbdde18e62c77c2ea92b27ffe18ec9d39bcebc78890c82c2a9d
    12.0K   96791cf1bed61de162550b6d61fac66e482777bd74c8796ffc43cde92203bddc
    12.0K   9a890f602c6fef85808606e1c44a1d52bec724f1b1a2f0e5763fdf79c3f5e9d9
    12.0K   afaf538676bc1ccde353907cddccfbd3e6dd2d5253af008f09d6bf1cd14b42e9
    12.0K   c38a1c47b1db32e963dc8cd32d6e4adc5404c997e02eeecd65c215fcf0dc075f
    12.0K   c9363caa1ef5950676b622e4ffcaaf0dfe0c91c838eff54b9537cb99759ae7b6
    12.0K   d1d04edab127cb6560d4ce4d4f8729ee81dc550eb5507c6cfb70cb65ca1527a6
    12.0K   d8905e9e7cdf964cb81275fea480935b4398ab2f45f31c2da75c5346d0f3fc23
    16.0K   1d394326c880d6a64695bd8da310456746d3907240139331a1f0c6613d296c36
    2.2G    7dd6d3b8d9c689669feddf9d19d86760c6dbbb9cfaba300789ddfbbb8b69b8b6
    20.0K   1045a6c132a3c7c772d1249612a9eb3710309fc44981e4fa5eb0afa523d16b8a-init
    20.0K   195fa28d0fd61fdea9e485eab829a5cf7c0260490dca8ae2a5e510e970865215-init
    20.0K   1f90896d958f90bf56443c5f35e8029159d051a040c2024274bf06e9bb176779-init
    20.0K   21d6c1a2a84792725672bff3ee029c97bce5be55844482da0fba911f1fc5c6bd-init
    20.0K   2979024fa7445b60530c5287a23c8284feae6c93489a0269687f7d52cc6997a9
    20.0K   7dd6d3b8d9c689669feddf9d19d86760c6dbbb9cfaba300789ddfbbb8b69b8b6-init
    20.0K   7fe13d948129ddd99fe277668375e469c80d19799a743fec845b0ec224cd52f2
    20.0K   7fe13d948129ddd99fe277668375e469c80d19799a743fec845b0ec224cd52f2-init
    20.0K   879bd53104f11eb89ecc165d53b4c986ffa3718de66748a417032e1c1185deb9-init
    20.0K   9110403b0b36b8d46a9b1ab9551cc823f5af555b5c29a8f624094989c49cbfa8-init
    20.0K   93ce86261a50fbbdde18e62c77c2ea92b27ffe18ec9d39bcebc78890c82c2a9d-init
    20.0K   96791cf1bed61de162550b6d61fac66e482777bd74c8796ffc43cde92203bddc-init
    20.0K   9a890f602c6fef85808606e1c44a1d52bec724f1b1a2f0e5763fdf79c3f5e9d9-init
    20.0K   a9fe1c0b58de5979317d4a75a13ce4da9b36b0b663b5209d0c75028489c01142-init
    20.0K   afaf538676bc1ccde353907cddccfbd3e6dd2d5253af008f09d6bf1cd14b42e9-init
    20.0K   c38a1c47b1db32e963dc8cd32d6e4adc5404c997e02eeecd65c215fcf0dc075f-init
    20.0K   c9363caa1ef5950676b622e4ffcaaf0dfe0c91c838eff54b9537cb99759ae7b6-init
    20.0K   d8905e9e7cdf964cb81275fea480935b4398ab2f45f31c2da75c5346d0f3fc23-init
    20.0K   d958016803dc5454236a8a3da2a91d6f5beb6578b655ca8256e975f8b9131bba-init
    22.8M   21d6c1a2a84792725672bff3ee029c97bce5be55844482da0fba911f1fc5c6bd
    22.8M   a9fe1c0b58de5979317d4a75a13ce4da9b36b0b663b5209d0c75028489c01142
    22.8M   d958016803dc5454236a8a3da2a91d6f5beb6578b655ca8256e975f8b9131bba
    36.0K   195fa28d0fd61fdea9e485eab829a5cf7c0260490dca8ae2a5e510e970865215
    36.0K   d1d04edab127cb6560d4ce4d4f8729ee81dc550eb5507c6cfb70cb65ca1527a6-init
    44.0K   879bd53104f11eb89ecc165d53b4c986ffa3718de66748a417032e1c1185deb9
    687.3M  7c9c410d059ff592916886bc896a4d863d2da55089040f18bda5d98e58428dd5
    8.0K    f3f0204ada780e84f4af4c48a385abf91640dd6ffc84616658760214d6d1989a
    84.0K   c219ae9f6e266ab4acb6933baef03d7bb9ed5fce69a5f8d0667fcef356bbb3b3
    90.3M   0b6f00737d55323c93e97392d6b3a7cd5bc39fc2eda7d9638fadd20e70a729e4
    root@default:/mnt/sda1/var/lib/docker/aufs/diff#

# [docker中，如何将镜像保存为tar文件或者将镜像保存为文件，将tar文件导入到docker中](https://www.cnblogs.com/chuanzhang053/p/10084156.html)
