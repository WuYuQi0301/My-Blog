---
title: Unity Simple Patrol
date: 2018-05-10 10:08:29
categories:
- Unity
---

发布-订阅模式实战。

# 游戏规则及设计要求
**游戏设计要求**
 + 创建一个地图和若干巡逻兵；
 + 每个巡逻兵走一个3~5个边的凸多边型，位置数据是相对地址。即每次确定下一个目标位置，用自己当前位置为原点计算；
 + 巡逻兵碰撞到障碍物如树，则会自动选下一个点为目标；
 + 巡逻兵在设定范围内感知到玩家，会自动追击玩家；
 + 失去玩家目标后，继续巡逻；
 + 计分：每次甩掉一个巡逻兵计一分，与巡逻兵碰撞游戏结束；
**程序设计要求**
 + 必须使用订阅与发布模式传消息
 + 工厂模式生产巡逻兵

# 类图
![UML](http://i2.bvimg.com/618639/da05db0090d7d9a9.png)
# 游戏实现
## 发布-订阅
### 概念
发布订阅模式实现：一个对象发生改变的时候将通知其他对象，其他对象将相应做出反应。其中这样有几个角色：发生改变的对象称为观察对象或者发布者，被通知的对象称为观察者或者订阅者。  
一个发布者可以被多个订阅者订阅，且发布者并不需要知道谁是它的订阅者，从而可以形成多对多关系的解耦。
### 应用
在巡逻兵游戏中，我们希望碰撞事件发生之后，场景控制器能够做出反应，故设“碰撞事件发生”为发布的信号，“FirstController”为订阅者；为了更好地集成各种事件，我们设置中间件，游戏事件管理器“GameEventManager”来向FirstController发出信号。  

在C#中有一种特殊的变量**delegate**，意味“委托”。它有些类似c++中的指针，一种存有对某个方法引用的引用类型变量，特别用于实现事件和回调。  
发布订阅模式中经常使用事件（event）来使用委托。  
事件在发布者 GameEventManager处声明并生成，并通过 GameEventManager类中的委托与事件处理器FirstController关联。
```c#
//class GameEventManager
    public delegate void ScoreEvent();
    public static event ScoreEvent ScoreChange;

    public delegate void GameoverEvent();
    public static event GameoverEvent GameoverChange;
```

```c#
//class FirstController
    GameEventManager.ScoreChange += AddScore;
    GameEventManager.GameoverChange += Gameover;
```


<iframe width="660" height="455" src="http://v.youku.com/v_show/id_XMzYwMDMzMzM0OA==.html?spm=a2h3j.8428770.3416059.1" frameborder="0" allowfullscreen></iframe>

巡逻兵代码Github [传送门](https://github.com/WuYuQi0301/Unity-Game-Programming/tree/patrol/Assets)  
大一学c++时候fork的[design_pattern](https://github.com/WuYuQi0301/design_patterns)
