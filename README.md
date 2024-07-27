# MZwrt RAX3000m 云编译
此版本为精简版，自用版本，极限精简如果安装插件可能会出现缺少内核等情况

纯原版安装后剩余空间88.7M足够使用


# 插件兼容
因为精简许多模块导致很多第三方插件无法安装，

兼容所有官方插件

因为我常用openclash和adguardhome默认兼容此插件

# 兼容第三方插件

首先安装我编译的openwrt，在后台安装插件会出现类似以下错误就说明系统不兼容此插件

下面以：luci-app-docker插件为例：


依赖的软件包 kmod-fs-btrfs 在所有仓库都未提供。

依赖的软件包 kmod-dm 在所有仓库都未提供。

依赖的软件包 kmod-br-netfilter 在所有仓库都未提供。

依赖的软件包 kmod-ikconfig 在所有仓库都未提供。

依赖的软件包 kmod-nf-ipvs 在所有仓库都未提供。

依赖的软件包 kmod-veth 在所有仓库都未提供。

出现这个错误是因为编译时候未编译进去这几个软件包

云源仓库也未提供此软件包

第一种方法：修改config文件

查找所有报错的软件包在编译时候编译进去就可以了

例如 kmod-fs-btrfs 软件包

在config文件里面搜索 kmod-fs-btrfs 

    # CONFIG_PACKAGE_kmod-fs-btrfs is not set
    修改成
    CONFIG_PACKAGE_kmod-fs-btrfs=y

第二种方法：适合小白：

在编译的时候直接将 docker 编译进去


搜索luci-app-dockerman

    # CONFIG_PACKAGE_luci-app-dockerman is not set
    改成
    CONFIG_PACKAGE_luci-app-dockerman=y


第一种方法适合只做兼容不安装插件

第二种方法直接安装所需插件

# 2024年7月27日更新

优化编译参数

CONFIG_DEFAULT_TARGET_OPTIMIZATION="-Os -pipe -mcpu=cortex-a53" 

改为

CONFIG_DEFAULT_TARGET_OPTIMIZATION="-Os -O2 -pipe -fstack-protector -ftree-vectorize -flto -march=armv8-a -mcpu=cortex-a53"

增加一些安全性参数

添加针对armv8-a特性的一些优化和编译器-O2 优化不选择-O3的原因是路由器最主要的是稳定不是性能-O3不会提高多少性能但是可能会降低稳定性和不确定性

2.添加对passwall的支持这样就可以满足所有主流vpn协议的支持

添加一下四个模块

CONFIG_PACKAGE_libatomic=y

CONFIG_PACKAGE_iptables-mod-conntrack-extra=y

CONFIG_PACKAGE_libstdcpp=y

CONFIG_PACKAGE_iptables-mod-iprange=y


# 默认安装的插件
luci-app-adguardhome  （官方库未提供后台界面安装会不显示）

