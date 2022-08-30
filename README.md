# ubuntu18.04-Autoware1.14---GPU

## 电脑配置

### Ubuntu: 18.04 (内核5.4)
    http://qiushao.net/2020/03/01/Linux/ubuntu-upgrade-kernel/index.html
### ROS: melodic
    http://wiki.ros.org/cn/melodic/Installation/Ubuntu
### NVIDIA： 3050Ti

## 软件配置
   参考
   https://blog.csdn.net/weixin_42343975/article/details/124754154

### Nvidia 驱动安装
软件与更新——附加驱动中选择510

    sudo apt-get update
    sudo apt-get upgrade

检查显卡驱动更新是否成功：

    nvidia-smi

### cmake安装

    tar zxvf cmake-3.12.2-Linux-x86_64
#### 注意路径（用户名替换）

    sudo ln -sf /home/jzc/cmake-3.12.2-Linux-x86_64/bin/* /usr/bin/  
