# 现代造型:艾导演

> 原文：<https://medium.com/geekculture/modelling-a-modern-day-pandemic-director-ai-608768484eb7?source=collection_archive---------35----------------------->

提高基于代理的模型的性能的一种方法是从代理中卸载所有的逻辑，并将它们放入一个单一的对象中，即控制器 ai

![](img/86ee29396a118f659c102ffd565d4360.png)

Photo by [Pietro Jeng](https://unsplash.com/@pietrozj?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们从什么是导演艾这个问题开始。艾导演的概念在 2009 年由 valve 软件和 turtle rock 工作室制作的《死亡之路 4》中首次使用，在导演将使用:党的状态，党是如何分裂的，党的整体技能以及有多少感染目前在地图上；确定它的位置:被感染的贮藏物，补给，特殊感染以及地图的结构。这创造了数小时的重放能力，也是 L4D 至今仍被演奏的原因。第二个例子可以在《毁灭战士 2016》中找到，导演控制一次可以攻击你的敌人数量，这种实现更类似于基于代理的模型中使用的实现。

# **导演艾**

那么你如何着手构建一个 director AI 呢，首先(和 C++中的所有类一样)我们需要一个头文件和一个 cpp 文件。

在我们的头文件中，我们还需要几个库，以及上一篇文章中的代理文件

```
/*HEADER FILE*/#include "Agent.h"#include <map>
#include <algorithm>
#include <memory>class Director
{
};------------
/*CPP FILE*/#include "Director.h"
```

为了保持模型的高性能，我们需要为每个人设置强制任务:

*   睡着的
*   去上班/上学
*   回家
*   无所事事

然而，要设置强制任务，需要创建任务的概念

```
#include <tuple>
#include <vector>struct Task
{
enum Destination_Types
{
    Hospitals = 0, POW, Restaurant, Cinema, Shopping_center, Parks
};
    std::tuple<int, int, int> location;
    std::tuple<Public_Buildings*, Education_Buildings*, Public_transport_building*, House*, Generic_work*> location_type;
    Destination_Types destination;
    unsigned int task_length = 0; //counts
    unsigned int run_time = 0;
};
```

*   结构与类的不同之处在于，除非指定，否则所有内容都是公共的，而在类中，除非指定，否则所有内容都是私有的

director 还需要跟踪所有活动任务，其中有多少任务是非强制性的，以及每次计数有多少任务失败(这更多是为了调试，但允许 director 从列表中删除它们)，并且最重要的是，director 必须跟踪每个代理处于什么状态。

```
#include "Agent.h"
#include "task.h"#include <map>
#include <algorithm>
#include <memory>class Director
{
public:
    enum class mandatory_task
    {
        sleep = 0, go_to_work, go_home, idle
    };

    int failed = 0;
    bool mandatory_tasks = false;

    std::vector<std::tuple<Task*, Actor*, int>> m_current_tasks;
    unsigned int max_actors_not_idle = 5000; std::vector<Actor*> m_outside;
    std::vector<Actor*> m_idle;
    std::vector<Actor*> m_transit;
    std::vector<Actor*> m_doing_task;};
```

导演人工智能也将需要跟踪人口，但是这将是一个私人变量，所以它不能在初始设置之外编辑

```
#include "Agent.h"
#include "task.h"#include <map>
#include <algorithm>
#include <memory>class Director
{
public:
    enum class mandatory_task
    {
        sleep = 0, go_to_work, go_home, idle
    };

    int failed = 0;
    bool mandatory_tasks = false;

    std::vector<std::tuple<Task*, Actor*, int>> m_current_tasks;
    unsigned int max_actors_not_idle = 5000;std::vector<Actor*> m_outside;
    std::vector<Actor*> m_idle;
    std::vector<Actor*> m_transit;
    std::vector<Actor*> m_doing_task;
private:
    std::vector<Actor*> m_population;
};
```

导演人工智能的全部要点是卸载代理的逻辑，所以在方法方面，我们需要添加什么:

*   任务权限
*   生成任务
*   任务设置
*   一种用于处理请求的系统
*   回家功能
*   去医院功能
*   世界任务
*   运行任务

director 还需要一些方法来实现其中的一些方法

*   在任务向量中找到特定任务的方法
*   确定生成哪个任务的方法
*   一种清除任务向量的方法
*   一种自动更改代理位置的方法

在析构函数和构造函数方面，我们将使用一个自定义的构造函数和默认的析构函数。

在我们开始添加这些方法之前，我们需要修改代理类，以便 director 可以执行它的检查来确定代理是否可以执行任务。这些检查是:

*   代理是否空闲
*   代理人不在途中吗
*   还有足够的任务预算吗
*   任务的位置是否合适(未关闭，未满负荷)
*   该代理四处移动是否安全(非确诊病例)

为了让导演把他们送到正确的地方，我们还需要存储他们的工作地点和家的位置，并使导演成为代理的朋友。我们将通过在代理类中创建两个新类来实现这一点(不过这些将在以后被删除),它们在这里只是为了演示。

```
#include <random>
#include <vector>
#include <string>
#include <chrono>class House
{
};class Place_of_work
{
};class Director
{
};class Agent 
{
   friend class Director; 
public:
    enum infection_status
    {
        suceptible = 0, infected, removed, dead
    };   
    enum State
    {
        idle = 0, transit, doing_task, waiting
    };
    enum Location
    {
         home = 0, work, public_building, in_transit, outside, unknown
    };
    int infection_length = 0;
    State current_state = idle;
    Location current_location = unknown; House* home = NULL; //NULL pointer means it has no memory adress
    Place_of_work* work = NULL;private:
    int m_x = 0;
    int m_y = 0;    

    infection_status m_stage = suceptible;
    bool m_infectious = false; double m_infection_chance = 0;
    double m_recovery_chance = 0;
    double m_dying_chance = 0;
private:
    std::vector<unsigned int> Discrete_distribution(std::vector<double>& weights, unsigned int runs);
public:
    void update_location(int x_loc, int y_loc);
    void update_infec_stat(infection_status stage);

    bool check_infectious();
    bool check_recovered();    

    void set_infection_chance(double chance);
    void set_recovery_chance(double chance);
    void set_dying_chance(double chance);};
```

现在代理类已经准备好了，我们可以开始向 director 类添加功能了，但是由于需要其他系统的代码，并不是所有的功能都会实现。

```
#include "Agent.h"
#include "task.h"#include <map>
#include <algorithm>
#include <memory>class Director
{
public:
    enum class mandatory_task
    {
        sleep = 0, go_to_work, go_home, idle
    };

    int failed = 0;
    bool mandatory_tasks = false;
    bool sleep_active = false;
    std::vector<std::tuple<Task*, Actor*, int>> m_current_tasks;
    unsigned int max_actors_not_idle = 5000;std::vector<Actor*> m_outside;
    std::vector<Actor*> m_idle;
    std::vector<Actor*> m_transit;
    std::vector<Actor*> m_doing_task;
private:
    std::vector<Actor*> m_population;
private:
    bool task_permission(Agent::State state);
    unsigned int generate_random_task(Actor::Age_Catagory age_cat, Tasks::Time_of_day time); //Not going to be written
    std::tuple<std::tuple<int, int, int>, Public_Buildings*> public_task_setup(int& actor_tile, Task::Destination_Types location_type);
    bool find_in_vector(const std::vector<Actor*>& vector, int& position, const Actor* value);
    std::vector<double> task_weight_vector(const std::vector<std::tuple<Task::Destination_Types, std::vector<Tasks::Time_of_day>>>& task_vector, const Tasks::Time_of_day& time);
    void clear_task_vector();
public:
    Director(std::vector<Agent*>& population, std::vector<Tile*>& tiles, Matrix<int>& tile_matrix, Transport_Net& net); //Not going to be written
    void request_task(Agent* requestee);
    void go_home(Agent* requestee);
    void world_task(mandatory_task task, int length);
    void run_tasks();
    void go_to_hospital(std::vector<agent*>& patients);
    void change_actor_location(agent* actor, Task task, bool task_end);
};
```

由于组成所有这些方法的代码量太大，超出了本文的范围，所以我只介绍其中的一些方法。

# 世界任务

```
void Director::world_task(mandatory task task, int length);
{
    clear_task_vector();
    mandatorytask = true;
    if (task_type != mandatory_task::sleep)
    {
        for (auto actor : m_population)
        {
             if (task_type == mandatory_task::go_home)
             {
                 Task* task = new Task;
                 task->location_type = { NULL, NULL, NULL, actor->Home, NULL };
                 task->location = actor->House_Location();
                 task->task_length = length;
                 int delay = Random::random_number(0, 10, {}); 
                 actor->set_state(Actor::waiting);
                 m_current_tasks.push_back({ task, actor, delay });
             }
             else if (task_type == mandatory_task::go_to_work)
             {
                 Task* task = new Task;
                 task->task_length = length;
                 task->loacation_type = { NULL, NULL, NULL, NULL, actor->Place_of_work};
                  task->location = actor->Place_of_work->locatio();
                  int delay = Random::random_number(0, 10, {});
                  std::tuple<int, int, int> null_tuple = { 0,0,0 };
                  if (task->location != null_tuple)
                  {
                      actor->set_state(Actor::waiting);
                      m_current_tasks.push_back({ task, actor, delay });
                  }
                  else
                  {
                  delete task;
                  }
             }
             else if (task_type == mandatory_task::idle)
             {
                 mandatorytask = false;
                 if (actor->state_check() != actor::idle)
                 {
                     actor->set_state(actor::idle);
                     actor->idle_counts = 0;
                     if (actor->state_check() == Actor::in_transit && m_transit.size() != 0)
                     {
                         find_in_vector(m_transit, x, actor);
                         m_transit.erase(m_transit.begin() + x);
                         m_idle.push_back(actor);
                     }
                     if (actor->state_check() == Actor::doing_task && m_doing_task.size() != 0)
                     {
                         find_in_vector(m_doing_task, x, actor);
                         m_doing_task.erase(m_doing_task.begin() + x);
                         m_idle.push_back(actor);
                     }  
                 }
             }
         }
      }
 else if (task_type == mandatory_task::sleep)
 {
     mandatorytask = false;
     if (sleep_active == false)
     {
         sleep_active = true;
     }
     else
     {
         sleep_active = false;
     }
 }
}
```

*   演员=代理人
*   其中的一些函数目前还没有出现在本文所涉及的代码中

正如你所看到的，对于一个非常简单的功能来说，有很多代码，而我只是刚刚触及到 ai 导演能做什么。再次完整的代码可以在这里找到[https://github.com/bptigg/CovidSim](https://github.com/bptigg/CovidSim)。

在下一篇文章中，我打算介绍导演如何使用世界和代理信息来决定什么任务最适合他们去做。