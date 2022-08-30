# ubuntu18.04-Autoware1.14---GPU

## 电脑配置

### Ubuntu: 18.04 (内核5.4)
    http://qiushao.net/2020/03/01/Linux/ubuntu-upgrade-kernel/index.html
### ROS: melodic

    http://wiki.ros.org/cn/melodic/Installation/Ubuntu
#### rosdep 安装    

    https://blog.csdn.net/qq_45240585/article/details/108027141
若sudo rosdep init出错：Website may be down.
    
    sudo gedit /etc/hosts
    199.232.28.133 raw.githubusercontent.com 
    保存
    sudo rosdep init
    rosdep update
 若rosdep update出错：ERROR: error loading sources list: The read operation timed out
 参考：
 
    https://blog.csdn.net/DDonggggg/article/details/115798335
 使用方法1或者方法2。 方法1复杂但是绝对有效。     

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
    
### CUDA-10.0安装
下载链接
        https://developer.nvidia.com/cuda-10.1-download-archive-base?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1804&target_type=runfilelocal

        参考
        https://www.cnblogs.com/chua-n/p/13208414.html
        
Installation Instructions:

    sudo sh cuda_10.1.105_418.39_linux.run
    
    echo "export PATH=$PATH:/usr/local/cuda-10.0/bin/" >> ~/.bashrc
    echo "export CUDA_HOME=$CUDA_HOME:/usr/local/cuda-10.0" >> ~/.bashrc
    source ~/.bashrc
检查安装版本

    cat /usr/local/cuda/version.txt
### cudnn安装
下载链接

    https://developer.nvidia.com/rdp/cudnn-archive
    选择Download cuDNN v7.5.0 (Feb 25, 2019), for CUDA 10.1
    cuDNN Runtime Library for Ubuntu18.04 (Deb)
    cuDNN Developer Library for Ubuntu18.04 (Deb)
    cuDNN Code Samples and User Guide for Ubuntu18.04 (Deb)
    
    tar zxvf cudnn-10.0-linux-x64-v7.5.0.56.tgz
    cd cuda
    sudo mkdir /usr/local/cuda 
    sudo cp -r include /usr/local/cuda 
    sudo cp -r lib64 /usr/local/cuda 
    sudo chmod a+r /usr/local/cuda/include/cudnn.h
    sudo chmod a+r /usr/local/cuda/lib64/libcudnn*
    
    sudo dpkg -i libcudnn7_7.5.0.56-1+cuda10.0_amd64.deb 
    sudo dpkg -i libcudnn7-dev_7.5.0.56-1+cuda10.0_amd64.deb 
    sudo dpkg -i libcudnn7-doc_7.5.0.56-1+cuda10.0_amd64.deb 

    echo "export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib64" >> ~/.bashrc
    source ~/.bashrc
    
检查版本

    cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2

### opencv-3.4.0

    unzip opencv-3.4.0.zip
    cd opencv-3.4.0
依赖库

    sudo apt-get install build-essential libgtk2.0-dev libavcodec-dev libavformat-dev libjpeg.dev libtiff4.dev libswscale-dev libjasper-dev

    mkdir build && cd build
    cmake ..
    make -j8 // nproc是读取CPU的核心数量 
    sudo make install
    
    sudo /bin/bash -c 'echo "/usr/local/lib" > /etc/ld.so.conf.d/opencv.conf'
    //更新库
    sudo ldconfig

    sudo gedit /etc/bash.bashrc 
    PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/usr/local/lib/pkgconfig  
    export PKG_CONFIG_PATH
    
    sudo source /etc/bash.bashrc
    //激活配置然后更新database
    sudo updatedb 
    
### 安装Qt 5.14.2
    sudo apt-get update
    sudo apt-get install gcc g++
    
下载链接，选择run文件尾
    https://download.qt.io/archive/qt/5.14/5.14.2/
    
    chmod a+x ./qt-opensource-linux-x64-5.14.0.run
    ./qt-opensource-linux-x64-5.14.0.run
    
    


    




    
