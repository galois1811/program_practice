本文主要介绍在需要使用代理的网络环境下，通过代理配置，来实现git的版本升级、解决常见问题、以及为git配置代理等。以Ubuntu16.04为例进行说明。  

- [Git版本升级](requre "git版本升级")

### git版本升级
Debian/Ubuntu
For the latest stable version for your release of Debian/Ubuntu
``` bash
# apt-get install git
```
For Ubuntu, this PPA provides the latest stable upstream Git version
``` bash
# add-apt-repository ppa:git-core/ppa # apt update; apt install git
```
由于Ubuntu 16.04只能更新到git的2.7版本，需要增加PPA源，如上描述。执行如上脚本添加PPA源后，在 /etc/apt/sources.list.d/git-core-ubuntu-ppa-xenial.list 中会增加如下两项内容，第一项为安装包的源配置，第二项为源码包的源配置：  
``` bash
deb http://ppa.launchpad.net/git-core/ppa/ubuntu xenial main
deb-src http://ppa.launchpad.net/git-core/ppa/ubuntu xenial main
```

**有可能源码包的源配置项被注释掉了，务必手动加上，否则在后面解决GNUTLS的问题过程中，基于源码包重新编译git时，会出现无法拉去到最新的git版本对应的源码包。**
