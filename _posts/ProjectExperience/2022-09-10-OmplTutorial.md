---
title: OMPL Tutorial
category: ProjectExperience
---

目标：利用现有八叉树地图设定边界，实现碰撞检测和RRT*的规划

# OMPL入门教程：

## 1 Geometric Planning for a Rigid Body in 3D（三维刚体的几何规划）

本教程描述如何为一个3D空间中的刚体(rigid body)做规划。有两种方式：用/不用ompl::geometric::SimpleSetup 类。区别在于对后者而言ompl::base::SpaceInformation 和 ompl::base::ProblemDefinition需要被实例化。而且，planner也需要被明确地实例化。相比之下，更推荐用ompl::geometric::SimpleSetup，因为更不容易出现bug而且完全没有在功能层面施加任何限制。



为三维刚体配置几何规划器需要以下步骤：

- 明确规划的空间：SE(3)
- 从可行的状态空间中选择一个，或者自己设置一个。对于SE(3)而言，ompl::base::SE3StateSpace即可
- 由于SE(3)包含一个$R^3$部分，所以需要设定边界
- 定义可行状态
- 定义起点和终点表示

这些步骤完成后，对问题的定义基本就完成了。示例如下：



### 用ompl::geometric::SimpleSetup 类

```cpp
// 设定命名空间
namespace ob = ompl::base;
namespace og = ompl::geometric;
// 声明状态可行检测函数
bool isStateValid(const ob::State *state);
 
void planWithSimpleSetup()
{
    // 首先建立一个规划的状态空间
    auto space(std::make_shared<ob::SE3StateSpace>());
    // 设定三维边界
    ob::RealVectorBounds bounds(3);
    bounds.setLow(-1);
    bounds.setHigh(1);
 
    space->setBounds(bounds);
    // 实例化一个SimpleSetup类，这个类的内部会实例化SpaceInfomation和ProblemDefinition
    og::SimpleSetup ss(space);
    // 设定状态可行检测(state validity checking)
    ss.setStateValidityChecker([](const ob::State *state) { return isStateValid(state); });
    // 设定一个随机的起始状态和目标状态，并放置到SimpleSetup中
    ob::ScopedState<> start(space);
    start.random();
    ob::ScopedState<> goal(space);
    goal.random();
    ss.setStartAndGoalStates(start, goal);
    // 接下来就可以尝试求解。这一命令会调用ompl::geometric::SimpleSetup::setup()并创建一个默认的规划器（因为没有指定），并且调用ompl::base::Planner::setup()，这个函数又调用ompl::base::SpaceInfomation::Setup()。这一系列调用会计算一些运行时参数，例如可行状态检测(state validity checking)的分辨率。然后从ompl::base::PlannerStatus返回一个描述“是否在给定时间(秒)内找到解”的值
    ob::PlannerStatus solved = ss.solve(1.0);
    // 如果找到解了，我们就可以选择simplify他然后输出他
    if (solved)
    {
        std::cout << "Found solution:" << std::endl;
        // print the path to screen
        ss.simplifySolution();
        ss.getSolutionPath().print(std::cout);
    }
}
```



### 不用ompl::geometric::SimpleSetup 类

```cpp
// 设定命名空间
namespace ob = ompl::base;
namespace og = ompl::geometric;
// 声明状态可行检测函数
bool isStateValid(const ob::State *state);
 
void plan()
{
    // 首先建立一个规划的状态空间
    auto space(std::make_shared<ob::SE3StateSpace>());
    // 设定三维边界
    ob::RealVectorBounds bounds(3);
    bounds.setLow(-1);
    bounds.setHigh(1);
 
    space->setBounds(bounds);
    // 为状态空间实例化一个ompl::base::SpaceInfomation类
    auto si(std::make_shared<ob::SpaceInformation>(space));
    // 设定状态可行检测(state validity checking)
    si->setStateValidityChecker(isStateValid);
    // 设定一个随机的起始状态和目标状态
    ob::ScopedState<> start(space);
    start.random();
    ob::ScopedState<> goal(space);
    goal.random();
    // 创建一个ompl::base::ProblemDefinition实例对象，并设定其实和目标状态
    auto pdef(std::make_shared<ob::ProblemDefinition>(si));
    pdef->setStartAndGoalStates(start, goal);
    // 创建一个规划器实例
    auto planner(std::make_shared<og::RRTConnect>(si));
    // 告诉规划器我们所要解决的问题
    planner->setProblemDefinition(pdef);
    // 确保按照上述顺序完成了所有的配置。接下来也是计算可行状态检测(state validity checking)的分辨率。
    planner->setup();
    // 接下来就可以尝试求解。这一命令会从ompl::base::PlannerStatus返回一个描述“是否在给定时间(秒)内找到解”的值
    ob::PlannerStatus solved = planner->ob::Planner::solve(1.0);
    // 如果找到解了，我们就可以选择simplify他然后输出他
    if (solved)
    {
        // 从problem definition中获取target的表示，与goal stateButong，并且查询找到的路径
        // and inquire about the found path
        ob::PathPtr path = pdef->getSolutionPath();
        std::cout << "Found solution:" << std::endl;
        // 打印路径
        path->print(std::cout);
    }
}
```





## 2 Setting up state validity checking 设置可行状态检查

OMPL本身并不包括可行状态检查的代码。这样的设置是取决于所要求解的问题的。比如，OMPL.app定义可行状态检测为检查加载的CAD模型的碰撞关系。MoveIt中的ROS OMPL接口定义可行状态检查为机器人模型与感知到的环境物体之间的碰撞检测。如果为蛋白质折叠做规划，那么可行状态检查就与能量函数有关了。

为了让用户定义可行状态，OMPL定义了两个抽象类

- ompl::base::StateValidityChecker
- ompl::base::MotionValidator

这些类将我们所规划的系统的性质抽象化了。例如，OMPL不需要表示机器人或者障碍物的几何关系。用户在配置ompl::base::SpaceInformation的时候可以设定具体用哪一种实现方式。注意，ompl::base::StateValidityChecker和ompl::base::MotionValidator类的实例化需要时线程安全的。

ompl::base::StateValidityChecker定义了ompl::base::StateValidityChecker::isValid()函数，规划器(planner)通过这个函数来评估一个状态是否是可行的。如果用户没有设置这个类的话，则会假设ompl::base::AllValidStateValidityChecker，也就是说所有的状态都是可行的。



```cpp
// 定义类
class myStateValidityCheckerClass : public base::StateValidityChecker
{
public:
     myStateValidityCheckerClass(const base::SpaceInformationPtr &si) :
       base::StateValidityChecker(si)
        {
     }
 
     virtual bool isValid(const base::State *state) const
     {
             return ...;
     }
};
// 或者用这个函数来check:
bool myStateValidityCheckerFunction(const base::State *state)
{
     return ...;
}
 
base::SpaceInformationPtr si(space);
// 如果定义类则通过这一种方式来做可行状态检查
si->setStateValidityChecker(std::make_shared<myStateValidityCheckerClass>(si));
// 如果只定义了上述函数就用这个做可行状态检查
si->setStateValidityChecker(myStateValidityCheckerFunction);
si->setStateValidityCheckingResolution(0.03); // 3%
si->setup();
```

ompl::base::MotionValidator 类定义了 ompl::base::MotionValidator::checkMotion() 方法来评估两个状态之间运动的可行性。默认情况下，这个类的是现实ompl::base::DiscreteMotionValidator，它的优点是可以通过ompl::base::StateValidityChecker中的函数单独实现。缺点是整个motion被离散化为特定的分辨率，然后以这个分辨率做可行状态检测。如果分辨率太大，则可能会有一些不可行的状态检测不到；如果分辨率太小的话，又可能会检测太多的状态，从而影响求解速度。运动离散的分辨率是通过ompl::base::StateSpace::validSegmentCount()计算的：每个状态空间可以计算运动应该被分割为多少段(segment)从而能满足可行状态检测的数量要求。用户可以定义通过调用 ompl::base::SpaceInformation::setStateValidityCheckingResolution()，定义“空间最大范围(ompl::base::StateSpace::getMaximumExtent())的百分比”来定义分辨率；或者通过调用ompl::base::StateSpace::setLongestValidSegmentFraction() 来针对单个的状态空间定义。如果用四大ompl::base::CompoundStateSpace的话，每个子空间可以定义不同的分辨率。如果连续碰撞检测可行，则推荐使用另一个ompl::base::MotionValidator提供的实现方案，这种方案不依赖于按照一定分辨率离散运动。

```cpp
// define this class:
class myMotionValidator : public base::MotionValidator
{
public:
    // implement checkMotion()
};
base::SpaceInformationPtr si(space);
si->setMotionValidator(std::make_shared<myMotionValidator>(si));
si->setup();
```



## 3 Working with States and State Spaces 使用状态和状态空间

### 为状态分配内存

#### 简易版本：

```cpp
ompl::base::StateSpacePtr space(new T());
ompl::base::ScopedState<> state(space);
```

或者

```cpp
ompl::base::SpaceInformationPtr si(space);
ompl::base::ScopedState<T> state(si);
```

ompl::base::ScopedState类会做必需的内存操作来从正确的状态空间中分配一个状态。这是除了ompl内部方法之外的推荐的分配状态的方法。ompl提供了一些方便的运算符，如=和==。如果给定了state space类型T，那么维持的状态被转换为T::StateType。操作符"="会用ompl::base::StateSpace::copyState()，操作符"=="会调用ompl::base::StateSpace::equalStates().

#### 高阶版本

```cpp
ompl::base::SpaceInformationPtr si(space);
ompl::base::State* state = si->allocState();
...
si->freeState(state);
```

一个状态的结构取决于状态空间的定义。ompl::base::State类型只是一个为其他状态空间抽象的基类。因此，状态不能够被直接分配，而是需要通过状态空间的分配机制：ompl::base::StateSpace::allocState()。释放状态可以用ompl::base::SpaceInformation::freeState()。为了方便操作，也定义了 ompl::base::SpaceInformation::allocState() 和 ompl::base::SpaceInformation::freeState()。SpaceInformation这种方式更好，因为能确保是与规划用的状态空间相同。这是最底层的状态操作，只推荐高阶用户使用。



### Working with states

为了让“状态”在设置起点终点的时候更好用，我们需要能够与他的内容交互。这里默认用户了解“为状态分配内存”和“状态与状态空间的操作”（即本篇另外两部分）

#### 简易版本：

状态的推荐用法是用ompl::base::ScopedState。给定一个状态空间，这个类会从这个空间中分配一个状态。当离开这个类的作用域的时候，这个内部维护的状态也就被释放了，ompl::base::ScopedState是一个模板类，继承自T::StateType，其中T是一个状态空间类型。这允许他将维护的状态转换为一个期望的类型，从而展示出作为T::StateType的功能。如果缺省了模板参数，内部的状态被设为ompl::base::State*。

```cpp
ompl::base::StateSpacePtr space(new ompl::base::SE2StateSpace());
ompl::base::ScopedState<ompl::base::SE2StateSpace> state(space);
state->setX(0.1);
state->setY(0.2);
state->setYaw(0.0);

// backup的内容从state拷贝而来，但这里backup的内部状态仍然是State*，因此setX()不能用。
ompl::base::ScopedState<> backup = state;

ompl::base::State *abstractState = space->allocState();

// 这会将abstractState的内容拷贝到state，并内部转换为ompl::base::SE2StateSpace::StateType
state = abstractState;
 
// 把state恢复到最初的状态
state = backup;
 
if (state != backup)
   throw ompl::Exception("This should never happen");
```

将 ompl::base::ScopedState 与 ompl::base::CompoundStateSpace结合使用 

```cpp
ompl::base::CompoundStateSpace *cs = new ompl::base::CompoundStateSpace();
cs->addSubspace(ompl::base::StateSpacePtr(new ompl::base::SO2StateSpace()), 1.0);
cs->addSubspace(ompl::base::StateSpacePtr(new ompl::base::SO3StateSpace()), 1.0);
 
// 将状态空间的指针放到智能指针里
ompl::base::StateSpacePtr space(cs);

// 这里仍然需要手动将状态的内容转换为我们期望的内容，所以其实ompl::base::ScopedState这里只内部转换了一次
ompl::base::ScopedState<ompl::base::CompoundStateSpace> state(space);
state->as<ompl::base::SO2StateSpace::StateType>(0)->setIdentity();
```

上述代码块等价于（其实就是演示了一下复合状态空间的不同构造方式）

```cpp
// 定义单独的状态空间
ompl::base::StateSpacePtr so2(new ompl::base::SO2StateSpace());
ompl::base::StateSpacePtr so3(new ompl::base::SO3StateSpace());
 
// 用重载的+运算符建立一个复合状态空间
ompl::base::StateSpacePtr space = so2 + so3;
 
ompl::base::ScopedState<ompl::base::CompoundStateSpace> state(space);
state->as<ompl::base::SO2StateSpace::StateType>(0)->setIdentity();
```

状态也可以用streams打印出来

```cpp
ompl::base::ScopedState<> state(space);
std::cout << state;
```

有时会需要提取部分状态，或者只给定部分状态，尤其是在使用复合状态空间时。

```cpp
// 一个SE2 state space 其实是R^2 and SO2组成的复合状态空间
ompl::base::StateSpacePtr space(new ompl::base::SE2StateSpace());
// 为状态空间定义一个完整的状态
ompl::base::ScopedState<ompl::base::SE2StateSpace> fullState(space);
// 随即设定状态值
fullState.random();
 
// 建立一个状态对应于SE2的位置分量 
ompl::base::ScopedState<> pos(space->as<ompl::base::SE2StateSpace>()->getSubspace(0));
 
// 复制position
pos << fullState;
 
// 同样，也可以这么做
fullState >> pos;
 
// 如果我们修改了pos，也可以这样反向设置full state
pos >> fullState;
```



#### 高阶版本：

对于一个T类型的状态空间类型，ompl::base::StateSpace::allocState()的结果可以被转换为T::StateType*来获取状态的内部成员变量。为了简化这个用法，定义了ompl::base::State::as()函数

```cpp
ompl::base::StateSpacePtr space(new ompl::base::RealVectorStateSpace(1));
ompl::base::State *state = space->allocState();
state->as<ompl::base::RealVectorStateSpace::StateType>()->values[0] = 0.1;
ompl::base::State *copy = space->allocState();
space->copyState(copy, state);
if (!space->equalStates(copy, state))
   throw ompl::Exception("This should not happen");
space->freeState(state);
space->freeState(copy);
```

更多关于复制状态的信息见[Advanced methods for copying states](https://ompl.kavrakilab.org/group__advancedStateCopy.html) 



### 状态与状态空间的操作

这些操作符是为了简化操作状态和状态空间而定义的。他们依赖于状态空间名字各不相同。下面是一些使用示例

```cpp
// 假设 X, Y, Z, W 为状态空间实例，都不继承自ompl::base::CompoundStateSpace.
// 将复合状态空间表示为 C[...],"..."代表子空间
 
ompl::base::StateSpacePtr X;
ompl::base::StateSpacePtr Y;
ompl::base::StateSpacePtr Z;
ompl::base::StateSpacePtr W;
 
// C1 = C[X, Y]
ompl::base::StateSpacePtr C1 = X + Y;
// C2 = C[X, Y, Z]
ompl::base::StateSpacePtr C2 = C1 + Z;
// C2 = C[X, Y, Z]
ompl::base::StateSpacePtr C2 = C1 + C2;
// C2 = C[X, Y, Z, W]
ompl::base::StateSpacePtr C2 = C2 + W;
// C3 = C[X, Z, Y]
ompl::base::StateSpacePtr C3 = X + Z + Y;
// C4 = C[Z, W]
ompl::base::StateSpacePtr C4 = C2 - C1;
// C5 = W
ompl::base::StateSpacePtr C5 = C2 - C3;
// C6 = C[]
ompl::base::StateSpacePtr C6 = X - X;
// C7 = Y
ompl::base::StateSpacePtr C7 = Y + C6;
```

状态空间可以用于操作状态

```cpp
ompl::base::ScopedState<> sX(X);
ompl::base::ScopedState<> sXY(X + Y);
ompl::base::ScopedState<> sY(Y);
ompl::base::ScopedState<> sZX(Z + X);
ompl::base::ScopedState<> sXZW(X + Z + W);
 
// 将状态sX的内容复制到sXZW对应的位置，而Z和W则不会被影响
sX >> sXZW;
 
// 将状态sXY的X初始化为sXZW的X
sXY << sXZW;
 
// 将状态sZX的X, Z初始化为sXZW的X, Z
sZX << sXZW;

// 对比sX和sY的联结(concatenation)与sXY对比。联结即通过X+Y构造的状态空间，其中的状态包含sX和sY的信息
bool eq = (sX ^ sY) == sXY;
```





## 4 Using existing samplers and creating new ones 使用现有的采样器和创建新的

### 可选择的状态采样器 

有两种采样器听起来相同但扮演着不同的角色：状态空间采样器(ompl::base::StateSampler)和有效状态采样器(ompl::base::ValidStateSampler)。对于每一种状态空间，都需要一个对应的ompl::base::StateSampler类来允许从状态空间均匀采样、在一个状态的附近采样和用高斯分布采样。 有效状态采样器将状态空间采样器作为一个低级的初始值。一般来说，他们用合适的状态空间采样器生成一系列状态采样点直到找到一个有效的状态或者超出最大迭代次数。下面列出了一些预定义的ompl::base::ValidStateSampler类

- ompl::base::UniformValidStateSampler: 这是默认的采样器。他只是调用状态空间采样器来生成均匀的采样直到找到一个有效的状态或者超出最大迭代次数。
- ompl::base::ObstacleBasedValidStateSampler: 这个采样器先试图找到一个无效的sample和一个有效的sample；然后增量式的从有效采样点向无效采样点插值。他返回最后一个有效的状态。这样的采样器的用处在于：在狭窄通道的环境下，障碍物附近会有更大的几率找到有效的采样。在求解运动规划的问题中，寻找这样的采样点一般是关键的问题。
- ompl::base::GaussianValidStateSampler: 这一采样器想解决的问题与上一个相同，但是是以另一种方式。他重复地生成状态对(pairs of states)。第一个是均匀采样随机得到的，而第二个是根据以第一个为中心的高斯分布采样得到的。如果一个采样是有效的而另一个是无效的，那就返回有效的。如果都是有效或都是无效的，则会生成新的状态对，重复直到超过最大迭代次数。
- ompl::base::MaximizeClearanceValidStateSampler: 这个采样器有点像第一个，但一旦他找到一个有效的状态，就会尝试寻找其他有更大的空间(clearance)的有效状态。最后返回的采样是有最大的clearance的状态。

接下来演示了如何定义一个规划器来使用上述的采样器之一和如何写你自己的有效状态采样器。代码样例来自于[StateSampling.cpp](https://ompl.kavrakilab.org/StateSampling_8cpp_source.html)演示程序，该代码也有python版本

### 使用一个已有的采样器

我们不会直接在SimpleSetup或者SpaceInformation类中直接设置采样器的类型，因为每一个线程都需要他自己拥有采样器的备份。我们需要定义ompl::base::ValidStateSamplerAllocator，一个“接收指向 ompl::base::SpaceInformation的指针，返回 ompl::base::ValidStateSamplerPtr”的函数。这个函数也可以在返回之前根据具体的状态空间信息配置有效的状态采样器。下面这个简单的样例演示了如何用ObstacleBasedValidStateSampler

```cpp
namespace ob = ompl::base;
namespace og = ompl::geometric;
ob::ValidStateSamplerPtr allocOBValidStateSampler(const ob::SpaceInformation *si)
{
    // 可以在这里实现任何额外的setup/configuration，但在ObstacleBasedValidStateSampler中没有需要额外配置的，所以这里没有任何额外设置
    return std::make_shared<ob::ObstacleBasedValidStateSampler>(si);
}
```

其他的setup步骤，比如定义起始状态末尾状态被省略掉了。

### 创建一个新的可行状态采样器

现在有很多种启发函数被应用于改进状态的采样。采样的质量可以通过例如与最近障碍物的距离，或者从一个状态的“可见性”来评判。有两种常见的情况可以利用特定问题的信息：

- 如何你用一个返回值是与最近障碍物距离、或者距离函数的梯度的碰撞检测器，那么久需要创建一个新的有效状态采样器来利用这一信息
- 如果你可以直接将状态可行约束融合到采样中（而非用标准有效状态采样器的拒绝采样的方案），那么这将使得算法表现大幅提升。下方给出了示例

下面的代码中我们为一个3D点规划在以原点为中心的立方体中的运动。有一个长方体障碍物。由于状态区域方便描述，我们可以直接在free space采样

```cpp
namespace ob = ompl::base;
namespace og = ompl::geometric;
 
// 这是一个根据问题定制的采样器，自动的生成有效的状态；他不需要调用SpaceInformation::isValid。这是一个受约束采样的例子。如果你可以明确地描述设置的有效状态并可以从中采样，那么这将会比从整个状态空间随机生成采样点再检查可行性更高效。
class MyValidStateSampler : public ob::ValidStateSampler
{
public:
    MyValidStateSampler(const ob::SpaceInformation *si) : ValidStateSampler(si)
    {
        name_ = "my sampler";
    }
    // 在R^3状态空间的有效部分采样
    // 有效的状态满足以下约束
    // -1<= x,y,z <=1
    // if .25 <= z <= .5, then |x|>.8 and |y|>.8
    bool sample(ob::State *state) override
    {
        double* val = static_cast<ob::RealVectorStateSpace::StateType*>(state)->values;
        double z = rng_.uniformReal(-1,1);
 
        if (z>.25 && z<.5)
        {
            double x = rng_.uniformReal(0,1.8), y = rng_.uniformReal(0,.2);
            switch(rng_.uniformInt(0,3))
            {
                case 0: val[0]=x-1;  val[1]=y-1;  break;
                case 1: val[0]=x-.8; val[1]=y+.8; break;
                case 2: val[0]=y-1;  val[1]=x-1;  break;
                case 3: val[0]=y+.8; val[1]=x-.8; break;
            }
        }
        else
        {
            val[0] = rng_.uniformReal(-1,1);
            val[1] = rng_.uniformReal(-1,1);
        }
        val[2] = z;
        assert(si_->isValid(state));
        return true;
    }
    // 在下面的例子中不需要这个
    bool sampleNear(ob::State* /*state*/, const ob::State* /*near*/, const double /*distance*/) override
    {
        throw ompl::Exception("MyValidStateSampler::sampleNear", "not implemented");
        return false;
    }
protected:
    ompl::RNG rng_;
};
```

我们用与上述描述的相似的方法定义一个有效状态分配器

```cpp
ob::ValidStateSamplerPtr allocMyValidStateSampler(const ob::SpaceInformation *si)
{
    return std::make_shared<MyValidStateSampler>(si);
}
```

最后，使用这个采样器

```cpp
ss.getSpaceInformation()->setValidStateSamplerAllocator(allocMyValidStateSampler);
```



## 5 Visualizing solution paths 可视化求解路径

OMPL不包含任何内置可视化工具，但一般用外部软件来可视化不会太难。一般的做法是将路径作为一个矩阵打印，保存为一个text文件，然后在外部程序中打开文件。OMPL中有两种路径： ompl::geometric::PathGeometric 和 ompl::control::PathControl。两个类都有一个方法叫printAsMatrix()。对于 ompl::geometric::PathGeometric而言，矩阵包含了路径上的所有状态，每行对应一个状态。对应 ompl::control::PathControl，每一行也包含controls（控制点？）和从上一个状态到达该状态所需要的控制时间（第一行的控制点和时间都为0）。在 ompl::control::PathControl中，你可能会想转换path为一个集合路径。这个转换自动以一定的步长对路径做插值。基本用法如下。

```cpp
bool solved = ss.solve(20.0);
if (solved)
{
    // ss 为 a ompl::geometric::SimpleSetup object
    ss.getSolutionPath().printAsMatrix(std::cout);
 
    // ss 为 ompl::control::SimpleSetup object
    ss.getSolutionPath().asGeometric().printAsMatrix(std::cout);
}
```

运行代码并保存printAsMatrix()的输出到一个文件，比如path.txt。假设路径保存的是SE(3)状态。这就意味着前三列表示3D位置，另外四列表示四元数姿态。下面我们将演示如何用一些方法可视化路径。

- OMPL.app GUI：OMPL.app GUI可以被用于回放任何SE(2)或SE(3)路径。首先，加载一个表示机器人的mesh和（可加可不加）环境。然后，加载path文件，你可以令GUI循环加载每一个状态或者直接同时显示所有状态。
- Matplotlib：代码见原文
- Matlab / Octave：代码见原文
- Excel：当你用Excel打开path.txt，它会自动引导你把文件加载到spreadsheet里。你需要选择space-delimited data。不幸的是，在Excel里画一个3D曲线似乎比较困难。然而你可以画2D曲线。要画这样一条曲线，选择前两列数据并选择"Insert>Chart..."。然后，点击"Scatter>Straight Marked Scatter"。（适用于Mac 2011, Microsoft Excel；在Windows中菜单选项名称可能有些区别）
- R语言：代码见原文
- gnuplot：代码见原文

在每个位置用一个坐标系表示4-7列代表的姿态信息是十分简单直观的，因此这里留给读者作为练习。



## 6 Benchmarking planner基准规划器

**以下内容为OMPL官方Tutorial [Benchmarking planners](https://ompl.kavrakilab.org/benchmark.html)的翻译**

OMPL 包含一个ompl::Benchmark类用于加速解决运动规划的问题。它会重复地使用不同参数、不同规划器、不同采样器、甚至是同一规划算法的不同配置版本来求解问题。接下来会演示你如何使用这个类。

对于可视化交互benchmark databases, 请见 [plannerarena.org](http://plannerarena.org/).

### 创建一个基准配置文件

OMPL.app包含一个命令行程序 ompl_benchmark，他可以读取一个使用Ini格式键值对的配置文本文件。这与OMPL.app GUI所能够读取和保存的格式是相同的。GUI忽略与benchmarking相关的配置。然而，一般通过创建GUI初始化配置并用文本编辑器添加基准设置的方式更方便。目前ompl_benchmark程序的基本功能仅支持SE(2), SE(3)的集合规划和对一些系统的动力学规划，然而用户可以扩展到其他类型的规划问题。

定义一个问题有一系列*必要*的参数。这些都存在于"\*\*[problem]\*\*"头标识下：

- name：要解决问题的名字
- robot：机器人的几何文件的路径
- start.[x|y|z|theta]，start.axis.[x|y|z]：描述机器人起始状态的值。在2D场景中，方向只需要通过start.theta来确定，在3D场景中则是用的轴角姿态
- goal.[x|y|z|theta]，goal.axis.[x|y|z]：描述机器人目标状态的值。



以下是一些"\*\*[problem]\*\*"头标识下*可选*的参数

- world：环境几何信息的文件路径。默认情况下认为机器人在空的工作空间中运动
- objective：OMPL中的一些规划器可以根据一些优化目标函数来优化路径。objective参数可以设置为length, max_min_clearance或者mechanical_work，分别用于最小化路径长度，最小最大化路径的周围空间，或者路径的机械功。默认值为length
- objective.threshold：如果确定了一个objective，你也可以选择给定一个objective.threshold，也就是当cost满足这个threshold之后就会停止优化。默认情况下为最优的可能值（例如，对于路径长度而言，默认值为0，那么规划器会试图找到最短的路径）
- control：OMPL.app有很少的内置动力学系统。control参数可以被设置为kinematic_car, dynamic_car, blimp和quadrotor。默认情况认为是rigid-body planning。注意，动力学轨迹规划比刚体规划更困难。
- sampler：这个参数确定规划器所用的采样器。可用的采样器有：uniform, gaussian, obstacle_based, max_clearance。默认为uniform。
- volume.[min|max].[x|y|z]：有的时候需要给定工作空间的边界。如果没有给定的话，OMPL.app会设置为包含环境（如果给定了）和起始、目标点的最小的bounding box，不过依赖于环境并不是一个很好的假设。



benchmarking相关的参数必须在"\*\*[benchmark]\*\*"头标识下给出：

- time_limit：规划器最大计算时间
- mem_limit：每个规划器最大的内存空间(MB)。内存测量并不是非常准确，因此推荐设一个比较大的值。
- run_count：每个规划器在规划的时候的最大重复次数。
- output：benchmark的日志文件储存路径。这个参数是可选的，默认为与configuration文件路径相同。
- save_paths：这个参数也是可选的，可以设置为none（默认值）, all或shortest来使其不保存路径，保存所有路径（包括近似的解），或者最短的准确解。这些路径可以通过OMPL.app GUI来回放。

最后一个需要明确的元素是基准规划器。在"\*\*[planner]\*\*"头标识下给定。对于几何规划基准而言，下面这些规划器是可以选择使用的：kpiece, bkpiece, lbkpiece, est, sbl, prm, lazyprm, lazyprmstar, rrt, rrtconnect, lazyrrt, rrtstar, lbtrrt, trrt, spars, spars2, stride, pdst, fmt, 和 aps。对于动力学规划而言，下面这些是可以选择使用的：kpiece, rrt, est, pdst, sycloprrt, 和 syclopest。

一个最小的SE(2)配置，对比rrt和est规划的例子如下：

```cpp
[problem]
name=my_benchmark_problem
robot=my_robot_mesh.dae
start.x=0.0
start.y=0.0
start.theta=0.0
goal.x=1.0
goal.y=1.0
goal.theta=0.0
 
[benchmark]
time_limit=10.0
mem_limit=1000.0
run_count = 3
 
[planner]
est=
rrt=
```

任何这些规划器定义的参数也可以作为benchmark的参数。例如，geometric::RRT规划器定义了两个参数，"range"和"goal_bias"，都为实值。默认值可以通过下面的方式在"planner"头标识下改变：

- rrt.range=50.0
- rrt.goal_bias=0.10

还有很多其他可选的参数可以被改变或者明确。ompl_benchmark利用ompl::base::ParamSet类，可以使用这个方法来设置任何文件中定义的参数。如果一个类给出一个参数的接口，那么就有可能通过config文件来调整它。OMPL.app在benchmark路径中提供两个示例configuration文件 example.cfg和example_complex.cfg来展示这些可选的参数。

可以对同一个规划器创建很多配置不同的实例。下面这段代码演示了创建两个不同参数的rrtconnect的实例：

```cpp
rrtconnect=
rrtconnect.range=100
rrtconnect=
rrtconnect.range=200
```

此外，不同的规划器实例之间问题的设定也可以不同。下例中，第二个kpiece的问题设定就被改变了

```cpp
kpiece=
kpiece=
# 在每一个维度上都增加projection的尺寸为factor倍。
problem.projection.cellsize_factor = 4.0
# 定义一个不同的采样器
problem.sampler=obstacle_based
```

当使用多个planner实例的时候，一个很有用的参数是"name"，因为它可以被用于重命名一个规划器。比如，两个geometric::PRM的实例可以被同时创建但命为不同的名字。有不同的名字在用benchmark script处理日志数据的时候十分有用。

```cpp
prm=
problem.sampler=uniform
prm.name=uniprm
prm=
problem.sampler=obstacle_based
prm.name=obprm
```

最后，如果要执行一个benchmark配置文件，只需要运行OMPL.app bin目录中的ompl_benchmark可执行文件，并提供config文件的路径作为参数即可。



### 写benchmarking代码

在OMPL中，对于一个具体的问题而言，在你的代码中用Benchmark类来Benchmarking一些规划器是十分简单的。需要的步骤如下：

- 用ompl::geometric::SimpleSetup或者ompl::control::SimpleSetup确定benchmark问题
- 创建一个以问题作为输入的ompl::Benchmark object
- （可选）用ompl::Benchmark::addExperimentParameter确定一些benchmark object的参数。当在参数化基准上统计一些基准结果的时候会非常有用。
- 添加一个或多个规划器
- （可选）添加在规划器运行前或后将会被调用的事件
- 在给定时间和内存限制的前提下，运行benchmark问题一定的次数

下面这段代码演示如何实现上述内容，我们会从一些你可能已经用过的代码开始。

```cpp
#include "ompl/tools/benchmark/Benchmark.h"
 
// 一个匹配ompl::base::PlannerAllocator类型的函数，他被分配一个EST的实例
ompl::base::PlannerPtr myConfiguredPlanner(const ompl::base::SpaceInformationPtr &si)
{
    geometric::EST *est = new ompl::geometric::EST(si);
    est->setRange(100.0);
    return ompl::base::PlannerPtr(est);
}
 
// 为我们规划的空间创建一个状态空间
ompl::geometric::SimpleSetup ss(space);
 
// 配置要解决的问题：设置起始状态和目标状态的表示。
// 所有都必须要在 ss.solve() 调用之前设置完成。不需要设置planner
```

Benchmarking代码从这里开始

```cpp
// 首先创建一个benchmark类
ompl::tools::Benchmark b(ss, "my experiment");
 
// 可选内容，确定一些benchmark参数（不会影响benchmark的运行）
b.addExperimentParameter("num_dofs", "INTEGER", "6")
b.addExperimentParameter("num_obstacles", "INTEGER", "10")
 
// 添加需要评估的规划器
b.addPlanner(base::PlannerPtr(new geometric::KPIECE1(ss.getSpaceInformation())));
b.addPlanner(base::PlannerPtr(new geometric::RRT(ss.getSpaceInformation())));
b.addPlanner(base::PlannerPtr(new geometric::SBL(ss.getSpaceInformation())));
b.addPlanner(base::PlannerPtr(new geometric::LBKPIECE1(ss.getSpaceInformation())));
// etc
 
// 对于我们想用特定方式来配置的规划器，需要用到ompl::base::PlannerAllocator
b.addPlannerAllocator(std::bind(&myConfiguredPlanner, std::placeholders::_1));
// etc.
 
// 现在我们可以benchmark了：每个规划计算有5s时间限制，100MB内存限制，50次迭代次数。下面的true代表
// 在计算的时候会显示一个文本模式的进度条
ompl::tools::Benchmark::Request req;
req.maxTime = 5.0;
req.maxMem = 100.0;
req.runCount = 50;
req.displayProgress = true;
b.benchmark(req);
 
// 这会生成一个 ompl_host_time.log 形式的文件
b.saveResultsToFile();
```

也可以为之前或之后的执行内容添加回调函数

```cpp
// 假设下列函数已经被定义了
void optionalPreRunEvent(const base::PlannerPtr &planner)
{
    // 在这里对规划器做如何我们期望的配置，包括通过planner->getProblemDefinition()改变问题定义
}
 
void optionalPostRunEvent(const base::PlannerPtr &planner, tools::Benchmark::RunProperties &run)
{
    // 做一些清理工作，或者为之后的运行设置一些值之类的
 
    // 也可以为收集的运行性质添加元素，添加的数据会记录在日志文件中
 
    run["some extra property name INTEGER"] = "some value";
    // 添加数据的格式是字符串键值对，其中键的最后一个单词是REAL，INTEGRE，BOOLEAN，STRING之一。（这回影响之后日志文件被处理和储存为数据库的类型）。
}
 
// Benchmark类被定义之后，下面这些前面提到的，“之前或之后”发生的事件可以被注册到类中（可选）
b.setPreRunEvent(std::bind(&optionalPreRunEvent, std::placeholders::_1));
b.setPostRunEvent(std::bind(&optionalPostRunEvent, std::placeholders::_1, std::placeholders::_2));
```



### Processing the benchmarking log file

考虑到暂时用不到，这一篇又比较长，后面有需要的时候再翻译









## 7 Optimal Planning 基于优化的规划

**以下内容为OMPL官方Tutorial [Optimal planning](https://ompl.kavrakilab.org/optimalPlanningTutorial.html) 的翻译**

定义一个基于优化的运动规划问题几乎与常规的问题定义一样，除了以下两点区别：

- 需要给ompl::base::ProblemDefinition定义优化目标
- 需要一个优化规划器来做真正的运动规划



### 找寻最短路径

我们将通过一个例子演示OMPL的最优路径规划框架。这个例子中，机器人被表示为一个方形区域上的(x,y)坐标点，其中(0,0)是方形的左下角，(1,1)是方向的右上角。方形区域中有一个中心为(0.5, 0.5)，半径为0.25的障碍物。为了表示这个环境，我们用到了二维ompl::base::RealVectorStateSpace并按照如下方式定义了我们的可行状态检测：

```cpp
// 碰撞检测。在这个例子中，机器人的状态空间位于[0,1]x[0,1]，其中有一个半径为0.25，中心位于(0.5,0.5)的圆。 任何位于这个圆中的状态都认为是碰撞的。
class ValidityChecker : public ob::StateValidityChecker
{
public:
    ValidityChecker(const ob::SpaceInformationPtr& si) :
        ob::StateValidityChecker(si) {}
 
    // 返回给定的状态位置是否与圆形障碍重合
    bool isValid(const ob::State* state) const
    {
        return this->clearance(state) > 0.0;
    }
 
    // 返回给定的状态与原型障碍物的举例
    double clearance(const ob::State* state) const
    {
        // 我们在这个例子中用到的是RealVectorStateSpace，因此我们将state装换为对应的类型
        const ob::RealVectorStateSpace::StateType* state2D =
            state->as<ob::RealVectorStateSpace::StateType>();
 
        // 从状态中提取(x,y)位置坐标
        double x = state2D->values[0];
        double y = state2D->values[1];
 
        // 两点之间距离公式，增加了一个圆圈半径的偏移
        return sqrt((x-0.5)*(x-0.5) + (y-0.5)*(y-0.5)) - 0.25;
    }
};
```

在我们的路径规划代码中，我们定义了状态空间及其边界，和我们的起始及目标状态。在这个例子中，起始状态为(0,0)，目标状态为(1,1)。如果你已经使用过OMPL一般的运动规划代码，那么下面的代码应该对你来说很熟悉。

```cpp
// 创建机器人状态空间[0,1]x[0,1], R^2的子集.
ob::StateSpacePtr space(new ob::RealVectorStateSpace(2));
 
// 设定空间边界 [0,1].
space->as<ob::RealVectorStateSpace>()->setBounds(0.0, 1.0);
 
// 为这个状态空间创建空间信息
ob::SpaceInformationPtr si(new ob::SpaceInformation(space));
 
// 设定有效状态检测函数
si->setStateValidityChecker(ob::StateValidityCheckerPtr(new ValidityChecker(si)));
 
si->setup();
 
// 设置机器人初始位置为(0,0)
ob::ScopedState<> start(space);
start->as<ob::RealVectorStateSpace::StateType>()->values[0] = 0.0;
start->as<ob::RealVectorStateSpace::StateType>()->values[1] = 0.0;
 
// 设置机器人目标位置为(1,1)
ob::ScopedState<> goal(space);
goal->as<ob::RealVectorStateSpace::StateType>()->values[0] = 1.0;
goal->as<ob::RealVectorStateSpace::StateType>()->values[1] = 1.0;
 
// 创建问题实例
ob::ProblemDefinitionPtr pdef(new ob::ProblemDefinition(si));
 
// 设定起点终点
pdef->setStartAndGoalStates(start, goal);
```

接下来，我们需要给优化规划定义一个ompl::base::OptimizationObjective。现在，我们希望定义一个目标函数为起点与终点之间的最短距离。我们将定义另一个函数返回这个目标值

```cpp
// 返回表示最优运动规划的优化目标。这个方法返回一个试图最小化路径的objective。
ob::OptimizationObjectivePtr getPathLengthObjective(const ob::SpaceInformationPtr& si)
{
    return ob::OptimizationObjectivePtr(new ob::PathLengthOptimizationObjective(si));
}
```

然后就可以设置目标函数了

```cpp
pdef->setOptimizationObjective(getPathLengthObjective(si));
```

> 注意：我们所用的优化规划器，ompl::geometric::RRTstar，默认为路径长度优化。也就意味着对于这个例子而言，其实上面的声明是不必要的。但如果你想定义一个其他的优化指标，则需要手动的给ompl::base::ProblemDefinition设定

接下来，我们定义优化规划器并尝试求解问题：

```cpp
// 用RRTstar算法建立优化规划器
ob::PlannerPtr optimizingPlanner(new og::RRTstar(si));
 
// 为我们的规划器设置问题定义
optimizingPlanner->setProblemDefinition(pdef);
optimizingPlanner->setup();
 
// 尝试在1s时间内求解问题
ob::PlannerStatus solved = optimizingPlanner->solve(1.0);
```

这里有一个动图演示RRTstar规划器求解上述定义问题结果（见原文链接）

注意，当试图求解优化问题的时候，我们设定时间限制为1秒。在常规运动规划中，规划器会在找到解后停止，这会用远小于1秒的时间。然而，如果你运行这个例子就会发现他总是使用了完整的1秒时间。这是因为优化求解器有一个更严格的终止条件。常规规划器在找到解之后停止；然而优化规划器需要在找到满足优化目标的解才会停止。



### 定义优化阈值

如何定义满足优化目标？这个行为可以个人定制，但默认是寻找最短的满足一些质量阈值的路径。对于最短路径规划而言，这意味着寻找一条比给定长度更短的路径。你可能会发现我们之前从未定义这个阈值，因为如果没有设置的话，ompl::base::PathLengthOptimizationObjective会默认将threshold设置为0.0。这就意味着只有长度小于0.0的路径才满足目标，那永远不可能满足。这么做的原因是我们假设如果你希望在给定时间内返回找到的最短的路径。因此，threshold设为0.0永远不能被满足，并保证规划器一直运行到时间上限，并返回他能找到的最优路径。

我们可以通过setCostThreshold方法创建一个质量阈值为1.51的OptimizationObjective：

```cpp
ob::OptimizationObjectivePtr getThresholdPathLengthObj(const ob::SpaceInformationPtr& si)
{
    ob::OptimizationObjectivePtr obj(new ob::PathLengthOptimizationObjective(si));
    obj->setCostThreshold(ob::Cost(1.51));
    return obj;
}
```

如果你将这个设置为ompl::base::ProblemDefinition的目标，你会发现规划器会终止得快很多，因为只要他找到一个满足阈值的路径就会返回了。注意当调用ompl::base::OptimizationObjective::setCostThreshold时，我们将阈值放在ompl::base::Cost中。后续会讲解更多关于ompl::base::Cost对象的内容，但现在你可以简单地把他当做一个用double值所表示路径cost的对象。









## 8 Constrained Planning 带约束的规划

**以下内容为OMPL官方Tutorial [Constrained planning](https://ompl.kavrakilab.org/constrainedPlanningTutorial.html) 的翻译**

定义一个受约束的运动规划问题很简单，而且也和无约束规划问题十分相近。主要的区别在于需要定义一个约束和受约束的状态空间的运用。在这个例子中，我们会实现定义一个简单地受约束的规划问题：$R^3$上的一个点别约束在一个球面空间上，给定约束方程为$f(q) = \Vert q \Vert - 1$。这与实例程序[ConstrainedPlanningSphere](https://ompl.kavrakilab.org/ConstrainedPlanningSphere_8cpp_source.html)定义的问题非常接近。



### 定义约束

首先，我们需要定义约束类（constraint class）。约束必须继承自基类ompl::base::Constraint。挂不是故意ompl::base::Constraint::function()必须要由约束的具体实现来施加。如下是一个球面约束的示例版本：

```cpp
// 约束必须继承自基类。默认情况下，会用中心有限差分计算得到约束方程的Jacobian的一个数值逼近。
class Sphere : public ob::Constraint
{
public:
    // ob::Constraint的构造函数需要两个参数，周围状态空间的维数和约束映射到的实向量的空间维数。在我们这个球面的例子中，因为我们规划的是R^3空间，那么第一个参数就是3；我们的约束输出一个实数，那么第二个参数就是1（这也是约束流形的co-dimension）
    Sphere() : ob::Constraint(3, 1)
    {
    }
 
    // 这里我们定义实际的约束函数，它的输入是一些状态"x"（来自于周围的空间），然后将输出的值(out)设置为约束函数的结果。注意我们正在实现具有此函数签名（function signature）的`function`，而不是ompl::base::State的那个
    void function(const Eigen::Ref<const Eigen::VectorXd> &x, Eigen::Ref<Eigen::VectorXd> out) const override
    {
        // 如上所述，定义一个球的函数是f(q) = || q || - 1。Eigen让这个方程易于表达
        out[0] = x.norm() - 1;
    }
};
```

现在我们有一个在R^3空间中定义了一个球面的约束函数。接下来可以用这个约束来定义约束状态空间。



#### 约束Jacobian

然而，我们希望做得更好。我们可以包含一个约束函数的解析的Jacobian，这样可以使规划求解更高效。我们可以手动计算，也可以用一些符号求解器（例如`ConstraintGeneration.py` 描述了如何用 [SymPy](http://www.sympy.org/en/index.html)实现）。因此我们给类添加了如下函数来计算Jacobian

```cpp
// 实现约束函数的Jacobian是一个矩阵，其维度等于约束的co-dimension x 周围环境的维度（此例中为1x3）。与上面的`function`相同，我们用下面的函数签名来实现`jacobian`方法
void Sphere::jacobian(const Eigen::Ref<const Eigen::VectorXd> &x, Eigen::Ref<Eigen::MatrixXd> out) const override
{
    out = x.transpose().normalized();
}
```

一般来说，高度推荐你给带约束的规划问题提供一个解析的Jacobian，尤其是对于高维问题。

#### 投影

ompl::base::Constraint的主要特性之一就是投影函数，ompl::base::Constraint::project()。这个函数的输入为一个可能不满足约束的状态，然后将其映射到约束流形上，生成一个满足约束的状态。默认情况下，ompl::base::Constraint::project()采用牛顿方法，因为他在大多数场景下都很好用。

然而，也可以用你自己的方法覆盖。比如，在这个球形的例子中，可以单纯的归一化状态，将其放置于球面上。另一个例子是对一个复杂的机械臂用逆运动学。

#### 约束参数

ompl::base::Constraint有两个会影响表现的参数。第一个是tolerance，可以通过ompl::base::Constraint::setTolerance()来设置，或者通过构造函数。用构造函数，Spher类可以定义如下：

```cpp
...
    // 设置环境和约束的维数，以及tolerance
    Sphere() : ob::Constraint(3, 1, 1e-3)
...
```

tolerance用于ompl::base::Constraint::project()来判断一个状态是否满足约束，在ompl::bae::Constraint::isSatisfied()中也是同样的作用。低tolerances一般可以简化规划问题，也即约束更容易被满足。

另外一个参数是maxIterations，可以通过ompl::base::ConstraintL::setMaxIterations()设置。maxIterations在无法找到满足条件的情况下，用于限制projection的使用的最大次数。对于容易/不易满足的约束，可能需要刻意调节这一值。

### 定义约束状态空间

#### 周围状态空间

在定义约束状态空间之前，我们需要定义周围的状态空间R^3

```cpp
// 定义状态空间
auto rvss = std::make_shared<ob::RealVectorStateSpace>(3);
 
// 设置空间的边界
ob::RealVectorBounds bounds(3);
bounds.setLow(-2);
bounds.setHigh(2);
 
rvss->setBounds(bounds);
```

周围空间(ambient space)是constraint所在的空间，会被约束状态空间使用

#### 约束实例

接下来需要建立一个约束的实例(constraint instance)，用于后面约束状态空间

```cpp
auto constraint = std::make_shared<Sphere>();
```

#### 约束状态空间

现在我们已经定义了周为状态空间和约束，可以定义约束状态空间了。此例中，我们将用ompl::base::ProjectedStateSpace，他实现了一种基于投影算子的方法来满足约束。然而，我们也可以简单地用其他的约束状态空间，如ompl::base::AtlasStateSpace或ompl::base::TangentBundleStateSpace来解决这个问题。我们也需要创建一个ompl::base::ConstrainedSpaceInformation，即带约束的ompl::base::SpaceInformation。

```cpp
// 将周围空间和约束放在一起得到约束状态空间。
auto css = std::make_shared<ob::ProjectedStateSpace>(rvss, constraint);
 
// 为这个约束状态空间定义约束空间信息
auto csi = std::make_shared<ob::ConstrainedSpaceInformation>(css);
```

ompl::base::ConstrainedSpaceInformation所做的最重要的事情就是调用ompl::base::ConstrainedStateSpace::setSpaceInformation，这个函数允许流形遍历与离散测地线计算一起进行碰撞检测。现在，我们有一个约束的状态空间和我们规划的空间的信息。



### 定义问题

现在我们为这个约束的空间定义一个简单地问题来求解：从球面的南极点向北极点穿越，且要必考赤道上的一个简易的障碍。用约束框架定义一个问题与定义无约束问题一样简单，也用的是一样的工具。例如，我们将创建一个ompl::geometric::SimpleSetup来帮助定义问题

```cpp
auto ss = std::make_shared<og::SimpleSetup>(csi);
```

#### 可行状态检测

定义一个可行状态检测器，这里的障碍物是赤道上一条带有洞的细长带（图见原文对应位置，看图更易理解）

```cpp
bool obstacle(const ob::State *state)
{
    // 因为ob::ConstrainedStateSpace::StateType继承自Eigen::Map<Eigen::VectorXd>，我们可以使用引用来简化获取状态
    const Eigen::Map<Eigen::VectorXd> &x = *state->as<ob::ConstrainedStateSpace::StateType>();
 
    // 或者，我们可以用如下命令来获得潜在的实向量状态：
    //   auto x = state->as<ob::ConstrainedStateSpace::StateType>()->getState()->as<ob::RealVectorStateSpace::StateType>();
    // 注意，我们对受约束的状态用"getState()"。这里获取的潜在的状态时周围环境所分配的
 
    // 定义一个细长的障碍带，其中一侧有一个洞
    if (-0.1 < x[2] && x[2] < 0.1)
    {
        if (-0.05 < x[0] && x[0] < 0.05)
            return x[1] < 0;
 
        return false;
    }
 
    return true;
}
 
// 在simple setup中设定state validity checker
ss->setStateValidityChecker(obstacle)
```

#### 起点终点

接下来设置起点终点，即球的南北极。注意对于约束问题而言，起点和终点必须满足约束函数，如果不满足就会出报错。

```cpp
// Start and goal vectors.
Eigen::VectorXd sv(3), gv(3);
sv << 0, 0, -1;
gv << 0, 0,  1;
 
// 我们将加入simple setup中的Scoped states
ob::ScopedState<> start(css);
ob::ScopedState<> goal(css);
 
// 将vector中的值复制到起始状态和目标状态中
start->as<ob::ConstrainedStateSpace::StateType>()->copy(sv);
goal->as<ob::ConstrainedStateSpace::StateType>()->copy(gv);
 
// 如果我们用 Atlas 或者 TangentBundleStateSpace，我们还需要锚定这些状态到charts中
//   css->anchorChart(start.get());
//   css->anchorChart(goal.get());
// 他们给atlas一个起点来生长
 
ss->setStartAndGoalStates(start, goal);
```

#### 规划器

最后，我们可以加入一个规划器。这里使用ompl::geometric::PRM，但其他规划器也是同样可以的。

```cpp
auto pp = std::make_shared<og::PRM>(csi);
ss->setPlanner(pp);
```

### 求解问题

现在我们已经准备好做规划了。

```cppp
ss->setup();
```

求解的过程也和无约束问题相类似。我们这里给求解器5s时间求解。

```cpp
ob::PlannerStatus stat = ss->solve(5.);
if (stat)
{
    // 用带约束的状态空间时路径简化同样有效
    ss->simplifySolution(5.);
 
    // 得到求解路径
    auto path = ss->getSolutionPath();
 
    // 插值对于约束状态空间同样有些，而且一般是必须的
    path.interpolate();
 
    // 像往常一样做任何你想对路径做的处理！
}
else
    OMPL_WARN("No solution found!");
```

#### 插值

注意，一般而言你求解的初始路径并非连续的。也就是说状态之间的距离可能很远。如果你希望路径有非常近的，满足中间约束的状态，你需要对路径做插值。在上述代码中，这是通过ompl::geometric::PathGeometric::interpolate()实现的。

### 总结

现在已经求解了一个球面上的运动规划问题。PRM运行后的结果可能看起来如下图所示（见原文），简化后的解路径已经黄色高亮显示。

整体而言，带约束的规划用起来十分简单。除了需要定义约束函数和将周围空间整合到一个约束状态空间中，OMPL的其他配置并没有变化。你可以在[main page](https://ompl.kavrakilab.org/constrainedPlanning.html)阅读更多关于带约束的规划。



## 9 QuotientSpace Planning 商空间的规划

**以下内容为OMPL官方Tutorial [QuotientSpace planning](https://ompl.kavrakilab.org/quotientSpacePlanningTutorial.html) 的翻译**

本篇教程描述QuatientSpace Planning任务与普通的配置空间规划的区别。最后，我们展示了如何从QuotientSpace planner总提取PlannerData

### 常规的配置空间规划

OMPL中一个常规的规划问题通过如下几个类定义

- ompl::base::SpaceInformationPtr，配置空间
- ompl::base::ProblemDefinitionPtr，起点和终点配置
- ompl::base::PlannerPtr，所用的规划器

我们建立并求解规划问题如下

```cpp
ompl::base::SpaceInformationPtr si;
ompl::base::ProblemDefinitionPtr pdef; 
 
//...
// 给pdef设置起点和终点状态
//...
 
ompl::base::PlannerPtr planner = std::make_shared<ompl::geometric::RRT>(si);
planner->setProblemDefinition(pdef);
planner->setup();
planner->solve();
```

### 商空间规划

在一个商空间规划的设置中，我们添加了一个有限的商空间，可以通过算法计算（如果支持的话）

因此需要额外的配置：

- std::vector<ompl::base::SpaceInformationPtr>，商空间。按照升序的顺序排序，最后一个元素需要是配置空间
- ompl::geometric::QRRT，一个具体的商空间的规划器，继承自ompl::geometric::QuotientSpace

于是一个规划问题可以按照如下方式求解

```cpp
std::vector<ompl::base::SpaceInformationPtr> siVec; 
ompl::base::ProblemDefinitionPtr pdef; 
 
//...
// 在siVec中构建商空间（见下）, 给pdef设置起点和终点状态
//...
 
auto planner = std::make_shared<og::QRRT>(siVec);
 
planner->setProblemDefinition(pdef);
planner->setup();
planner->solve();
```

### 构建商空间

接下来在SE(2)配置空间中为二维刚体建立两个商空间。这个示例可以在demos/QuotientSpacePlanningRigidBody2D.cpp文件中看到。

首先，我们需要为每个商空间创建SpaceInformationPtr。本例中，我们选择两层{R^2, SE(2)}。这个空间的边界应该是单位圆[0, 1]^2

```cpp
auto SE2(std::make_shared<ob::SE2StateSpace>());
ob::RealVectorBounds bounds(2);
bounds.setLow(0);
bounds.setHigh(1);
SE2->setBounds(bounds);
ob::SpaceInformationPtr si_SE2(std::make_shared<ob::SpaceInformation>(SE2));
 
auto R2(std::make_shared<ob::RealVectorStateSpace>(2));
R2->setBounds(0, 1);
ob::SpaceInformationPtr si_R2(std::make_shared<ob::SpaceInformation>(R2));
```

其次，我们需要为每个SpaceInformationPtr设置validitychecker。这是通过Validity函数来判断状态是否可行实现的。

对于SE(2)约束，我们定义所有的位于"以(0.5,0.5)为圆心，0.2为半径"的圆之，且小于$\frac{\pi}{2}$的旋转都是可行的。

```cpp
bool boxConstraint(const double values[])
{
    const double &x = values[0]-0.5;
    const double &y = values[1]-0.5;
    double pos_cnstr = sqrt(x*x + y*y);
    return (pos_cnstr > 0.2);
}
bool isStateValid_SE2(const ob::State *state) 
{
    const auto *SE2state = state->as<ob::SE2StateSpace::StateType>();
    const auto *R2 = SE2state->as<ob::RealVectorStateSpace::StateType>(0);
    const auto *SO2 = SE2state->as<ob::SO2StateSpace::StateType>(1);
    return boxConstraint(R2->values) && (SO2->value < boost::math::constants::pi<double>() / 2.0);
}
```

商空间的有效性函数可以通过例如移除一些约束来构建。在实际实验中，我们经常从机器人中删除一些关节或部分关节，这隐含地删除了约束。

```cpp
bool isStateValid_R2(const ob::State *state) 
{ 
    const auto *R2 = state->as<ob::RealVectorStateSpace::StateType>();
    return boxConstraint(R2->values);
}
```

最后，我们需要设置有效性函数并将SpaceInformationPtr放到向量中。注意SpaceInformationPtr需要根据维度来按照升序排列。

```cpp
si_SE2->setStateValidityChecker(isStateValid_SE2);
si_R2->setStateValidityChecker(isStateValid_R2);
 
siVec.push_back(si_R2);
siVec.push_back(si_SE2);
```

注意：QuotientSpace规划器的运行时间主要取决于QuotientSpace定义的顺序。对于一些空间，规划可能很快完成，而对于其他的可能还不如常规的规划器如ompl::geometric::RRT(即在单个配置空间中运行ompl::geometric::RRT)。哪个空间更好仍然是一个开放式的问题。但一般而言，空间中的狭小通道越多，则就应该用更多的商空间。更多信息请见 [QRRT paper](https://arxiv.org/abs/1906.01350)。



### AnnotatedPlannerDataVertex

定义了一个QuotientSpace规划器之后，我们可以像一般的ompl::base::PlannerPtr一样使用它，也就是说我们可以把它加入benchmark，然后得到规划数据。

然而，常规的PlannerData数据结构并没有QuatientSpaces。为了添加这一信息，我们使用了一个新的类ompl::base::PlannerDataVertexAnnotated，继承自PlannerDataVertex。这个类加入了一些新的方法：

- getLevel()，返回当前定点所在的QuotientSpace层级
- getMaxLevel()，返回QuotientSpace层级的数量
- getQuotientState()，返回当前定点所在层级的一个QuotientSpace状态
- getState()，返回初始配置空间中的状态（这与常规的PlannerData类是一样的）

```cpp
#include <ompl/geometric/planners/quotientspace/PlannerDataVertexAnnotated.h>
 
PlannerDataVertexAnnotated *v = dynamic_cast<PlannerDataVertexAnnotated*>(&pd->getVertex(0));
if(v!=nullptr){
    unsigned int level = v->getLevel();
    unsigned int maxLevel = v->getMaxLevel();
    const ob::State *s_QuotientSpace = v->getQuotientState();
    const ob::State *s_ConfigurationSpace = v->getState();
}
```

更多信息请见[QuotientSpaces](https://ompl.kavrakilab.org/quotientSpacePlanning.html) 或者 [demos](https://ompl.kavrakilab.org/group__demos.html).



# OMPL进阶教程：

















# OMPL教程高阶：