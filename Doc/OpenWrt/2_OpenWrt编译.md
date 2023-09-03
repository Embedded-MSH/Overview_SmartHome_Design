# OpenWrt 编译

1. [OpenWrt 编译](#openwrt-编译)
   1. [选型](#选型)
   2. [编译](#编译)
   3. [插件推荐](#插件推荐)

## 选型

这里推荐使用 [LEAN LEDE](https://github.com/coolsnowwolf/lede) 仓库编译，编译稳定性很好并且很多针对国内网络环境的优化适配都很不错，并且 README 写的很清晰如何进行编译。

## 编译

1. 首选 ubuntu20.04，使用 docker 也行: `docker run -dit --name openwrt  -v /home/layton:/host   ubuntu:20.04 bash`；如果你想直接创建一个带用户的镜像，可以使用如下 dockerfile，默认创建一个 layton 账户，root 密码 123456：

```docker
FROM ubuntu:20.04
MAINTAINER      Fisher "layton@gmail.com"
RUN apt-get update && apt-get install -y sudo
RUN     /bin/echo 'root:123456' |chpasswd
RUN useradd --create-home --no-log-init --shell /bin/bash layton \
    && adduser layton sudo \
    && echo "layton:1" | chpasswd
RUN     /bin/echo -e "LANG=\"en_US.UTF-8\"" >/etc/default/local
WORKDIR /home/layton
USER layton
```

2. 注意使用非 root 默认 bash，我用 zsh 会遇到一个很奇怪的编译问题；

3. 下载环境依赖库：

```text
> sudo apt update -y

> sudo apt full-upgrade -y

> sudo apt install -y ack antlr3 asciidoc autoconf automake autopoint binutils bison build-essential \
bzip2 ccache cmake cpio curl device-tree-compiler fastjar flex gawk gettext gcc-multilib g++-multilib \
git gperf haveged help2man intltool libc6-dev-i386 libelf-dev libglib2.0-dev libgmp3-dev libltdl-dev \
libmpc-dev libmpfr-dev libncurses5-dev libncursesw5-dev libreadline-dev libssl-dev libtool lrzsz \
mkisofs msmtp nano ninja-build p7zip p7zip-full patch pkgconf python2.7 python3 python3-pyelftools \
libpython3-dev qemu-utils rsync scons squashfs-tools subversion swig texinfo uglifyjs upx-ucl unzip \
vim wget xmlto xxd zlib1g-dev python3-setuptools
```

4. 下载相关源码，使用 [kenzok8](https://github.com/kenzok8/kenzok8) 扩展插件列表：

```text
> git clone https://github.com/coolsnowwolf/lede
> sed -i '$a src-git kenzo https://github.com/kenzok8/openwrt-packages' feeds.conf.default
> sed -i '$a src-git small https://github.com/kenzok8/small' feeds.conf.default
> cd lede
> ./scripts/feeds update -a
> ./scripts/feeds install -a
> make menuconfig
```

5. 进行编译：

```text
> make download -j8
> make V=s -j8 # 虽说推荐 j1 编译 但是是在太慢了😓
```

6. 二次编译：

```text
> cd lede
> git pull
> ./scripts/feeds update -a
> ./scripts/feeds install -a
> make defconfig
> make download -j8
> make V=s -j8
```

## 插件推荐

1. OpenClash：梯子；
2. AdGuard Home：去广告;
3. SmartDNS：优化 DNS。

讲真我常用就哥仨，安装其他七七八八总是容易出问题（技术不到家😂）。
