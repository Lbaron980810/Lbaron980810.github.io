---
title: PX4学习
categories: ProjectExperience
---
# 初识PX4：

## 运行Gazebo PX4仿真

```bash
cd PX4/Firmware
make px4_sitl_default gazebo_iris
make px4_sitl_default gazebo    %% 二者效果相同，非ROS下的Gazebo仿真
```

用make px4_sitl_default gazebo + QGC 可以自动连接上

make出现问题时：

```bash
make clean
sudo apt-get update
sudo apt-get upgrade
make px4_sitl_default gazebo
```

## ROS 下的Gazebo仿真

step1: 编译

```bash
cd Firmware
make px4_sitl_default gazebo 
```

step2: 添加环境变量：为了让系统知道有这个功能包存在

```bash
source Tools/setup_gazebo.bash $(pwd) $(pwd)/build/px4_sitl_default 
export ROS_PACKAGE_PATH=$ROS_PACKAGE_PATH:$(pwd) 
export ROS_PACKAGE_PATH=$ROS_PACKAGE_PATH:$(pwd)/Tools/sitl_gazebo  
```

或者打开终端输入

```bash
gedit .bashrc
```

```bash
source /home/liuboyuu/PX4/PX4-Autopilot/Tools/setup_gazebo.bash /home/liuboyuu/PX4/PX4-Autopilot /home/liuboyuu/PX4/PX4-Autopilot/build/px4_sitl_default  

export  ROS_PACKAGE_PATH=$ROS_PACKAGE_PATH:/home/liuboyuu/PX4/PX4-Autopilot

export  ROS_PACKAGE_PATH=$ROS_PACKAGE_PATH:/home/liuboyuu/PX4/PX4-Autopilot/Tools/sitl_gazebo
```

step1 & step2 只需要做一次，之后每次运行只需要做step3)

step3: 运行Gazebo仿真

```bash
roslaunch px4 posix_sitl.launch
```

这时可以打开QGC地面站，地面站会默认连接这台飞机，可以尝试利用地面站发送起飞指令测试。

运行Mavros

```bash
roslaunch mavros px4.launch fcu_url:="udp://:14540@127.0.0.1:14557"
```

这里在第一次运行的时候出现了错误，应该是Mavros没装好报的错，于是用下面命令进行重装,注意路径要改称自己电脑的。

该命令会消耗很长时间，开始装了我就去上课了

```bash
sudo /opt/ros/melodic/lib/mavros/install_geographiclib_datasets.sh
```



## PX4通讯

PX4是使用UDP来进行这些消息通讯的。单个飞机仿真时，默认的UDP设置如下：

- UDP端口  **14540**  用于offboard模式的通讯。is used for  communication with offboard APIs. Offboard APIs are expected to listen  for connections on this port.
- UDP端口  **14550**  用地面站的通讯。
- TCP端口 **4560**  用于与仿真器的通讯。 PX4 listens to this port,  and simulators are expected to initiate the communication by  broadcasting data to this port



sitl_gazebo git代码库介绍

- src文件夹。这里是核心代码，即plugin（插件）代码。比如gps、imu、电机的插件，简单说如何模拟产生gps信号，就是这里的代码负责的。Gazebo自身也会提供一些默认插件，或者第三方也会提供插件，比如你要新增一个传感器，就可以找到相应的插件。
- models文件夹。存放各种Gazebo模型文件。
- worlds文件夹。存放Gazebo的世界环境文件。



### PX4编译选项与模块功能说明

可以看到Firmware文件夹下又有几个小文件夹，现对其中部分功能进行说明

#### build

编译目标目录，其中包括了程序源代码编译之后所生成的编译选项、中间文件、目标文件等，通常也是**按不同的编译选项分为多个不同的目录**，例如当我们执行不同的编译选项时，就会出现多个编译目标目录

build/

├── nuttx_px4fmu-v2_default

├── nuttx_px4fmu-v3_default

├── posix_sitl_default

└── posix_sitl_test

用nuttx_px4fmu-v2_default和nuttx_px4fmu-v3_default编译选项编译之后生成的文件即运行在STM32F系列的FMU中，并采用Nuttx操作系统运行整个飞控程序；

用posix_sitl_default和posix_sitl_test即运行在支持Posix标准的平台上，如运行Linux内核的Arm平台，或PC机



#### cmake

cmake目录中存放了整个飞控程序的所有编译选项

在最开始学习的时候，在网上找教程并随机运行了一些命令，但mavlink始终无法与px4连接上，在看到知乎上这篇文章之后认为可能是有一些文件没有make（因为缺少了很多文件夹）



#### mavlink

目前的mavlink版本为2.0，使用时只需要包含其头文件即可，其目录为：mavlink/include/mavlink/v2.0/common



#### msg

msg文件夹中存放的就是uORB所需要的所有Message，其实也就是uORB在通讯时需要定义的数据格式，以.msg结尾，其内容为特定的uORB数据类型

msg文件夹下包括templates文件夹&tools文件夹，以及一堆.msg文件

 message：在msg目录直属的.msg文件，都是uORB的message文件，即通讯协议内容

 templates 中存放的是msg.cpp.template、msg.h.template、uORBTopics.cpp.template 这三个文件，用于将.msg文件编译成.h头文件和.cpp源文件

tools 中存放的是将.msg生成.h和.cpp源代码的工具文件，大多是采用Python编写的



#### platforms

platforms即为运行平台，px4架构支持多种运行平台，例如：运行在STM32F4系列上、运行在Arm系列上，运行在Posix标准架构上等等。在不同的运行平台上，编译源代码的内容与链接源代码均不尽相同，所以就需要在不同的编译选项下，编译不同的平台源代码，platforms中存放了不同平台下的源代码内容



#### ROMFS

ROMFS中存放了不同编译选项中的ROM文件系统，用简单的语言来说就是存放了飞控程序启动之后首先加载的运行脚本rcS（在Nuttx配置文件defconfig中配置）和混控文件mixer。其中rcS运行脚本语法格式类Unix的Shell脚本，但并不完全相同。具体语法可以参见nuttx.org官网



#### src飞控中最重要的一个目录

包括：

drivers：驱动目录，其中存放所有的驱动模块，包括了GPS、MPU6000、HM5883等等

include：包含头文件目录

lib：常用的库函数，例如全局坐标与本地坐标的转换函数等等

 modules：系统主要模块commander、mc_pos_control、mc_att_control等等

systemcmds：系统命令param、pwm、reboot等



#### Tools

此目录为px4编译与烧写固件的工具包，其中有很多采用Python编写的工具类或是使用shell脚本编译的工具程序。例如：固件烧写工具px_uploader.py；飞控参数生成工具srcparser.py；半物理仿真工具jmavsim等等



# Mavlink官网资料学习

## Dialect(本项目中omni_am.xml)的编写

```xml
<?xml version="1.0"?>
<mavlink>

    <include>common.xml</include>
    <dialect>3</dialect>   

    <messages>
        <message id="12000" name="ARM_ROTATION">
            <description>positions and angular velocitu of six arms of a hexacopter</description>
			......
            <field type="float" name="vel6" units="rad/s">angular velocity of arm6, in rad/s</field>
        </message>
    </messages>

</mavlink>
```

模板如上，注意两点

1. include表示版本继承common.xml，对大多数情况适用，如果不继承任何文件，需要自己设定<version>...</version>
2. 新dialect的序号要比现有最大的dialect序号大1，例如下一个就4，依次类推



## [Gazebo与ROS仿真教程](https://docs.px4.io/master/zh/simulation/ros_interface.html)



## [写一个简单的app](https://dev.px4.io/v1.11_noredirect/en/apps/hello_sky.html)

### Create a new directory 

**Firmware/src/examples/px4_simple_app**

### Create a new C file in that directory 

**px4_simple_app.c**

```c
#include <px4_platform_common/log.h>

__EXPORT int px4_simple_app_main(int argc, char *argv[]);

int px4_simple_app_main(int argc, char *argv[])
{
    PX4_INFO("Hello Sky!");
    return OK;
}
```

### Create and open a new *cmake* definition file

 **CMakeLists.txt**

```C
px4_add_module(
    MODULE examples__px4_simple_app
    MAIN px4_simple_app
    STACK_MAIN 2000
    SRCS
        px4_simple_app.c
    DEPENDS
    )
```

### 在Firmware/boards/px4/sitl/default.cmake文件中添加

examples/px4_simple_app

### 退回到PX4-Autopilot文件夹进行make

make之后，在命令栏输入**px4_simple_app**,会得到回复：“Hello Sky!"



## [PX4开发者官网例程——MAVROS发送自定义消息](https://docs.px4.io/master/zh/ros/mavros_custom_messages.html)

### 源码修改：

#### MAVROS：

1. 在**workspace/src/mavros/mavros_extras/src/plugins**下新建了一个cpp文件 keyboard_command.cpp

代码功能：从ROS消息主题/mavros/keyboard_command/keyboard_sub订阅一个字符消息，作为MAVLink消息发送出去

2. 在 **workspace/src/mavros/mavros_extras**中修改**mavros_plugins.xml**文件，加入

```xml
<class name="keyboard_command" type="mavros::extra_plugins::KeyboardCommandPlugin" base_class_type="mavros::plugin::PluginBase">
     <description>Accepts keyboard command.</description>
</class>
```

3. 在 **workspace/src/mavros/mavros_extras**中修改**CMakeLists.txt**，加入

```c++
add_library( 
...
  src/plugins/keyboard_command.cpp 
)
```

4. 在**workspace/src/mavlink/message_definitions/v1.0**下的**common.xml**中加入

```xml
  <message id="229" name="KEY_COMMAND">
     <description>Keyboard char command.</description>
     <field type="char" name="command"> </field>
   </message>
```

#### PX4：

1. 在相应的**common.xml** (**PX4-Autopilot/mavlink/include/mavlink/v2.0/message_definitions**)中加入

```xml
  <message id="229" name="KEY_COMMAND">
     <description>Keyboard char command.</description>
     <field type="char" name="command"> </field>
   </message>
```

2. 删除**v2.0**路径下的common, standard文件夹，用mavlink_generator重新生成文件

```bash
python3 mavgenerate.py
```

3. 在**PX4-Autopilot/msg**中定义uORB消息， key_command.msg
4. 修改msg文件夹的**CMakeLists.txt**，加入key_command.msg
5. 修改**PX4-Autopilot/src/modules/mavlink**中的**mavlink_receiver.h**文件（主要是要include key_command.h头文件，加入相应的handle函数）
6. 修改**PX4-Autopilot/src/modules/mavlink**中的**mavlink_receiver.cpp**文件。这是 PX4 接收 ROS 发送过来的 MAVLink 消息的地方，并且将消息作为 uORB 主题发布。（代码比较复杂，看原文章）
7. 订阅uORB主题，在 **/PX4-Autopilot/src/modules**中新建key_receiver文件夹，创建 **CMakeLists.txt**, **key_receiver.cpp**两个文件
8. 在**PX4-Autopilot/boards/**相应文件夹（我需要用到的是sitl）中修改default.cmake，在MODULES中添加key_receiver

### 编译

#### ROS

```bash
catkin build
```

/workspace/src/mavros/mavros/launch中设置px4.launch文件

#### PX4

```bash
make px4_sitl_default gazebo    
```

### 运行

#### ROS

terminal 1:

```bash
roslaunch mavros px4.launch
```

terminal 2:

```bash
rostopic pub -r 10 /mavros/keyboard_command/keyboard_sub std_msgs/Char 97
```

这时在第一个终端中应该会持续收到打印的字符

#### PX4

```bash
 make px4_sitl_default gazebo
```

编译完成之后按回车，出现pxh>符号

输入key_receiver就成功啦！



## Development

### [适用于开发完整应用的模板](https://docs.px4.io/master/en/modules/module_template.html)







# [PX4工作流程](https://blog.csdn.net/qq_18112493)

## PX4程序编译过程解析

### Makefile(编译源)

首先在根目录下找到Makefile，所有的编译过程都是从需要编译的根目录下的Makefile文件开始，打开该文件开始分析。整个文件以`# -----`分段

（英文注释为源代码中的注释，中文注释为个人理解和博客解释）

```makefile
ifeq ($(wildcard .git),)
    $(error YOU HAVE TO USE GIT TO DOWNLOAD THIS REPOSITORY. ABORTING.)
endif
# 对git是否存在进行判断 (后面原网页说的对cmake进行判断的代码没有了，应该是版本区别)

# Help
# --------------------------------------------------------------------
#该makefile只是向cmake传递参数来允许我们保持wiki page，描述了如何build px4 firmware(现在改名为PX4-Autopilot)


# Parsing
# --------------------------------------------------------------------
FIRST_ARG := $(firstword $(MAKECMDGOALS))
ARGS := $(wordlist 2,$(words $(MAKECMDGOALS)),$(MAKECMDGOALS)) #这里应该对应make px4_sitl_default ...那条命令，再后面是对开发环境进行选择

NINJA_BIN := ninja                                          #这一部分属于对使用开发环境进行选择
......
ifdef replay                                                
	BUILD_DIR_SUFFIX := _replay
else
	BUILD_DIR_SUFFIX :=
endif

# Functions
# --------------------------------------------------------------------
# describe how to build a cmake config 
# 这段英文注释下面是进入cmake的编译规则设置，不管

# ADD CONFIGS HERE
# --------------------------------------------------------------------

# Other targets
# --------------------------------------------------------------------

# 选择不同的编译模式，可以看uorb_graph等等，后面基本都是命令选择，包括clean等等
```

### px4fmu-v2_default(编译文件讲解，我们用应该是sitl_default)

因为版本问题，原博客所说的文件(nuttx_px4fmu-v2_default.cmake)已经不存在，而cmake语法似乎也有所改变，call的文件名不再显式地在代码中给出。

根据makefile第210行代码，个人认为取而代之的是boards文件夹下的boards/px4/fmu-v2文件夹中的一系列cmake文件，这些文件中的module也如原博客所说的一样，可以在src/module中找到。

```makefile
210 # Get a list of all config targets boards/*/*.cmake
```

该文件定义了要参与编译的模块，真正需要用到的模块见下一章节的启动文件分析。



## PX4-rcS启动文件解析

rcS的启动类似于linux的shell文件，如果不知道shell文件是什么东西可以理解成是为程序的流程框，它是告诉处理器应该怎么样去运行。

首先rcS文件位于`ROMF/px4fmu_common/init.d或init.d-posix`中，前者对应nuttx OS后者对应posix，暂时理解为前者用于实物飞机，后者用于仿真。

```sh
#!/bin/sh
# PX4命令需要'px4-'前缀，因此需要px4-alias.sh文件在路径中，该文件会给每一个模块以alias <module>=px4-<module> 的形式设置好
. px4-alias.sh
# 在px4-alias.sh文件最开头有如下一行，也就是定义R为当前的工作路径，应该是PX4-Autopilot/build/px4_sitl_default/
## R="`pwd`/" ## 
# 因此可以看到在rcS中有很多类似PATH="$PATH:${R}etc/init.d-posix"的表述

set ...... # 一系列set，可以理解为define，定义一系列变量

# Use the variable set by sitl_run.sh to choose the model settings.
# 选择机型等参数，博客中的版本太老了，完全不一样，自己理解
# 这句代码重点解析，取airframe中的所有参数输出到 | 右侧，
REQUESTED_AUTOSTART=$(ls "${R}etc/init.d-posix/airframes" | sed -n 's/^\([0-9][0-9]*\)_'${PX4_SIM_MODEL}'$/\1/p')
# 根据代码下一段"# Load parameters"以及eeprom文件夹命名方式推测，REQUESTED_AUTOSTART变量的值是airframe代号(例如3060 for omni_am)。而Load parameters这一部分就是根据airframe文件加载物理模型

# PX4_ESTIMATOR 来自环境变量，但是没找到具体位置

```

```sh
# rcS启动的文件，rcS找不到的变量定义可以到这些文件里找一下
. ${R}etc/init.d/rc.vehicle_setup
. px4-alias.sh
. ${R}etc/init.d-posix/rc.replay
. "$autostart_file"
. px4-rc.params
. px4-rc.simulator
. ${R}etc/init.d/rc.vehicle_setup
. px4-rc.mavlink
. "$autostart_file".post
. ${R}etc/init.d/rc.logging
```



## [PX4底层驱动分析](https://blog.csdn.net/qq_18112493/article/details/69951086?spm=1001.2014.3001.5501)

原博客中所说的nsh/defconfig文件现在的目录是`PX4-Autopilot/boards/px4/fmu-v2/nuttx-config/nsh/defconfig`

该文件定义了许多参数，包括如下参数定义了默认驱动板是用的STM32F4

```text
CONFIG_ARCH_CHIP="stm32"
CONFIG_ARCH_CHIP_STM32=y
CONFIG_ARCH_CHIP_STM32F427V=y
```

还有stm32的AD,DMA,IIC,SDIO,SPI,串口，找不到的定义可以到这里来找。

下面这个config定义了APP层的一个入口

```text
CONFIG_USER_ENTRYPOINT="nsh_main" 
```

该文件位于`PX4-Autopilot/platforms/nuttx/NuttX/apps/system/nsh/nsh_main.c`，其中有一个初始化函数`nsh_initialize()`

<font color='red'>现在问题：找不到这个函数的定义</font>

博客后面都是spi相关设置，先跳过了，实物搭建时可能会需要













# PX4插件/模块源码阅读

## Module.h

**@brief**: 这个互斥锁(mutex)在模块的启动和关闭过程中保护不受竞争条件的影响。每个模块实例化可以有一个互斥锁，但是为了减少内存占用，只有一个全局互斥锁。这听起来很糟糕，但实际上我们不希望在这里发生争用，因为模块启动是连续的。

```cpp
extern pthread_mutex_t px4_modules_mutex;
```

**@class**: ModuleBase. Function: 'start', 'stop', 'status'等。这个类被用作curiously recurring template pattern (CRTP，奇异递归模板模式)。

---

**知识点**  CRTP，奇异递归模板模式

[多态](https://www.runoob.com/cplusplus/cpp-polymorphism.html)

多态按字面的意思就是多种形态。当类之间存在层次结构，并且类之间是通过继承关联时，就会用到多态。C++ 多态意味着调用成员函数时，会根据调用函数的对象的类型来执行不同的函数。

C++ 多态意味着调用成员函数时，会根据调用函数的对象的类型来执行不同的函数。

有了多态，可以有多个不同的类，都带有同一个名称但具有不同实现的函数，函数的参数甚至可以是相同的。

下面代码中涉及到的virtual函数就是一个多态的应用。继承的子类可以定义和使用一个同名的函数，类似于在父类中预约一个这样的函数。

而子类中用override表示当前函数重写了基类的虚函数

CRTP:

CRTP主要应用于静态多态，要进行扩展同时又要顾忌效率的代码。

将派生类作为基类的模板参数，意味基类可以访问派生类的一些内容。

```cpp
// The Curiously Recurring Template Pattern (CRTP)
template<class T>
class Base{
    // methods within Base can use template to access members of Derived
};
class Derived : public Base<Derived>{
    // ...
};
```

---

**@note**: 子类必须要有的method. 

任务在自己的线程(thread)上跑的时候：必须要有task_spawn, *instantiate, custom_command, print_suage

当在work queue上跑的时候： custom_command, print_suage与上面一样，task_spawn有些许不同

**代码段1** 定义class ModuleBase

```cpp
template <class T>
class ModuleBase
{
public:
    ModuleBase() : _task_should_exit{false} {}
    virtual ~ModuleBase() {}
	static int main(int argc, char *argv[]){/*Module entry point*/}
    static int run_trampoline(int argc, char *argv[]){/*Entry point for px4_task_spawn_cmd*/}
    static int start_command_base(int argc, char *argv[]){/*Command start*/}
    static int stop_command(){/*Command stop*/}
    static int status_command(){/*Command status*/}
    virtual int print_status(){/*虚函数，在子类中定义*/}
    virtual void run(){}
    static bool is_running(){}
protected:
    virtual void request_stop(){/*used from outside or inside the module thread*/}
    bool should_exit() const {/*used within the module thread*/}
    ...
    static px4::atomic<T *> _object;
    static int _task_id;
    static constexpr const int task_id_is_work_queue = -2;
private:
    static void lock_module(){}
    static void unlock_module(){}
}
```



控制部分：PX4多旋翼模型对应所需要启动的的控制器代码文件，都在**px4fmu_common/init.d/rc.mc_app**中列出来了，主要是用到了mc_att_control，mc_pos_control，mc_rate_control三个module

## PositionControl.cpp

src/module/mc_pos_control/PositionControl.cpp

```cpp
void PositionControl::setVelocityGains(){/*得到PID参数，在配置文件里*/}
void PositionControl::setVelocityLimits(){/*horizontal, up, down*/}
void PositionControl::setThrustLimits(){/*min, max(min有一个最小值10e-4)*/}
void PositionControl::updateHoverThrust(){/*更新悬停力*/}
void PositionControl::setState(){/*位置、速度、yaw、acc*/}
void PositionControl::setInputSetpoint(){/*位置、速度、yaw、acc*/}
bool PositionControl::update(){/*位置、速度、yaw、acc*/}
void PositionControl::_positionControl(){/*velo_sp = delta_P * gain_P*/}
void PositionControl::_velocityControl(const float dt){/*acc_sp = velo_PID*/}
void PositionControl::_accelerationControl(){/*得到thrust_sp*/}
bool PositionControl::_updateSuccessful(){/**/}
void PositionControl::getLocalPositionSetpoint(){/*得到set points*/}
void PositionControl::getAttitudeSetpoint(){/**/}
```



## ControlAllocation.cpp

src/modules/control_acllocator/ControlAllocation/ControlAllocation.cpp

Function1: ControlAllocation::setEffectivenessMatrix(输入参数：下面的一个矩阵, 一个向量, int actuators序号？)

const Matrix: <float, 桨数量（6），Actuator数量（16）> 名称effectiveness

const Vector: <float, Actuatoer] Armed by internal command
WARN  [commander] Failsafe enabled: No manual control stick input
INFO  [commander] Failsafe modr数量（16）> 名称 actuator_trim













# PX4 源码修改

目的：找到期望值的输入位置，从四维扩展到六维，然后修改PX4控制部分的计算方式，输出分离式的力和力矩

修改内容：

msg/vehicle_attitude_setpoint.msg 中添加了roll_sp_move_rate, pitch_sp_move_rate



Attitude_setoint 来自于Position Control，输出到Attitude Control来使用







# 阅读Devel_log.md

1. 在`tools/sitl_run.sh`里面可以修改`gz model --verbose --spawn-file`的参数

   * 注意把RPY角度设为0，~~模型是否会倒和sdfversion有关，建议在本机的gazebo生成默认的world文件~~模型会不会倒和物理设置有关

<font color='red'>这是在干嘛？？？我没有改，会怎样呢？</font>



1. 将solidworks转出的urdf转成sdf导入`Tools/sitl_gazebo/models/模型文件夹`
2. 在`/platforms/posix/cmake/sitl_target.cmake`的`set(models xxx)`添加对应的模型名称
3. Create an airframe file under `ROMFS/px4fmu_common/init.d-posix/airframes`
4. 在`romfs/px4fmu_common/init.d-posix/airframes/cmakelist.txt`添加模型
5. 在`msg`目录下定义arm_rotation.msg文件
6. 在`msg/cmakelists.txt`里面增加对应的文件
7. 在工作空间src文件夹里下载mavlink repo，`git clone git@github.com:mavlink/mavlink`
8. 在` mavlink/message_definitions/v1.0`下创建dialect文件，命名为`omni_am.xml`
9. 在`mavlink`文件夹终端下运行

```
python3 mavgenerate.py
```

选C和2.0，生成文件放到`mavlink/generated`下面，然后把文件全部复制到`PX4-Autopilot/mavlink/include/mavlink/v2.0`下

10. 修改`src/modules/mavlink/mavlink_messages.cpp`

11. 修改启动项`ROMFS/px4fmu_common/init.d-posix/px4-rc.mavlink`

    ```
    mavlink stream -r 50 -s ARM_ROTATION -u $udp_gcs_port_local
    ```

<font color='red'>这句话是什么意思？->见csdn px4收藏夹下的文章</font>

12. 修改`src/modules/mavlink/mavlink_main.cpp`文件

在`configure_streams_to_default()`的`case MAVLINK_MODE_ONBOARD`下面添加

```
configure_stream_local("ARM_ROTATION", 50.0f);
```

13. 配置mavros

* 安装一些工具

  ```
  sudo apt-get install python3-catkin-tools python3-rosinstall-generator -y
  ```

* 创建ros的工作空间

  ```
  mkdir -p /catkin_ws/src
  cd /catkin_ws
  catkin init
  wstool init src
  ```

* 拉取源码和配置编译环境

  ```
  rosinstall_generator --rosdistro noetic mavlink | tee /tmp/mavros.rosinstall
  rosinstall_generator --upstream mavros | tee -a /tmp/mavros.rosinstall
  wstool merge -t src /tmp/mavros.rosinstall
  wstool update -t src -j14
  rosdep install --from-paths src --ignore-src -y
  //这一步之后src下面多了mavlink和mavros的功能包
  sudo ./src/mavros/mavros/scripts/install_geographiclib_datasets.sh
  //这一步下载dataset
  ```

* 编译

  ```
  catkin build
  ```

  * 每次编译后都要执行`source devel/setup.bash`以便能用ros相关命令




14. 在mavros中自定义消息，对应uORB消息即可；加入cmakelists







<font color ='red'>arm_rotation.cpp中修改了成原来版本的了，与最新的有较大差别</font>>















---

# 2021.10.08 PX4官网资料学习



1. [Install PX4 firmware](https://docs.px4.io/master/en/config/firmware.html#installing-px4-master-beta-or-custom-firmware) (including "custom" firmware with your own changes).
2. [Start with the airframe](https://docs.px4.io/master/en/config/airframe.html) that best-matches your vehicle from the [airframe reference](https://docs.px4.io/master/en/airframes/airframe_reference.html).
3. [Basic Configuration](https://docs.px4.io/master/en/config/) explains how to perform basic configuration.
4. [Parameter Configuration](https://docs.px4.io/master/en/advanced_config/parameters.html) explains how you can find and modify individual parameters.





# 1. Toolchain Installation: [Ubuntu Setup](https://docs.px4.io/master/en/dev_setup/dev_env_linux_ubuntu.html)

有视频教学

Download PX4 Source Code:

```bash
git clone https://github.com/PX4/PX4-Autopilot.git --recursive
```

Run the **ubuntu.sh** with no arguments (in a bash shell) to install everything:

```bash
bash ./PX4-Autopilot/Tools/setup/ubuntu.sh
```

```bash
make px4_sitl gazebo
make px4_sitl jmavsim
```

然后就会出现图形界面，就可以飞了

# 2. Write your First APP

新的文件夹**PX4-Autopilot/src/examples/px4_simple_app**

新文件**px4_simple_app.c**

```c
/**
 * @file px4_simple_app.c
 * Minimal application example for PX4 autopilot
 *
 * @author Example User <mail@example.com>
 */

#include <px4_platform_common/log.h>

__EXPORT int px4_simple_app_main(int argc, char *argv[]);

int px4_simple_app_main(int argc, char *argv[])
{
	PX4_INFO("Hello Sky!"); //相当于printf for PX4 shell (included from px4_platform_common/log.h). There are different log levels: PX4_INFO, PX4_WARN, PX4_ERR, PX4_DEBUG. Warnings and errors are additionally added to the ULog and shown on Flight Review .
	return OK;
}
```

The main function must be named `<module_name>_main` and exported from the module as shown.

新文件**CMakeLists.txt**

```cmake
px4_add_module(
	MODULE examples__px4_simple_app #这个名字要一直回溯到src文件夹下，因为是src/example/px4_simple_app所以要这样写
	MAIN px4_simple_app #主函数入口
	STACK_MAIN 2000
	SRCS
		px4_simple_app.c
	DEPENDS
	)
```



> 以下部分涉及到动态链接库，暂时没看
>
> TIP
>
> The `px4_add_module()` format is documented in [PX4-Autopilot/cmake/px4_add_module.cmake](https://github.com/PX4/PX4-Autopilot/blob/master/cmake/px4_add_module.cmake)
>
> Note
>
> If you specify `DYNAMIC` as an option to `px4_add_module`, a *shared library* is created instead of a static library on POSIX platforms (these can be loaded without having to recompile PX4, and shared to others as  binaries rather than source code). Your app will not become a builtin command, but ends up in a separate  file called `examples__px4_simple_app.px4mod`. You can then run your command by loading the file at runtime using the `dyn` command: `dyn ./examples__px4_simple_app.px4mod`



新文件**Kconfig**

```text
 bool "PX4 Simple app"
 default n
 ---help---
 	Enable PX4 simple app
```

**Test APP (SITL)**

```sh
pxh> px4_simple_app
INFO  [px4_simple_app] Hello Sky!
```



## Subscribing to Sensor Data

app之间的独立信息通道叫做topics

订阅一个topic，比如想订阅sensor_combined的消息，poll()函数保持挂起，一旦有数据来了，就可以读取，一个poll可以等待多个topics

```cpp
#include <poll.h>
#include <uORB/topics/sensor_combined.h>
..
int sensor_sub_fd = orb_subscribe(ORB_ID(sensor_combined));

/* one could wait for multiple topics with this technique, just using one here */
px4_pollfd_struct_t fds[] = {
    { .fd = sensor_sub_fd,   .events = POLLIN },
};

while (true) {
	/* wait for sensor update of 1 file descriptor for 1000 ms (1 second) */
	int poll_ret = px4_poll(fds, 1, 1000);
	..
	if (fds[0].revents & POLLIN) {
		/* obtained data for the first file descriptor */
		struct sensor_combined_s raw;
		/* copy sensors raw data into local buffer */
		orb_copy(ORB_ID(sensor_combined), sensor_sub_fd, &raw);
		PX4_INFO("Accelerometer:\t%8.4f\t%8.4f\t%8.4f",
					(double)raw.accelerometer_m_s2[0],
					(double)raw.accelerometer_m_s2[1],
					(double)raw.accelerometer_m_s2[2]);
	}
}
```

## Publish Data

mavlink app 将attitude 送到地面控制站

```c
#include <uORB/topics/vehicle_attitude.h>../* advertise attitude topic */struct vehicle_attitude_s att;memset(&att, 0, sizeof(att));orb_advert_t att_pub_fd = orb_advertise(ORB_ID(vehicle_attitude), &att);
```

```c
orb_publish(ORB_ID(vehicle_attitude), att_pub_fd, &att);
```



## Full Code

```c
/**
 * @file px4_simple_app.c
 * Minimal application example for PX4 autopilot
 *
 * @author Example User <mail@example.com>
 */

#include <px4_platform_common/px4_config.h>
#include <px4_platform_common/tasks.h>
#include <px4_platform_common/posix.h>
#include <unistd.h>
#include <stdio.h>
#include <poll.h>
#include <string.h>
#include <math.h>

#include <uORB/uORB.h>
#include <uORB/topics/sensor_combined.h>
#include <uORB/topics/vehicle_attitude.h>

__EXPORT int px4_simple_app_main(int argc, char *argv[]);

int px4_simple_app_main(int argc, char *argv[])
{
	PX4_INFO("Hello Sky!");

	/* subscribe to sensor_combined topic */
	int sensor_sub_fd = orb_subscribe(ORB_ID(sensor_combined));
	/* limit the update rate to 5 Hz */
	orb_set_interval(sensor_sub_fd, 200);

	/* advertise attitude topic */
	struct vehicle_attitude_s att;
	memset(&att, 0, sizeof(att));
	orb_advert_t att_pub = orb_advertise(ORB_ID(vehicle_attitude), &att);

	/* one could wait for multiple topics with this technique, just using one here */
	px4_pollfd_struct_t fds[] = {
		{ .fd = sensor_sub_fd,   .events = POLLIN },
		/* there could be more file descriptors here, in the form like:
		 * { .fd = other_sub_fd,   .events = POLLIN },
		 */
	};

	int error_counter = 0;

	for (int i = 0; i < 5; i++) {
		/* wait for sensor update of 1 file descriptor for 1000 ms (1 second) */
		int poll_ret = px4_poll(fds, 1, 1000);

		/* handle the poll result */
		if (poll_ret == 0) {
			/* this means none of our providers is giving us data */
			PX4_ERR("Got no data within a second");

		} else if (poll_ret < 0) {
			/* this is seriously bad - should be an emergency */
			if (error_counter < 10 || error_counter % 50 == 0) {
				/* use a counter to prevent flooding (and slowing us down) */
				PX4_ERR("ERROR return value from poll(): %d", poll_ret);
			}

			error_counter++;

		} else {

			if (fds[0].revents & POLLIN) {
				/* obtained data for the first file descriptor */
				struct sensor_combined_s raw;
				/* copy sensors raw data into local buffer */
				orb_copy(ORB_ID(sensor_combined), sensor_sub_fd, &raw);
				PX4_INFO("Accelerometer:\t%8.4f\t%8.4f\t%8.4f",
					 (double)raw.accelerometer_m_s2[0],
					 (double)raw.accelerometer_m_s2[1],
					 (double)raw.accelerometer_m_s2[2]);

				/* set att and publish this information for other apps
				 the following does not have any meaning, it's just an example
				*/
				att.q[0] = raw.accelerometer_m_s2[0];
				att.q[1] = raw.accelerometer_m_s2[1];
				att.q[2] = raw.accelerometer_m_s2[2];

				orb_publish(ORB_ID(vehicle_attitude), att_pub, &att);
			}

			/* there could be more file descriptors here, in the form like:
			 * if (fds[1..n].revents & POLLIN) {}
			 */
		}
	}

	PX4_INFO("exiting");

	return 0;
}
```



# 3. Application/Module Template

一个app既可以作为task（单独的进程）也可以作为work queue task（线程，share stack and thread priorit），一般来说都用后者，因为最节省资源

#### work queue task(模板位于src/examples/work_item)

与常规task差不多，只是需要specify他是一个work queue task，然后在初始化的时候要schedule itself

1. CMakeLists.txt需要在DEPENDS里加px4_work_queue
2. 继承ModuleBase, ScheduledWorkItem（在对应的.hpp文件中定义的）
3. 添加到某一个queue中，例如例子中就是添加到 wq_configurations::test1；可以用的queue在WorkQueueManager.hpp中(platforms/common/include/px4_platform_common/px4_work_queue/WorkQueueManager.hpp)

```C++
WorkItemExample::WorkItemExample() :
    ModuleParams(nullptr),
    ScheduledWorkItem(MODULE_NAME, px4::wq_configurations::test1)
{
}
```

4. 用 `ScheduledWorkItem::Run()` 方法来使其工作
5. 用`task_spawn`方法specify这个task是一个work queue
6. 用一个schedule方法来预约这个work queue task

#### Tasks(模板位于src/templates/template_module)

完整应用需要的或者有用的additional features/aspects

1. 与参数交互，并且要应对参数更新
2. uORB订阅
3. 控制task在后台工作
4. 命令行工具
5. Documentation PRINT_MODULE_* (module help的时候用)



# 4. PX4 System Architecture

Flight Controller (only)

<img src="./Pics_PX4Learning/PX4Architecture_FlightControllerOnly.jpg" alt="PX4Architecture_FlightControllerOnly" style="zoom:50%;" />

FC & Companion computer

<img src="./Pics_PX4Learning/PX4Archi_FC&CompanionComputer.jpg" alt="PX4Archi_FC&CompanionComputer" style="zoom:50%;" />



FC与上一种一样跑常规的PX4飞行堆栈(flight stack)，Companion computer提供避障和防撞。两个系统通过mavlink连接，与地面站和cloud的连接也是通过companion computer



# 5. PX4 Overview

**两个主要的层次**

**Flight stack是测试和飞行控制系统**

**middleware是一个通用的机器人layer，支持各种各样的机器人，提供内外communication和硬件集成**



## High-Level Software架构

图片的上面部分是middleware，下面部分是flight stack。 

每个部分都是一个模块，程序运行的时候，可以通过`top`命令(在Nuttx Shell中，SITL Shell中是pxh>)查看哪些模块在工作，并通过`<module_name> start/stop`控制。

![PX4Structure](./Pics_PX4Learning/PX4Structure.jpg)

可分为五个部分：数据存取(Storage)，外部通讯(External Connectivity)，驱动程序(Drivers)，飞行控制(Flight Control)，消息总线(Message Bus)

### Flight Stack:

导航、控制算法的集合，包含控制器和位姿估计器

![PX4Archi_FightStackBuildingBlock](./Pics_PX4Learning/PX4Archi_FightStackBuildingBlock.jpg)

估计器测量，控制器计算，mixer转换



### Middleware:

包括嵌入传感器的驱动和与外界交流以及uORB message bus。此外，middleware包括仿真环境



### Runtime Environment:

模块工作方式：Tasks / Work queue tasks(WQT)

工作在WQT的好处是占用更少的内存。缺点是WQT不允许sleep或者对消息进行轮询(?)，或者 blocking IO(例如读取文件)。因此长时间工作的task应该选择tasks或者至少是独立的work queue。



# 6. Flight Modes

决定了飞控如何相应用户的输入和控制飞机运动

基于飞控提供的控制级别 loosely grouped, 分为manual, assisted and auto modes.在地面站或者遥控器上有开关可以切换状态。

## Manual flight modes

遥控器或者遥杆直接控制，level可以改变飞机对控制信号的响应速度

对于多旋翼而言，

**MANUAL/STABILIZED**是一样的，通过roll和pitch角和yaw rate命令输入。Throttle（节流阀？）直接发送给mixer。飞控控制姿态，也就是说当控制器位于中间的时候会把roll和pitch调节到0。在这个模式下，位置不受飞控控制，所以会漂

**ACRO** 通过roll和pitch角和yaw rate命令输入。控制角速度，但不控制姿态。Throttle is passed directly to the output mixer.



## Flight Tasks

继承于FlightTask基类







## System Startup

`ROMFS/px4fmu_common/init.d` 与`ROMFS/px4fmu_common/init.d-posix`文件夹里的数字开头的文件，是封装的机架构型配置文件，在编译时会导出至airframs.xml中，QGC通过解析这些xml文件在UI界面上进行机架构型选择。

在启动过程中第一个运行的脚本文件是 init.d/rcS，该脚本会调用所有其他脚本。







## make 命令的使用

在`/platforms/posix/cmake/sitl_target.cmake`文件中`set(model xxx)`那里的名称就是可以加在gazebo_后面的（注意仿真器的选择）



## mixer混控器和执行器

混合意味着接收力的指令（比如： 向右转），然后将这些指令转换成实际的执行器指令来控制电机或者舵机。 

将混控逻辑与实际的姿态控制器分离开来大大提高了程序的可复用性。 

`控制器`发送（-1，+1）的指令给`混控器`，`混控器`相应地去设置每个单独的执行器。

`Output driver(UART,UAVCAN,PWM etc)`将混控器输出缩放为原来的单位。

### 控制组

PX4系统用`控制组`和`输出组`。`控制组`是姿态等控制量，输出则是物理上的总线、例如飞空的PWM舵机输出口。混控器定义了控制信号如何连接到输出口。

控制组 ---> 混控器 ---> 输出组

### 映射

一个控制组可以向多个输出组发送命令。混控器没有指明实际的输出组。相反，混控器的目的地是通过mixer文件名定义的，然后映射到相应的物理总线上。

这样做的原因是MAIN output的输出物理总线并不总是相同的，它取决于飞控是否有IO板或者是否用UAVCAN来进行电机控制。

### 混控器定义

文件名：*XXXX.main.mix* for MAIN output, *YYYY.aux.mix* for AUX output(副翼，我们不需要)

MAIN file 在`airframes`的文件中用set MIXER XXXX`来加载`XXXX.main.mix`文件

### 加载一个自定义的混控器

命名规则如上

#### 语法

---

四种混控器： 

multirotor mixer 多旋翼，输出数取决于机型

helicopter mixer 直升机

summing mixer 对于伺服电机或者驱动器控制，一个output

null mixer 产生一般情况必须为0的输出，一个output

VTOL mixer conbinesthe other mixer types.

---

可以在一个文件中定义多个混控器，输出顺序取决于读取mixer定义的顺序。对于PWM设备来说输出顺序与declear顺序相同。

比如对一个四旋翼无人机，定义了一个multi-rotor mixer,然后null mixer和两个summing mixer。那么就会先是一个四输出，再是一个空输出，再是两个输出。

---

mixer definition:

```bash
<tag>: <mixer arguments>
```

tag: 

```bash
R: Multirotor mixer
H: Helicopter mixer
M: Summing mixer
Z: Null mixer
```

不是以`大写字母：`形式开头的行会被忽略，所以注释可以随意的写

---

#### Summing Mixer

用于actuator和伺服控制。

Summing mixer将0或者更多的控制输入合并成一个actuator输出。输入是缩放（归一化）之后的，mixing函数将结果加起来之后给到output scaler。

定义：

```text
M: <control count>
O: <-ve scale> <+ve scale> <offset> <lower limit> <upper limit> <traversal time>
S: <group> <index> <-ve scale> <+ve scale> <offset> <lower limit> <upper limit>
```

当control count是0的时候，sum就是0，然后就会输出offset值，这个值被lower limit和upper limit 限制。

`<traversal time>`存在的作用是怕actuator运动太快就会损坏，所以做一个限制。

所有系数都会除一个10000。

举个例子：`<traversal time>` = 20000 意味着actuator至少需要两秒才能从`<lower limit>`到`<upper limit>`

不要给控制姿态的actuator添加limit，这很可能会导致控制器失效。

S行：必须在O行下面。 `<group>` 确定从哪个控制组（mixer部分的最开始介绍了控制组的序号，例如0是Flight Control，1是Flight Control VTOL/Alternate）读取scaler。`<index>`指哪一个值。例如：`<group> = 0, <index> = 0` 就表示roll

![ControlGroup](./Pics_PX4Learning/ControlGroup.jpg)

#### Null Mixer

输出恒为0的值。一般在mixer的集合里用作占位符(placeholder)以实现actuator输出的某一个特定的排列。也可能作为filesafa设备输出值的控制。

定义：

```text
Z:
```

#### 针对多旋翼的混控器(Multirotor mixer)

将四个输入(roll, pitch, yaw, thrust)转换成一系列驱动器输出来驱动电机转速控制器。

定义：

```text
R: <geometry> <roll scale> <pitch scale> <yaw scale> <idlespeed>
```

`<geometry>`取值： 4X, 4+, 6X, 6+, 8X, 8+ 分别表示不同旋翼的不同构型

`<roll scale> <pitch scale> <yaw scale>`决定了这几个值相对于thrust control的scale

`<idlespeed>`是用电机最大转速归一化的值，是当所有控制输入都为0时电机的转速。

当某一个actuator的值饱和了，就要重新scale，所以饱和值可以限定在1。



## 消息总线：异步消息机制（uORB）

它可以实现不同模块中的数据快速通讯，并且以异步通讯为基本原则，也就是说在通讯过程中发送者只负责发送数据，而并不关心数据由谁接收，也不关心接收者是否能将所有的数据都接收到；而对于接收者来说并不关心数据是由谁发送的，也不关心在接收过程中是否将所有数据都接收到。





# 2021.10.11 PX4代码阅读笔记

## 启动命令（具体见https://blog.csdn.net/lipi37/article/details/102918032）

```bash
make px4_sitl_ctrlalloc gazebo_omni_hex
```

这里首先涉及到makefile的阅读，学习了makefile的编写规则后，可以在PX4-Autopilot下的Makefile中看到有一个这样的部分，%表示缺省。这里就涉及到了所有的config_targets，不知道有哪些的话可以在PX4-Autopilot路径下用make list_config_targets命令显示

```makefile
FIRST_ARG := $(firstword $(MAKECMDGOALS))
ARGS := $(wordlist 2,$(words $(MAKECMDGOALS)),$(MAKECMDGOALS))
# -------------------------------------------------------------------
# wordlist	获取指定从某个索引到到另一个索引的之间的单词, 例:$(wordlist 2, 5, “This is a test sentence”), 返回 “is” “a” “test” “sentencce”
# 因此这里FIRST_ARG和ARGS分别就是px4_sitl_ctrlalloc，gazebo_omni_hex
# -------------------------------------------------------------------

# Get a list of all config targets boards/*/*.cmake
ALL_CONFIG_TARGETS := $(shell find boards -maxdepth 3 -mindepth 3 -name '*.cmake' -print | sed -e 's|boards\/||' | sed -e 's|\.cmake||' | sed -e 's|\/|_|g' | sort)

# ADD CONFIGS HERE
# -------------------------------------------------------------------
#  Do not put any spaces between function arguments.
# -------------------------------------------------------------------
# ..........省略...........
# -------------------------------------------------------------------

# All other targets are handled by PX4_MAKE. Add a rule here to avoid printing an error.
%:
	$(if $(filter $(FIRST_ARG),$@), \
		$(error "Make target $@ not found. It either does not exist or $@ cannot be the first argument. Use '$(MAKE) help|list_config_targets' to get a list of all possible [configuration] targets."),@#)

```

首先第一个参数 **px4_sitl_ctrlalloc**，在路径`PX4-Autopilot/boards/px4/sitl`下有ctrlalloc.cmake和default.cmake等文件，因此第一个参数可以是px4_sitl_ctrlalloc或者px4_sitl_default等等（一个cmake文件对应一个命令），对于boards下其他路径同理（不过在最新版本中好像用kconfig换掉了cmake文件）

第二个参数**gazebo_omni_hex**，如果选择sitl仿真，那么就会选择posix平台，也就是进入PX4-Autopilot/platforms/posix路径，其下的CMakeLists.txt文件中如下一句负责添加sitl_target.cmake文件，在`PX4-Autopilot/platforms/posix/cmake`路径下的文件sitl_target.cmake，负责管理所有仿真器和模型等，如下代码所示，我们需要在set(models)最后加上omni_hex，如果不加的话就会报错 `unknown target 'gazebo_omni_hex'`，也就起到了链接gazebo仿真器和omni_hex模型的作用。

此外，`PX4-AutopilQGot/Tools/sitl_gazebo/models`下储存模型文件，其中的omni_hex是我们需要用的。

```cmake
include(sitl_target) #添加sitl_target.cmake文件
```

```cmake
set(viewers
	none
	jmavsim
	gazebo
	ignition
)
set(models
	none
	believer
	boat
	...
	omni_hex
）
```

## Module启动顺序

rcS中写了一些关键module的启动顺序，主要顺序为

```
px4-rc.params
210 dataman start
px4-rc.simulator
216 load_mon start
217 battery_simulator start
218 tone_alarm start
219 rc_update start
220 sensors start
221 commander start
222 navigator start
if px4-micrortps_client status > /dev/null 2>&1
then
    . px4-rc.rtps
fi
if param greater -s MNT_MODE_IN -1
then
	vmount start
fi
if param greater -s TRIG_MODE 0
then
	camera_trigger start
	camera_feedback start
fi
if param compare -s IMU_GYRO_FFT_EN 1
then
	gyro_fft start
fi
if param compare -s IMU_GYRO_CAL_EN 1
then
	gyro_calibration start
fi
# Note: rc.vehicle_setup is the entry point for rc.interface,
#       rc.fw_apps, rc.mc_apps, rc.rover_apps, and rc.vtol_apps.
#
. ${R}etc/init.d/rc.vehicle_setup

. px4-rc.mavlink
```

通过rc.vehicle_setup启动rc.fw_apps, rc.mc_apps, rc.rover_apps, and rc.vtol_apps等等，在这里再启动mc_att_control等module

## 输入参数以及地面站的连接和控制

对于启动命令搞清楚之后，接下来应该弄懂控制数据是如何传输到控制器的。

1. 通过px4_simple_app，这是官方教程中教学写的一个插件，可以通过该插件用uORB消息给控制模块发送目标位置或姿态等命令，比较简单，之后再说
2. 通过QGroundControl控制，包括控制起飞，通过摇杆控制等等

通过QGC控制主要包含两个Mode，`NAVIGATION_STATE_ACRO`控制姿态，`NAVIGATION_STATE_POSCTL`控制位置

PX4-Autopilot/src/modules/commander/Commander.cpp文件的update_control_mode()负责切换这两种mode，在case vehicle_status_s::NAVIGATION_STATE_ACRO不同的case下可以设定控制的变量。如下代码可以看出，两个Mode的唯一区别就在于ACRO mode中注掉的那一行，也就是在ACRO mode中没有control attitude

```cpp
case vehicle_status_s::NAVIGATION_STATE_POSCTL:
		_vehicle_control_mode.flag_control_manual_enabled = true;
		_vehicle_control_mode.flag_control_rates_enabled = true;
		_vehicle_control_mode.flag_control_attitude_enabled = true;
		_vehicle_control_mode.flag_control_altitude_enabled = true;
		_vehicle_control_mode.flag_control_climb_rate_enabled = true;
		_vehicle_control_mode.flag_control_position_enabled = !_status.in_transition_mode;
		_vehicle_control_mode.flag_control_velocity_enabled = !_status.in_transition_mode;
		break;

case vehicle_status_s::NAVIGATION_STATE_ACRO:
		_vehicle_control_mode.flag_control_manual_enabled = true;
		_vehicle_control_mode.flag_control_rates_enabled = true;
		//_vehicle_control_mode.flag_control_attitude_enabled = true;
		_vehicle_control_mode.flag_control_altitude_enabled = true;
		_vehicle_control_mode.flag_control_climb_rate_enabled = true;
		_vehicle_control_mode.flag_control_position_enabled = !_status.in_transition_mode;
		_vehicle_control_mode.flag_control_velocity_enabled = !_status.in_transition_mode;
		break;
```





## 摇杆控制：

在src/module/commander/ManualControl.hpp中有对manual_control_setpoint的订阅

在/home/liuboyuu/omniAM/PX4-Autopilot/src/modules/flight_mode_manager/tasks/Utility/Sticks.cpp中有对manual_control_setpoint的应用，得到了_position和 _position_expo

/home/liuboyuu/omniAM/PX4-Autopilot/src/modules



Anyway，现在假设QGC将manual_control_setpoint通过uORB消息pub出来，然后PX4这边接受，那么接下来怎么确定自己在处理之后pub的是最新的数据呢？又或者说是在哪里定义了用到的module呢？用到的module应该是mc_att_control或者mc_pos_control



## [mc_att_control_main.cpp](https://blog.csdn.net/XL__MAX/article/details/106284121)

```cpp
int mc_att_control_main(int argc, char *argv[])
{
	return MulticopterAttitudeControl::main(argc, argv);
}
```

MulticopterAttitudeControl类本身没有定义main函数，而是是用的所继承的`public ModuleBase<MulticopterAttitudeControl>`类中的main函数，这个main函数就是对接收“mc_att_control start”命令，并且在第一次接收该命令的时候调用start_command_base()函数，然后再在该函数内部调用T::task_spawn(argc, argv);（即MulticopterAttitudeControl::task_spawn(int argc, char *argv[])函数）

task_spawn函数中主要做了三件事，一个是实例化一个类的对象，二是将这个模块给这对象初始化

```cpp
MulticopterAttitudeControl *instance = new MulticopterAttitudeControl(vtol); // vtol vertical takeoff 垂直起降
```



## mavlink 命名后缀_s _t解析

```cpp
void
MavlinkReceiver::handle_message_gimbal_manager_set_attitude(mavlink_message_t *msg)
{
	mavlink_gimbal_manager_set_attitude_t set_attitude_msg;
	mavlink_msg_gimbal_manager_set_attitude_decode(msg, &set_attitude_msg);
//mavlink_name_t是接收到的mavlink消息，然后通过mavlink_name_decode(msg, &set_attitude_msg 解码msg消息到set_attitude_msg中，set_attitude_msg的所有参数都定义在common.xml文件中对应的部分，按照handle_message_后面的名字进行查找即可。
	gimbal_manager_set_attitude_s gimbal_attitude{};
//gimbal_manager_set_attitude_s是后面发送uorb消息所用的结构体，gimbal_manager_set_attitude是msg的名字，_s是一个后缀，该变量的所有参数都定义在msg文件中
	gimbal_attitude.timestamp = hrt_absolute_time();
	gimbal_attitude.origin_sysid = msg->sysid;
	gimbal_attitude.origin_compid = msg->compid;
	gimbal_attitude.target_system = set_attitude_msg.target_system;
	gimbal_attitude.target_component = set_attitude_msg.target_component;
	gimbal_attitude.flags = set_attitude_msg.flags;
	gimbal_attitude.gimbal_device_id = set_attitude_msg.gimbal_device_id;
}

```



## mavlink消息

lyq定义的vehicle_attitude_bodyz_in只用在了px4_simple_app中，然后传递到MulticopterPositionControl.cpp

那么摇杆用的是哪个数据输入呢？



## offboard模式

当用户发送一个 ros 话题的消息后，对应的 plugin 中的回调函数被处罚，ros 消息被包装成 mavlink 消息，从链路中发送出去。



用户代码发送了一个 "/mavros/setpoint_position/local" 话题的消息，plugin中的代码setpoint_position.cpp的ros node为setpoint_position，订阅local话题，收到消息后触发 SetpointPositionPlugin::setpoint_cb 函数，回调函数调用send_position_target，再调用set_position_target_local_ned，从而把数据转换为`mavlink::common::msg::SET_POSITION_TARGET_LOCAL_NED`数据发送出去。



目前的情况：

make px4_sitl_default gazebo可以被ros节点offboard控制

make px4_sitl_ctrlalloc gazebo_omni_hex 不能被ros节点offboard控制

MulticopterPositionControl.cpp中_local_pos_sp_pub 传递到哪里去了呢



参照之前注掉的代码，将上面的_control.getLocalPositionSetpoint改成 _control.getAttitudeSetpoint试一下



publish attitude setpoint



```cpp
void MulticopterPositionControl::Run(){
    if (_local_pos_sub.update(&local_pos)) {
        const hrt_abstime time_stamp_now
        const float dt 

        setDt(dt);
        const bool was_in_failsafe
        _in_failsafe
        _vehicle_control_mode_sub.update(&_vehicle_control_mode);
        _vehicle_land_detected_sub.update(&_vehicle_land_detected);

        if (use_hte) //用hover thrust estimator
        {
            _control.updateHoverThrust;
        }

        PositionControlStates states{set_vehicle_states(local_pos)}; 
        //更新得到当前状态：位置速度加速度

        if (_vehicle_control_mode.flag_multicopter_position_control_enabled) {
            const bool is_trajectory_setpoint_updated;.
            if (_setpoint.timestamp < local_pos.timestamp) {
                // adjust existing (or older) setpoint with any EKF reset deltas 更新旧的setpoint x,y,z,yaw,vx,vy,vz
            }
            // update vehicle constraints and handle smooth takeoff
            _vehicle_constraints_sub.update(&_vehicle_constraints);

            //更新向上的速度
            _vehicle_constraints.speed_up = _param_mpc_z_vel_max_up.get();

            if (_vehicle_control_mode.flag_control_offboard_enabled) {
                _vehicle_constraints.want_takeoff; 
                //设置want_takeoff参数，条件1. setpoint.z有限且设定值小于当前位置（好像坐标系是反的？）; 
                // 2. setpoint.vz有限且目标速度小于0; 3. z方向设定加速度有限且小于0
            }

            // handle smooth takeoff
            _takeoff.updateTakeoffState();

            const bool flying = (_takeoff.getTakeoffState() >= TakeoffState::flight);

            if (is_trajectory_setpoint_updated) {
                if (not_taken_off || flying_but_ground_contact) {
                    reset_setpoint_to_nan(_setpoint);
                    Vector3f(0.f, 0.f, 100.f).copyTo(_setpoint.acceleration);
                    _control.resetIntegral();
                }
            }
            _control.setThrustLimits;
            _control.setVelocityLimits;
            _control.setInputSetpoint(_setpoint);

            // update states
            _control.setState(states);

            // Run position control
            if (_control.update(dt)) {
                _failsafe_land_hysteresis.set_state_and_update(false, time_stamp_now);
            else
            {
                //Failsafe

                failsafe(time_stamp_now, failsafe_setpoint, states, !was_in_failsafe);
                // reset constraints
                _vehicle_constraints = {0, NAN, NAN, NAN, false, {}};
                _control.setInputSetpoint(failsafe_setpoint);
                _control.setVelocityLimits(_param_mpc_xy_vel_max.get(), _param_mpc_z_vel_max_up.get(), _param_mpc_z_vel_max_dn.get());
                _control.update(dt);
            }

            // 发布位置和姿态控制
            vehicle_local_position_setpoint_s local_pos_sp{};
            _control.getLocalPositionSetpoint(local_pos_sp);
            local_pos_sp.timestamp = hrt_absolute_time();
            _local_pos_sp_pub.publish(local_pos_sp);

            vehicle_attitude_setpoint_s attitude_setpoint{};
            Vector3f world_vel;
            world_vel(0) = local_pos.vx;
            world_vel(1) = local_pos.vy;
            world_vel(2) = local_pos.vz;
            Vector3f angular_vel;
            angular_vel(0) = ang_vel.xyz[0];
            angular_vel(1) = ang_vel.xyz[1];
            angular_vel(2) = ang_vel.xyz[2];
            _control.getAttitudeSetpoint(attitude_setpoint, att, set_current_att, do_override_att, override_roll, override_pitch, world_vel, angular_vel);
            attitude_setpoint.timestamp = hrt_absolute_time();
            _vehicle_attitude_setpoint_pub.publish(attitude_setpoint);
        }

        // Publish takeoff status

        // save latest reset counters
    }

}
```

函数阅读笔记

- 参数查询

  _param_mpc_use_hte这样的参数，均可以通过在PX4-Devguide中查询得到，例如

  > MPC_USE_HTE (INT32): Hover thrust source selector, Set false to use the fixed parameter MPC_THR_HOVER Set true to use the value computed by the hover thrust estimator
  >
  > PX4-Autopilot/src/modules/mc_pos_control/mc_pos_control_params.c中定义了
  >
  > PARAM_DEFINE_FLOAT(MPC_THR_HOVER, 0.5f);



## 通过roslaunch启动

首先，需要添加环境变量，使其能find px4

```bash
export ROS_PACKAGE_PATH=$ROS_PACKAGE_PATH:~/omniAM/PX4-Autopilot
export ROS_PACKAGE_PATH=$ROS_PACKAGE_PATH:~/omniAM/PX4-Autopilot/Tools/sitl_gazebo
```



1105

启动时加入地图的方法

```bash
make px4_sitl_default gazebo_iris__warehouse
```

但如果用自己的模型就无法启动，还需要调查原因

另外，今天发现了一个新文件，但用起来好像没啥效果

roslaunch px4 mavros_posix_tests_offboard_posctl.test







```bash
rostopic list -v /topic_name # 如下命令可以获得topic的详细信息，比如消息格式、几个publisher，几个subscriber

rosmsg show nav_msgs/Odometry # how the data is structured
```

<img src="Pics_PX4Learning/Screenshot from 2021-11-06 19-52-13.png" alt="Screenshot from 2021-11-06 19-52-13" style="zoom:50%;" />





# 用SW画模型导入gazebo的正确姿势

## 获取每个link的mesh文件

1. 不要用SW2018SP0，因为这个版本的导出stl文件有bug
2. 尽量减少link的数量，按照关节来分开，把没有相对运动的block当成整体，一般每个link是一个装配体
3. 关于坐标系
   1. sdf里面的坐标系为前左上-xyz，右手系为选装正方向，注意和sw的default不一样，不要弄错了
   2. 在part或者子装配体中建立一个坐标系作为原点，在最外面的装配体中用配合与原点坐标系对齐
4. 关于单位转换，sdf里面用的是kg和m
5. 关于stl导出
   1. 务必hide很精细的一些part，否则导出文件非常大
   2. 选择自建的原点坐标系，meter单位，do not transform to positive space

## sdf的构造

1. 官方文档见http://sdformat.org/spec
2. inertia要选择原点坐标系，看taken at centre of mass and aligned with output coord
3. collision尽量不要用mesh，用最小包围轮廓，contact要给一个缓冲min_depth否则加速度会爆

## 关于plugin

1. 顺序尽量和官方的例子保持一致
2. imu一定要放在mavlink interface后面否则很卡



# 现在要解决的问题

1. 在栅格化的地图中用路径规划算法
3. 将其调整为适用于omni hex的算法
4. 实机实验









# CSDN上待整理的资料

![飞控系统控制流程](Pics_PX4Learning/Screenshot from 2021-10-13 09-21-32.png)

# 1224 omni_hex/PX4SIM/PX4-Autopilot代码整理

1. `sitl_run.sh`第180行可以修改gazebo的跟踪模式

2. 目前理解是不需要mavlink手动generate那个步骤了？只需要包含xml文件即可？——最新版本的PX4包含了mavlink submodule，还是需要手动generate，只是存放的路径变成了/PX4_Autopilot/src/modules/mavlink/mavlink/pymavlink/generator/C/include_v2.0

3. mavlink消息的添加和链接方式：在mavlink_message.cpp中添加#include "streams/ARM_ROTATION.hpp"，并在mavlink/streams下定义ARM_ROTATION.hpp文件，文件内容是class MavlinkStreamArmRotation : public MavlinkStream
4. 



# Bottom



