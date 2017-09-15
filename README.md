# alpine-glibc

默认的alpine版本是非常精简的,不足5M!
如果需要构建oracle jdk环境,因为java底层是使用c++编写的,就必须需要c++的运行库glibc.
不然会报错 `java: not found` 的错误

# CONTAINS

这个镜像里除了官方的工具外,附加了wget,bash,ca-certificates.
[wget]busybox的wget对网络延迟很敏感,如果只在局域网内使用它,可以不用升级.
[bash]默认的shell是ash,ash是经过简化后的bash,但是ash并不兼容bash.鉴于很多java开源工具
使用的是bash,所以也安装
[ca-certificates]涉及到ssl需要认证就需要它,用来下载glibc包,它本身很小.

# SIZE

安装后的镜像大小是12+M,主要是bash的安装问价比较大7+M,新安装的软件为`ncurses-libs ncurses-terminfo ncurses-terminfo-base readline`
`ca-certificates` `bash` `glibc`
镜像中的软件:

    $apk info
    musl
    busybox
    alpine-baselayout
    alpine-keys
    libressl2.5-libcrypto
    libressl2.5-libssl
    zlib
    apk-tools
    scanelf
    musl-utils
    libc-utils
    ncurses-terminfo-base
    ncurses-terminfo
    ncurses-libs
    readline
    bash
    ca-certificates
    wget
    glibc

# HOW TO USE

在Dockerfile中就像这样使用

    FROM fansin/alpine:glibc
    RUN apk add --no-cache mysql-client
    ENTRYPOINT ["mysql"]
    
这个例子中镜像大小为42.8MB,相比官方实例增加了7+M.
