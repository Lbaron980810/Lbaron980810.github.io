---
title: Ubuntu新系统配置步骤
category: ProjectExperience
---
1. 修改中文源镜像：软件和更新中修改，可以自动搜索，一般选择清华的，勾上“源码”

2. 中文输入法：sudo apt install fcitx，搜狗输入法官网，注意对应ubuntu版本！！

3. 安装electron-ssr和chrome（先安装vpn并设置http_proxy以解决后面ros下载更新的问题）
   `export http_proxy=http://127.0.0.1:12333`
   `export https_proxy=http://127.0.0.1:12333`

4. ROS：
   4.1 `sudo rosdep init`  "Website may be down"的解决办法:

   ```bash
   sudo vi /etc/hosts
   #在末尾添加：151.101.84.133 raw.githubusercontent.com （IP地址可能不一样）
   ```

   4.2 `sudo rosdep init` "ERROR: cannot download default sources list from:
   https://raw.githubusercontent.com/ros/rosdistro/master/rosdep/sources.list.d/20-default.list
   Website may be down."的解决办法：

   ```bash
   sudo chmod 777 /etc
   # 手动新建/etc/ros/rosdistro/master/rosdep/sources.list.d/20-default.list
   # 点开上面链接，把内容复制到20-default.list中，直接rosdep update即可
   ```

5. 配置git相关

   ```bash
   git config --global user.name "lbaron980810"
   git config --global user.email "lbaron980810@163.com"
   git config --global http.proxy "socks5://127.0.0.1:1080"
   git config --global https.proxy "socks5://127.0.0.1:1080"
   ```

6. 配置terminator, doom-emacs（周末正好学习一下）,mendeley, vscode, deepin, 微信, QQ, 百度网盘
   terminator装好了，但快捷键还没修改
   mendeley和向日葵也装好了
   neovim 配置: sudo add-apt-repository ppa:neovim-ppa/stable
   sudo apt-get update
   suaptins -y neovim        
   安装vim plug和NERDTree之类的
   
7. 配置git 仓库所需要的东西
   依赖：
   `pip install future`
   `sudo apt-get install python3-pip python-pip python-lxml`
   `sudo apt-get install libapacke.dev`
   `sudo apt install libgeographic-dev ros-melodic-geographic-msgs ros-melodio-joy ros-melodic-octomap-ros ros-melodic-ompl*`
   `suaptins python3-catkin-pkg python3-rosdistro python3-rospkg`
   `sudo apt-get install python3-catkin-tools python3-rosinstall-generator -y`
   `pip3 install em`
   `pip3 install --user pyros-genmsg packaging toml numpy`
   
   > ompl使用时的坑：需要用
   > `sudo ln -s /opt/ros/melodic/include/ompl-1.4/ompl /opt/ros/melodic/include/ompl` 建立一个软连接，否则会显示找不到文件 
   
   
   
   
   
8. 转移一些必须的个人文件

