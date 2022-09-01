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
    
### CUDA Toolkit 11.0 Download
下载链接
        https://developer.nvidia.com/cuda-11.0-download-archive?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1804&target_type=runfilelocal

        参考
        https://www.cnblogs.com/chua-n/p/13208414.html
        
Installation Instructions:

    sudo sudo sh cuda_11.0.2_450.51.05_linux.run
    
    
    echo "export PATH=$PATH:/usr/local/cuda-11.0/bin/" >> ~/.bashrc
    echo "export CUDA_HOME=$CUDA_HOME:/usr/local/cuda-11.0" >> ~/.bashrc
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

### opencv-3.4.5

    unzip opencv-3.4.5.zip
    cd opencv-3.4.5
依赖库

    sudo apt-get install build-essential libgtk2.0-dev libavcodec-dev libavformat-dev libjpeg.dev libtiff4.dev libswscale-dev libjasper-dev

    mkdir build && cd build
    cmake ..
    如果下载IPPICV: Download: ippicv_2020_lnx_intel64_20191018_general.tgz卡住
    在文件目录 /opencv-3.4.16/3rdparty/ippicv/ippicv.cmake 中修改
    "https://raw.githubusercontent.com/opencv/opencv_3rdparty/${IPPICV_COMMIT}/ippicv/"
    为 "https://ghproxy.com/https://raw.githubusercontent.com/opencv/opencv_3rdparty/${IPPICV_COMMIT}/ippicv/"
    
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
    
    

### Autoware1.14源码安装
    
#### 环境依赖

    sudo apt update

    sudo apt install -y python-catkin-pkg python-rosdep ros-$ROS_DISTRO-catkin

    sudo apt install -y python3-pip python3-colcon-common-extensions python3-setuptools python3-vcstool
    
    pip3 install -U setuptools
    
#### 建立工作空间

    mkdir -p autoware.ai/src
    cd autoware.ai

#### 下载Autoware

    wget -O autoware.ai.repos "https://gitlab.com/autowarefoundation/autoware.ai/autoware/raw/1.14.0/autoware.ai.repos?inline=false"
因网络问题可能失败，可以下载好源文件

    vcs import src < autoware.ai.repos 

编译：

    复制autoware_1.14/src下的所有文件到autoware.ai/src下
    
    sudo apt install python-pip
    sudo pip install rosdepc
    sudo rosdepc init
    rosdepc update
    rosdepc install -y --from-paths src --ignore-src --rosdistro $ROS_DISTRO   

    AUTOWARE_COMPILE_WITH_CUDA=1 colcon build --cmake-args -DCMAKE_BUILD_TYPE=Release
    
    
###编译出现的问题：
找不到math_functions.hpp

    sudo ln -s /usr/local/cuda/include/crt/math_functions.hpp /usr/local/cuda/include/math_functions.hpp

package failed: ndt_gpu
修改.make文件中的版本需求如10.0改11.7

config.make 问题如下解决
例如：

    sudo apt-get install ros-melodic-jsk-rviz-plugins
    sudo apt-get install ros-melodic-nmea-msgs

出现sterr:kitt-player (opencv必须是3.4.5)

    sudo apt-get install libopenblas-dev

安装完全opencv后 配置sudo ldconfig 出现问题 /sbin/ldconfig.real: /usr/local/cuda-11.7/targets/x86_64-linux/lib/libcudnn_ops_infer.so.8 is not a symbolic link
修改参考

    https://blog.csdn.net/qq_39096302/article/details/116494406 
    
    或者
    
    sudo ln -sf /usr/local/cuda-11.7/targets/x86_64-linux/lib/libcudnn_adv_train.so.8.2.0 /usr/local/cuda-11.7/targets/x86_64-linux/lib/libcudnn_adv_train.so.8
    sudo ln -sf /usr/local/cuda-11.7/targets/x86_64-linux/lib/libcudnn_ops_infer.so.8.2.0 /usr/local/cuda-11.7/targets/x86_64-linux/lib/libcudnn_ops_infer.so.8
    sudo ln -sf /usr/local/cuda-11.7/targets/x86_64-linux/lib/libcudnn_cnn_train.so.8.2.0 /usr/local/cuda-11.7/targets/x86_64-linux/lib/libcudnn_cnn_train.so.8
    sudo ln -sf /usr/local/cuda-11.7/targets/x86_64-linux/lib/libcudnn_adv_infer.so.8.2.0 /usr/local/cuda-11.7/targets/x86_64-linux/lib/libcudnn_adv_infer.so.8
    sudo ln -sf /usr/local/cuda-11.7/targets/x86_64-linux/lib/libcudnn_ops_train.so.8.2.0 /usr/local/cuda-11.7/targets/x86_64-linux/lib/libcudnn_ops_train.so.8
    sudo ln -sf /usr/local/cuda-11.7/targets/x86_64-linux/lib/libcudnn_cnn_infer.so.8.2.0 /usr/local/cuda-11.7/targets/x86_64-linux/lib/libcudnn_cnn_infer.so.8
    sudo ln -sf /usr/local/cuda-11.7/targets/x86_64-linux/lib/libcudnn.so.8.2.0 /usr/local/cuda-11.7/targets/x86_64-linux/lib/libcudnn.so.8
    
编译期间问题参考

    https://blog.csdn.net/CCCrunner/article/details/108896363
    
运行：
    //必须执行
    source install/setup.bash 
    
    roscd runtime_manager
    roslaunch runtime_manager runtime_manager.launch
    出现闪退问题：
    下载：
    
    https://pypi.org/project/wxPython/#files
    
    确保pip是更新的，否则后面安装步骤可能出错
    
    python3 -m pip install --upgrade pip setuptools wheel
    
    pip install wxPython-4.0.7.post2.tar.gz 
    再修改源码/home/$usrname/autoware14.ai/src/autoware/utilities/runtime_manager/scripts/runtime_manager_dialog.py
    增加import wx.adv （第17行）
    所有wx.HyperlinkCtrl替换为wx.adv.HyperlinkCtrl (5处)
    所有wx.EVT_HYPERLINK替换为wx.adv.EVT_HYPERLINK (5处)
    
    重新编译
    
    AUTOWARE_COMPILE_WITH_CUDA=1 colcon build --cmake-args -DCMAKE_BUILD_TYPE=Release
    
    


   

    
