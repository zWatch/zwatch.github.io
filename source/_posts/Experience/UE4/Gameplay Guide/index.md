# Gameplay Guide

## Overviews and examples of gameplay functionality for programmers and visual scripters.

[Unreal Engine 4.9](https://docs.unrealengine.com/en-US/SiteIndex/index.html?versions=4_9)

## Basic Gameplay Concepts

These pages will introduce you to key Unreal terms. To add new types of gameplay objects, you will generally create a new class. A class is a template, or collection of rules, for your new object, so that you can create as many copies as you need, each containing properties and behavior that you set in the template.
这些页面将向您介绍一些重要的虚幻术语。 要添加新类型的游戏对象，通常将创建一个新类。 类是新对象的模板或规则集合，因此您可以创建所需数量的副本，每个副本都包含您在模板中设置的属性和行为。

1. Unreal Projects and Gameplay 
Introduction to projects, levels, classes, and Actors in Unreal Engine.   
虚幻项目和游戏玩法虚幻引擎中的项目，级别，类和演员的简介。[here](https://docs.unrealengine.com/en-US/Gameplay/UnrealTerminology/index.html)  

2. Class Creation Basics
Examples showing how to create classes with Blueprints alone, C++ alone, and a combination of C++ and Blueprints.  
“类创建基础示例”显示了如何使用单独的蓝图，单独的C ++以及C ++和蓝图的组合创建类的示例。[here](https://docs.unrealengine.com/en-US/Gameplay/ClassCreation/index.html)

## Framework

In Unreal Engine 4, there are a number of classes that come with preset behavior to help you get started with your game. Read more about these building blocks and how they work together in the framework overview or quick reference, or jump straight to a particular class page for more information.  
在虚幻引擎4中，预设行为具有许多类，可以帮助您开始游戏。 在框架概述或快速参考中了解有关这些构建块以及它们如何协同工作的更多信息，或者直接跳到特定的类页面以获取更多信息。

  0. **CameraAnims**  
  CameraAnims allow you to layer on animations to your camera to simulate impact, motion in your environment, and other effects.  
  CameraAnims允许您在相机的动画上分层以模拟冲击，环境中的运动以及其他效果。[here](https://docs.unrealengine.com/en-US/Gameplay/Framework/Camera/Animations/index.html)
 
 0. **AIController**  
  The AIController observes the world around it and makes decisions and reacts accordingly without human player input.
  观察周围的世界并做出决策并做出相应的反应，而无需人工输入。[here](https://docs.unrealengine.com/en-US/Gameplay/Framework/Controller/AIController/index.html)

 0. **PlayerController**  
  The PlayerController implements functionality for taking the input data from the player and translating that into actions, such as movement, using items, firing weapons, etc.  
  PlayerController实现了以下功能：从玩家那里获取输入数据，并将其转换为动作，例如移动，使用物品，发射武器等。[here](https://docs.unrealengine.com/en-US/Gameplay/Framework/Controller/PlayerController/index.html)

 0. **Character**  
  A Character is a Pawn which has some basic bipedal movement functionality by default.  
  角色是 Pawn ，默认情况下具有一些基本的两足动物移动功能。[here](https://docs.unrealengine.com/en-US/Gameplay/Framework/Pawn/Character/index.html)

 

 0. **Gameplay Framework Quick Reference**  
  Short overview of classes for game rules, characters, controllers, user interfaces, etc., that make up the framework for the game.  
  构成游戏框架的游戏规则，角色，控制器，用户界面等的类的简短概述。[here](https://docs.unrealengine.com/en-US/Gameplay/Framework/QuickReference/index.html)

 0. **Game Flow Overview**  
The process of starting the engine and launching a game or play-in-editor session.  
游戏流程概述启动引擎，启动游戏或编辑中游戏会话的过程。[here](https://docs.unrealengine.com/en-US/Gameplay/Framework/GameFlow/index.html)

 0. **Game Mode and Game State**  
  Overview of the Game Mode and Game State  
  游戏模式和游戏状态概述 [here](https://docs.unrealengine.com/en-US/Gameplay/Framework/GameMode/index.html)

 0. **Pawn**  
  The Pawn is the physical representation of a player within the world.  
  Pawn是世界上玩家的物理表示。[here](https://docs.unrealengine.com/en-US/Gameplay/Framework/Pawn/index.html)

 

 0. **Controller**  
  In the context of a player or AI entity, the Controller is essentially the brain.  
  在玩家或AI实体的上下文中，控制器本质上是大脑。[here](https://docs.unrealengine.com/en-US/Gameplay/Framework/Controller/index.html)

 

 0. **Camera**  
  The Camera represents the player's point of view; how the player sees the world.  
  摄影机代表玩家的观点； 玩家如何看待世界。[here](https://docs.unrealengine.com/en-US/Gameplay/Framework/Camera/index.html)

 

 0. **User Interfaces & HUDs**  
  Guides and information for artists and programmers creating user interfaces such as menus and HUDs.  
  用于艺术家和程序员创建菜单和HUD等用户界面的指南和信息。[here](https://docs.unrealengine.com/en-US/Gameplay/Framework/UIAndHUD/index.html)

 

 0. **Gameplay Framework**  
  Core systems, such as game rules, player input and controls, cameras, and user interfaces.  
  核心系统，例如游戏规则，玩家输入和控件，摄像头和用户界面。[here](https://docs.unrealengine.com/en-US/Gameplay/Framework/index.html)

## Gameplay Elements

Although each game may have different characters, rules, and visual styles, there are some core elements that are more universal. From input to saving, find overviews of these topics and example setups in C++ and/or Blueprints here.  
尽管每个游戏可能具有不同的角色，规则和视觉样式，但其中一些核心元素更为通用。 从输入到保存，请在此处找到这些主题的概述以及C ++和/或“蓝图”中的示例设置。

 0. **Input**  
  The Input object is responsible for converting input from the player into data in a form Actors can understand and make use of.  
  Input对象负责将播放器的输入转换为Actor可以理解和使用的格式的数据。[here ](https://docs.unrealengine.com/en-US/Gameplay/Input/index.html)

 0. **Networking and Multiplayer**  
  Setting up networked games for multiplayer.  
  设置多人网络游戏。[here](https://docs.unrealengine.com/en-US/Gameplay/Networking/index.html)

 0. **Saving and Loading**   
Your GameOverview of how to save and load your game.  
您的游戏概述如何保存和加载游戏 [here](https://docs.unrealengine.com/en-US/Gameplay/SaveGame/index.html)

 0. **Data Driven Gameplay Elements**  
Driving gameplay elements using externally stored data.  
使用外部存储的数据来驱动游戏元素。[here](https://docs.unrealengine.com/en-US/Gameplay/DataDriven/index.html)

 0. **AI and Behavior Trees**  
Information over Artificial Intelligence including the use of Behavior Trees, EQS and Perception.  
人工智能方面的信息，包括行为树，EQS和感知的使用。
[here](https://docs.unrealengine.com/en-US/Gameplay/AI/index.html)

 0. **Localization**
Information about how to localize your project.  
有关如何本地化项目的信息。[here](https://docs.unrealengine.com/en-US/Gameplay/Localization/index.html)

 0. **In-Game Analytics**  
Using in-game analytics to track player engagement and find balance issues.  
使用游戏内分析来跟踪玩家参与度并发现平衡问题。 [here](https://docs.unrealengine.com/en-US/Gameplay/Analytics/index.html)

 0. **Gameplay Tags**  
Gameplay Tags can be used to identify, categorize, match, and filter objects.  
游戏标记可用于识别，分类，匹配和过滤对象。[here](https://docs.unrealengine.com/en-US/Gameplay/Tags/index.html)

## Gameplay Tools

 0. **Gameplay Debugger**
Tool that enables analyzing realtime gameplay data at runtime.  
该工具可在运行时分析实时游戏数据。 [here](https://docs.unrealengine.com/en-US/Gameplay/Tools/GameplayDebugger/index.html)

 0. **Network Profiler**  
Tool for displaying network traffic and performance information captured at runtime.  
用于显示运行时捕获的网络流量和性能信息的工具。 [here](https://docs.unrealengine.com/en-US/Gameplay/Tools/NetworkProfiler/index.html)

 0. **Visual Logger**  
Tool that captures state from actors and then displays it visually in game or editor.  
捕获演员状态，然后在游戏或编辑器中直观显示状态的工具。[here](https://docs.unrealengine.com/en-US/Gameplay/Tools/VisualLogger/index.html)

## Gameplay How To's

This section provides step-by-step instructions on how to generate some of the most common gameplay scenarios. Whether it is how to spawn enemies or pick-ups in your game, change camera angles, or set up player inputs; these pages provide example setups for you in both Blueprints and C++.  
本节提供有关如何生成一些最常见的游戏场景的分步说明。 无论是在游戏中生成敌人或拾取物品，更改摄影机角度还是设置玩家输入的方式； 这些页面为您提供了蓝图和C ++中的示例设置。

 0. **入门**  
    + [**Input 输入** **4.22**](https://docs.unrealengine.com/en-US/Gameplay/Input/index.html)  
    + [**Saving and Loading Your Game 保存和加载** **4.22**](https://docs.unrealengine.com/en-US/Gameplay/SaveGame/index.html)   
    + [**Spawning/Destroying an Actor Overview 生成/销毁角色概述** **4.9**](https://docs.unrealengine.com/en-US/Gameplay/HowTo/SpawnAndDestroyActors/index.html)  
    + [**Using Cameras 使用相机** **4.9**](https://docs.unrealengine.com/en-US/Gameplay/HowTo/UsingCameras/index.html)  

 0. **进阶**    
    + [**RawInput Plugin RawInput插件** **4.16**](https://docs.unrealengine.com/en-US/Gameplay/Input/RawInput/index.html)  
    + [**Possessing Pawns 大概是让Pawns运行起来** **4.9**](https://docs.unrealengine.com/en-US/Gameplay/HowTo/PossessPawns/index.html)
    + [**Using Timers 使用计时器** **4.9**](https://docs.unrealengine.com/en-US/Gameplay/HowTo/UseTimers/index.html)  
    + [**Setting Up Inputs 设置输入** **4.9**](https://docs.unrealengine.com/en-US/Gameplay/HowTo/SetUpInput/index.html)  
    + [**Setting Up Input on an Actor 在Actor上设置输入** **4.9**](https://docs.unrealengine.com/en-US/Gameplay/HowTo/ActorInput/index.html)  
    + [**Setting Up Character Movement 设置角色移动** **4.9**](https://docs.unrealengine.com/en-US/Gameplay/HowTo/CharacterMovement/index.html)  
    + [**RawInput Plugin RawInput插件** **4.16**](https://docs.unrealengine.com/en-US/Gameplay/Input/RawInput/index.html)  
    + [**Finding Actors 查找Actors** **4.9**](https://docs.unrealengine.com/en-US/Gameplay/HowTo/FindingActors/index.html)  
    + [**Using the OnHit Event 使用OnHit事件** **4.9**](https://docs.unrealengine.com/en-US/Gameplay/HowTo/UseOnHit/index.html)  

 0. **高级**  
    + [**Respawning a Player 重生玩家** **4.9**](https://docs.unrealengine.com/en-US/Gameplay/HowTo/RespawnPlayer/index.html)  
    + [**Referencing Actors 引用Actors** **4.9**](https://docs.unrealengine.com/en-US/Gameplay/HowTo/ReferenceAssets/index.html)  
    + [**Setting Up a Game Mode 设置游戏模式** **4.9**](https://docs.unrealengine.com/en-US/Gameplay/HowTo/SettingUpAGameMode/index.html)