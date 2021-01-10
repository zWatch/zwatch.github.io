# Introduction to C++ Programming in UE4

UE4中的C ++编程简介

## Introductory guide for C++ programmers new to Unreal Engine

[Unreal Engine 4.9](https://docs.unrealengine.com/en-US/SiteIndex/index.html?versions=4_9)

![image alt text](https://docs.unrealengine.com/Images/Programming/Introduction/image_0.jpg)

## Unreal C++ Is Awesome!

This guide is about learning how to write C++ code in Unreal Engine 4 (UE4). Don't worry, C++ programming in Unreal Engine is fun, and actually not hard to get started with! We like to think of Unreal C++ as "assisted C++", because we have so many features to help make C++ easier for everyone.
本指南旨在学习如何在虚幻引擎4（UE4）中编写C ++代码。 不用担心，虚幻引擎中的C ++编程很有趣，而且实际上并不难入门！ 我们喜欢将Unreal C ++视为“辅助C ++”，因为我们有许多功能可以帮助所有人简化C ++。

Before we go on, it's really important that you are already familiar with C++ or another programming language. This page is written with the assumption that you have some C++ experience, but if you know C#, Java, or JavaScript, you should find many aspects familiar.
在我们继续之前，您已经熟悉C ++或另一种编程语言非常重要。 在编写此页面时，假定您具有一定的C ++经验，但是如果您了解C＃，Java或JavaScript，则应该熟悉许多方面。

If you are coming in with no programming experience at all, we have you covered also! Check out our [Blueprint Visual Scripting guide](https://docs.unrealengine.com/en-US/Engine/Blueprints/index.html) and you will be on your way. You can create entire games using Blueprint scripting!
如果您完全没有编程经验，那么我们也能满足您的要求！ 请查看我们的[Blueprint Visual Scripting guide](https://docs.unrealengine.com/en-US/Engine/Blueprints/index.html)，您将一路走来。 您可以使用Blueprint脚本创建整个游戏！

It is possible to write standard C++ code in UE4, but you will be most successful after reading through this guide and learning the basics about the Unreal programming model. We will talk more about that as we go along.
可以在UE4中编写标准的C ++代码，但是在通读本指南并学习了有关虚幻编程模型的基础知识之后，您将获得最大的成功。 我们将继续讨论。

## C++ and Blueprint

UE4 provides two methods, C++ and Blueprint Visual Scripting, to create new gameplay elements. Using C++, programmers add the base gameplay systems that designers can then build upon or with to create the custom gameplay for a level or the game. In these cases, the C++ programmer works in a text editor (like Notepad++) or an IDE (usually Microsoft Visual Studio, or Apple's Xcode) and the designer works in the Blueprint Editor within UE4.
UE4提供了两种方法来创建新的游戏元素，即C ++和Blueprint Visual Scripting。 程序员使用C ++添加基本的游戏系统，设计人员可以在其上或之上构建基础游戏系统，以为关卡或游戏创建自定义游戏。 在这些情况下，C ++程序员在文本编辑器（例如Notepad ++）或IDE（通常是Microsoft Visual Studio或Apple的Xcode）中工作，而设计器在UE4中的蓝图编辑器中工作。

The gameplay API and framework classes are available to both of these systems, which can be used separately, but show their true power when used in conjunction to complement each other. What does that really mean, though? It means that the engine works best when programmers are creating gameplay building blocks in C++ and designers take those blocks and make interesting gameplay.
这两个系统都可以使用游戏性API和框架类，它们可以单独使用，但在结合使用时可以发挥真正的威力。 不过，这到底是什么意思？ 这意味着，当程序员使用C ++创建游戏性构建基块并且设计人员采用这些块并进行有趣的游戏性时，该引擎最有效。

With that said, let us take a look at a typical workflow for the C++ programmer that is creating building blocks for the designer. In this case, we are going to create a class that is later extended via Blueprints by a designer or programmer. In this class, we are going to create some properties that the designer can set and we are going to derive new values from those properties. The whole process is very easy to do using the tools and C++ macros we provide for you.
话虽如此，让我们看一看C ++程序员的典型工作流程，该工作流程正在为设计人员创建构件。 在这种情况下，我们将创建一个类，稍后由设计人员或程序员通过蓝图对其进行扩展。 在此类中，我们将创建一些设计人员可以设置的属性，并从这些属性中获取新的值。 使用我们为您提供的工具和C ++宏，整个过程非常容易做到。

### Class Wizard 类向导

The first thing we're going to do is use the Class Wizard within the Editor to generate the basic C++ class that will be extended by Blueprint later. The image below shows the wizard's first step where we are creating a new Actor.
我们要做的第一件事是使用编辑器中的类向导生成基本的C ++类，稍后将由Blueprint对其进行扩展。 下图显示了向导的第一步，我们正在其中创建新的Actor。

![image alt text](https://docs.unrealengine.com/Images/Programming/Introduction/image_1.jpg)

The second step in the process tells the wizard the name of the class you want generated. Here's the second step with the default name used.
该过程的第二步告诉向导您要生成的类的名称。 这是使用默认名称的第二步。

![image alt text](https://docs.unrealengine.com/Images/Programming/Introduction/image_2.jpg)

Once you choose to create the class, the wizard will generate the files and open your development environment so that you can start editing it. Here is the class definition that is generated for you. For more information on the Class Wizard, follow [this link.](https://docs.unrealengine.com/en-US/Programming/Development/ManagingGameCode/CppClassWizard/index.html)
一旦选择创建类，向导将生成文件并打开您的开发环境，以便您可以开始对其进行编辑。 这是为您生成的类定义。 有关类向导的更多信息，请单击[此链接。](https://docs.unrealengine.com/zh-CN/Programming/Development/ManagingGameCode/CppClassWizard/index.html)

```c++
#include "GameFramework/Actor.h"
#include "MyActor.generated.h"

UCLASS()
class AMyActor : public AActor
{
    GENERATED_BODY()

public:
    // Sets default values for this actor's properties
    AMyActor();

    // Called every frame
    virtual void Tick( float DeltaSeconds ) override;

protected:
    // Called when the game starts or when spawned
    virtual void BeginPlay() override;
};
```

The Class Wizard generates your class with `BeginPlay` and `Tick` specified as overloads. `BeginPlay` is an event that lets you know the Actor has entered the game in a playable state. This is a good place to initiate gameplay logic for your class. `Tick` is called once per frame with the amount of elapsed time since the last call passed in. You can do any recurring logic there. However, if you do not need that functionality, it is best to remove it to save yourself a small amount of performance. If you remove it, make sure to remove the line in the constructor that indicated ticking should occur. The constructor below contains the line in question.
类向导使用指定为重载的“ BeginPlay”和“ Tick”生成您的类。 “ BeginPlay”是一个事件，可让您知道Actor已进入可玩状态。 这是为您的班级启动游戏逻辑的好地方。 每隔一帧调用一次“滴答”，其间隔为自从上次调用传入以来经过的时间。您可以在其中执行任何重复逻辑。 但是，如果不需要该功能，则最好将其删除以节省少量性能。 如果删除它，请确保删除构造函数中指示应该出现滴答声的行。 下面的构造函数包含有问题的行。

```c++
AMyActor::AMyActor()
{
    // Set this actor to call Tick() every frame.  You can turn this off to improve performance if you do not need it.
    PrimaryActorTick.bCanEverTick = true;
}
```

### Making a Property Show up in the Editor 在编辑器中显示属性

We have our class, so now we can create some properties that designers can set in the Editor. Exposing a property to the Editor is easy with the `UPROPERTY` Specifier. All you have to do is put `UPROPERTY(EditAnywhere)` on the line above your property declaration, as seen in the class below.
我们有我们的课程，所以现在我们可以创建一些属性，设计人员可以在编辑器中设置这些属性。 使用`UPROPERTY`说明符可以很容易地将属性显示给编辑器。 您所要做的就是在属性声明上方的行中放置`UPROPERTY（EditAnywhere）`，如下面的类所示。

```c++
UCLASS()
class AMyActor : public AActor
{
    GENERATED_BODY()
public:
    UPROPERTY(EditAnywhere)
    int32 TotalDamage;
    ...
};
```

That is all you need to do to be able to edit that value in the Editor. There are more ways to control how and where it is edited. This is done by passing more information into the `UPROPERTY()` Specifier. For instance, if you want the TotalDamage property to appear in a section with related properties, you can use the categorization feature. The property declaration below shows this.
这是您能够在编辑器中编辑该值所需要做的一切。 有更多方法可以控制其编辑方式和位置。 这是通过将更多信息传递到`UPROPERTY()`说明符中来完成的。 例如，如果希望TotalDamage属性显示在具有相关属性的部分中，则可以使用分类功能。 下面的属性声明显示了这一点。

```xml
UPROPERTY(EditAnywhere, Category="Damage")
int32 TotalDamage;
```

When the user looks to edit this property, it now appears under the Damage heading along with any other properties that you have marked with this category name. This is a great way to place commonly used settings together for editing by designers.
现在，当用户希望编辑此属性时，它会与您使用此类别名称标记的所有其他属性一起显示在“损害”标题下。 这是将常用设置放在一起以供设计人员编辑的好方法。

Now let's expose that same property to Blueprint.
现在，让我们将相同的属性公开给Blueprint。

```xml
UPROPERTY(EditAnywhere, BlueprintReadWrite, Category="Damage")
int32 TotalDamage;
```

As you can see, there is a Specifier to make a property available for reading and writing in Blueprint graphs. There's a separate Specifier, `BlueprintReadOnly`, that you can use if you want the property to be treated as `const` in Blueprints. There are quite a few options available for controlling how a property is exposed to the Editor. To see more options, follow [this link.](https://docs.unrealengine.com/en-US/Programming/UnrealArchitecture/Reference/Properties/Specifiers/index.html)
如您所见，有一个说明符，使属性可用于在蓝图图中进行读写。 如果希望将该属性在“蓝图”中视为“ const”，则可以使用一个单独的“ BlueprintReadOnly”说明符。 有许多选项可用于控制如何向编辑器公开属性。 要查看更多选项，请点击[此链接。](https://docs.unrealengine.com/zh-CN/Programming/UnrealArchitecture/Reference/Properties/Specifiers/index.html)

Before continuing to the section below, let's add a couple of properties to this sample class. There is already a property to control the total amount of damage this actor will deal out, but let us take that further and make that damage happen over time. The code below adds one designer settable property and one that is visible to the designer but not changeable by them.
在继续下面的部分之前，让我们为该示例类添加几个属性。 已经有一个属性可以控制此演员要造成的伤害总额，但是让我们进一步加以考虑，使伤害随着时间的推移发生。 下面的代码添加了一个设计器可设置的属性，以及一个对设计器可见但不可更改的属性。

```c++
UCLASS()
class AMyActor : public AActor
{
    GENERATED_BODY()
public:
    UPROPERTY(EditAnywhere, BlueprintReadWrite, Category="Damage")
    int32 TotalDamage;
    UPROPERTY(EditAnywhere, BlueprintReadWrite, Category="Damage")
    float DamageTimeInSeconds;
    UPROPERTY(BlueprintReadOnly, VisibleAnywhere, Transient, Category="Damage")
    float DamagePerSecond;
    ...
};
```

`DamageTimeInSeconds` is a property the designer can modify. The `DamagePerSecond` property is a calculated value using the designer's settings (see the next section). The `VisibleAnywhere` Specifier marks that property as viewable but not editable. The `Transient` Specifier means that it won't be saved or loaded from disk; it is meant to be a derived, non-persistent value, so there is no need to store it. The image below shows the properties as part of the class defaults.
`DamageTimeInSeconds` 是设计者可以修改的属性。 `DamagePerSecond` 属性是使用设计者的设置计算得出的值（请参阅下一节）。 `VisibleAnywhere` 说明符将该属性标记为可见，但不可编辑。 “瞬态”说明符意味着它不会被保存或从磁盘加载； 它是一个派生的非持久值，因此不需要存储它。 下图显示了作为类默认值一部分的属性。

![image alt text](https://docs.unrealengine.com/Images/Programming/Introduction/image_3.jpg)

### Setting Defaults in the Constructor

在构造函数中设置默认值

Setting default values for properties in a constructor works the same as your typical C++ class. Below are two examples of setting default values in a constructor and are equivalent in functionality.
在构造函数中为属性设置默认值的工作原理与典型的C ++类相同。 下面是在构造函数中设置默认值的两个示例，它们在功能上等效。

```c++
AMyActor::AMyActor()
{
    TotalDamage = 200;
    DamageTimeInSeconds = 1.0f;
}

AMyActor::AMyActor() :
    TotalDamage(200),
    DamageTimeInSeconds(1.0f)
{
}
```

Here is the same view of the properties after adding default values in the constructor.
这是在构造函数中添加默认值之后的属性相同视图。

![image alt text](https://docs.unrealengine.com/Images/Programming/Introduction/image_4.jpg)

In order to support per-instance designer-set properties, values are also loaded from the instance data for a given object. This data is applied after the constructor. You can create defaults based off of designer-set values by hooking into the `PostInitProperties()` call chain. Here is an example of that process where `TotalDamage` and `DamageTimeInSeconds` are designer-specified values. Even though these are designer specified, you can still provide sensible default values for them, as we did in the example above.
为了支持按实例设计者设置的属性，还从实例数据中为给定对象加载值。 此数据在构造函数之后应用。 您可以通过挂钩`PostInitProperties（）`调用链来基于设计者设置的值创建默认值。 这是该过程的一个示例，其中 `TotalDamage` 和 `DamageTimeInSeconds` 是设计者指定的值。 即使这些是设计者指定的，您仍然可以为它们提供合理的默认值，就像我们在上面的示例中所做的那样。

If you do not provide a default value for a property, the engine will automatically set that property to zero, or null in the case of pointer types.
如果您没有为属性提供默认值，则引擎将自动将该属性设置为零，对于指针类型则为null。

```c++
void AMyActor::PostInitProperties()
{
    Super::PostInitProperties();
    DamagePerSecond = TotalDamage / DamageTimeInSeconds;
}
```

Here is the same view of the properties after we have added the `PostInitProperties()` code that you see above.
在添加上面您看到的`PostInitProperties（）`代码之后，这是属性的相同视图。

![image alt text](https://docs.unrealengine.com/Images/Programming/Introduction/image_5.jpg)

### Hot Reloading 热装(热加载)

Here is a cool feature of Unreal that you might be surprised about if you are used to programming C++ in other projects. You can compile your C++ changes without shutting down the Editor! There are two ways to do this:
这是虚幻引擎的一个很酷的功能，如果您习惯在其他项目中编程C ++，可能会感到惊讶。 您可以编译C ++更改而无需关闭编辑器！ 有两种方法可以做到这一点：

1. With the Editor still running, go ahead and build from Visual Studio or Xcode like you normally would. The Editor will detect the newly compiled DLLs and reload your changes instantly!
   在编辑器仍在运行的情况下，像往常一样从Visual Studio或Xcode进行构建。 编辑器将检测到新编译的DLL，并立即重新加载您的更改！

   ![image alt text](https://docs.unrealengine.com/Images/Programming/Introduction/image_6.jpg)

   If you are attached with the debugger, you'll need to detach first so that Visual Studio will allow you to build.
   如果您连接了调试器，则需要先进行分离，以便Visual Studio可以进行构建。

2. Or, simply click the **Compile** button on the Editor's main toolbar.或者，只需单击编辑器主工具栏上的**编译**按钮。

   ![image alt text](https://docs.unrealengine.com/Images/Programming/Introduction/image_7.jpg)

You can use this feature in the sections below as we advance through the tutorial.
随着本教程的逐步进行，您可以在以下各节中使用此功能。

### Extending a C++ Class via Blueprints 通过蓝图扩展C ++类

So far, we have created a simple gameplay class with the C++ Class Wizard and added some properties for the designer to set. Now, let's take a look at how a designer can start creating unique classes from our humble beginnings here.
到目前为止，我们已经使用C ++类向导创建了一个简单的游戏类，并添加了一些属性供设计人员设置。 现在，让我们来看看设计师如何从我们谦虚的起点开始创建独特的类。

The first thing we are going to do is create a new Blueprint class from our `AMyActor` class. Notice in the image below that the name of the base class selected shows up as **MyActor** instead of **AMyActor**. This is intentional and hides the naming conventions used by our tools from the designer, making the name friendlier to them.
我们要做的第一件事是从我们的AMyActor类创建一个新的Blueprint类。 请注意，在下图中，所选基类的名称显示为 **MyActor ** 而不是 **AMyActor **。 这是有意的，并向设计者隐藏了我们工具使用的命名约定，从而使名称更友好。

![image alt text](https://docs.unrealengine.com/Images/Programming/Introduction/image_8.jpg)

Once you choose **Select**, a new default named Blueprint class is created for you. In this case, I set the name to **CustomActor1** as you can see in the snapshot of the **Content Browser** below.
选择 **Select ** 之后，将为您创建一个新的默认名称Blueprint类。 在这种情况下，我将名称设置为 **CustomActor1** ，如下面的 **内容浏览器** 快照中所见。

![image alt text](https://docs.unrealengine.com/Images/Programming/Introduction/image_9.jpg)

This is the first class that we are going to customize with our designer hats on. First thing we are going to do is change the default values for our damage properties. In this case, the designer changed the `TotalDamage` to 300 and the time it takes to deliver that damage to 2 seconds. This is how the properties now appear.
这是我们要戴上设计帽进行定制的第一堂课。 我们要做的第一件事是更改损坏属性的默认值。 在这种情况下，设计人员将“ TotalDamage”更改为300，将这种损坏所花费的时间更改为2秒。 现在，属性就是这样显示的。

![image alt text](https://docs.unrealengine.com/Images/Programming/Introduction/image_10.jpg)

Our calculated value does not match what we would expect. It should be 150 but it is still at the default value of 200. The reason for this is that we are only calculating our damage per second value after the properties have been initialized from the loading process. Runtime changes in the Editor are not accounted for. There is a simple solution to this problem because the Engine notifies the target object when it has been changed in the Editor. The code below shows the added hooks needed to calculate the derived value as it changes in the Editor.
我们的计算值与我们的预期不符。 应该为150，但仍为默认值200。这样做的原因是，我们仅在从加载过程初始化属性后才计算每秒的损坏值。 不考虑编辑器中的运行时更改。 有一个简单的解决方案，因为在编辑器中更改目标对象后，引擎会通知目标对象。 下面的代码显示了在编辑器中更改派生值时所需的添加的挂钩。

```
void AMyActor::PostInitProperties()
{
    Super::PostInitProperties();

    CalculateValues();
}

void AMyActor::CalculateValues()
{
    DamagePerSecond = TotalDamage / DamageTimeInSeconds;
}

#if WITH_EDITOR
void AMyActor::PostEditChangeProperty(FPropertyChangedEvent& PropertyChangedEvent)
{
    CalculateValues();

    Super::PostEditChangeProperty(PropertyChangedEvent);
}
#endif
```

One thing to note is that the `PostEditChangeProperty` method is inside an Editor-specific `#ifdef`. This is so that building your game only compiles the code that you actually need, removing any extra code that might increase the size of your executable unnecessarily. Now that we have that code compiled in, the `DamagePerSecond` value matches what we would expect it to be, as seen in the image below.

![image alt text](https://docs.unrealengine.com/Images/Programming/Introduction/image_11.jpg)

### Calling Functions across the C++ and Blueprint Boundary

So far, we have shown how to expose properties to Blueprints, but there is one last introductory topic that we should cover before you dive deeper into the engine. Whilee creating gameplay systems, designers will need to be able to call functions created by a C++ programmer. The programmer will also need to be able to call functions implemented in Blueprints from C++ code. Let us start by first making the CalculateValues() function callable from Blueprints. Exposing a function to Blueprints is just as simple as exposing a property. It takes only one macro placed before the function declaration! The code snippet below shows what is needed for this.

```
UFUNCTION(BlueprintCallable, Category="Damage")
void CalculateValues();
```

The `UFUNCTION()` macro handles exposing the C++ function to the reflection system. The `BlueprintCallable` option exposes it to the Blueprint virtual machine. Every Blueprint exposed function requires a category associated with it, so that the right click context menu works properly. The image below shows how the category affects the context menu:

![image alt text](https://docs.unrealengine.com/Images/Programming/Introduction/image_12.jpg)

As you can see, the function is selectable from the **Damage** category. The Blueprint code below shows a change in the TotalDamage value followed by a call to recalculate the dependent data.

![image alt text](https://docs.unrealengine.com/Images/Programming/Introduction/image_13.jpg)

This uses the same function that we added earlier to calculate our dependent property. Much of the engine is exposed to Blueprints via the **UFUNCTION()** macro, so that people can build games without writing C++ code. However, the best approach is to use C++ for building base gameplay systems and performance critical code with Blueprints used to customize behavior or create composite behaviors from C++ building blocks.

Now that designers can call our C++ code, let us explore one more powerful way to cross the C++/Blueprint boundary. This approach allows C++ code to call functions that are defined in Blueprints. We often use the approach to notify designers of an event that they can respond to as they see fit. Often that includes the spawning of effects or other visual impact, such as hiding or unhiding an actor. The code snippet below shows a function that is implemented by Blueprints.

```
UFUNCTION(BlueprintImplementableEvent, Category="Damage")
void CalledFromCpp();
```

This function is called like any other C++ function. Under the covers, the Unreal Engine generates a base C++ function implementation that understands how to call into the Blueprint VM. This is commonly referred to as a Thunk. If the Blueprint in question does not provide a function body for this method, then the function behaves just like a C++ function with no body behaves: it does nothing. What if you want to provide a C++ default implementation while still allowing a Blueprint to override the method? The UFUNCTION() macro has an option for that too. The code snippet below shows the changes needed in the header to achieve this.

```
UFUNCTION(BlueprintNativeEvent, Category="Damage")
void CalledFromCpp();
```

This version still generates the thunking method to call into the Blueprint VM. So how do you provide the default implementation? The tools also generate a new function declaration that looks like `<function name>_Implementation()`. You must provide this version of the function or your project will fail to link. Here is the implementation code for the declaration above.

```
void AMyActor::CalledFromCpp_Implementation()
{
    // Do something cool here
}
```

Now this version of the function is called when the Blueprint in question does not override the method. Note that in previous versions of the build tools, the _Implementation() declaration was automatically generated. In versions 4.8 and up, you must explicitly add that to the header.

Now that we have walked through the common gameplay programmer workflow and methods to work with designers to build out gameplay features, it is time for you to choose your own adventure. You can either continue with this document to read more about how we use C++ in the Engine, or you can jump right into one of our samples that we include in the launcher to get a more hands-on experience.

## Diving Deeper

I see you are still with me on this adventure. Excellent. The next topics of discussion revolve around what our gameplay class hierarchy looks like. In this section, we'll start with the base building blocks and talk through how they relate to each other. This is where we'll look at how the Unreal Engine uses both inheritance and composition to build custom gameplay features.

### Gameplay Classes: Objects, Actors, and Components

There are 4 main class types that you derive from for the majority of gameplay classes. They are `UObject`, `AActor`, `UActorComponent`, and `UStruct`. Each of these building blocks are described in the following sections. Of course, you can create types that do not derive from any of these classes, but they will not participate in the features that are built into the engine. Typical use of classes that are created outside of the `UObject` hierarchy are: integrating 3rd party libraries, wrapping of OS specific features, and so on.

#### Unreal Objects (UObject)

The base building block in the Engine is called `UObject`. This class, coupled with `UClass`, provides a number of the Engine's most important services:

- Reflection of properties and methods
- Serialization of properties
- Garbage collection
- Finding a `UObject` by name
- Configurable values for properties
- Networking support for properties and methods

Each class that derives from `UObject` has a singleton `UClass` created for it that contains all of the metadata about the class instance. `UObject` and `UClass` together are at the root of everything that a gameplay object does during its lifetime. The best way to think of the difference between a `UClass` and a `UObject` is that the `UClass` describes what an instance of a `UObject` will look like, what properties are available for serialization, networking, and so on. Most gameplay development does not involve directly deriving from `UObject`, but instead from AActor and UActorComponent. You do not need to know the details of how `UClass` or `UObject` works in order to write gameplay code, but it is good to know that these systems exist.

#### AActor

An `AActor` is a `UObject` that is meant to be part of the gameplay experience. Actors are either placed in a level by a designer or created at runtime via gameplay systems. All objects that can be placed into a level extend from this class. Examples include `AStaticMeshActor`, `ACameraActor`, and `APointLight`. Since `AActor` derives from `UObject`, it enjoys all of the standard features listed in the previous section. Actors can be explicitly destroyed through gameplay code (C++ or Blueprints) or by the standard garbage collection mechanism when the owning level is unloaded from memory. Actors are responsible for the high-level behaviors of your game's objects. `AActor` is also the base type that can be replicated during networking. During network replication, Actors can also distribute information for any `UActorComponents` that they own and that require network support or synchronization.

Actors have their own behaviors (specialization through inheritance), but they also act as containers for a hierarchy of Actor Components (specialization through composition). This is done through the Actor's `RootComponent` member, which contains a single `USceneComponent` that, in turn, can contain many others. Before an Actor can be placed in a level, it must contain at least one Scene Component, from which the Actor will draw its translation, rotation, and scale.

Actors have a series of events that are called during their lifecycles. The list below is a simplified set of the events that illustrate the lifecycle:

- `BeginPlay`: Called when the Actor first comes into existence during gameplay.
- `Tick`: Called once per frame to do work over time.
- `EndPlay`: Called when the object is leaving the gameplay space.

See [Actors](https://docs.unrealengine.com/en-US/Programming/UnrealArchitecture/Actors/index.html) for a more detailed discussion on the `AActor` class.

##### Runtime Lifecycle

Just above, we discussed a subset of an Actor's lifecycle. For Actors that are placed in a level, understanding the lifecycle is pretty easy to imagine: Actors are loaded and come into existence and eventually the level is unloaded and the Actors are destroyed. Spawning an actor is a bit more complicated than creating a normal object in the game, because Actors need to be registered with a variety of runtime systems in order to serve all of their needs. The initial location and rotation for an Actor need to be set. Physics may need to know about it. The manager responsible for telling an Actor to tick needs to know. And so on. Because of this, we have a method devoted to the spawning of an Actor, `SpawnActor` (a member of `UWorld`). When the Actor spawns successfully, the Engine will call its `BeginPlay` method, followed by `Tick` on the next frame.

Once an Actor has lived out its lifetime, you can get rid of it by calling `Destroy`. During that process, `EndPlay` will run, enabling you to perform custom logic before the Actor goes to garbage collection. Another option for controlling how long an Actor exists is to use the `Lifespan` member. You can set an amount of time in the Actor's constructor or with other code at runtime. Once that amount of time has expired, the Actor will automatically have `Destroy` called on it.

To learn more about spawning Actors see the [Spawning Actors](https://docs.unrealengine.com/en-US/Programming/UnrealArchitecture/Actors/Spawning/index.html) page.

#### UActorComponent

Actor Components (class `UActorComponent`) have their own behaviors and are usually responsible for functionality that is shared across many types of Actors, such as providing visual meshes, particle effects, camera perspectives, and physics interactions. While Actors are often given high-level goals related to their overall roles your game, Actor Components usually perform the individual tasks that support those higher-level objectives. Components can also be attached to other Components, or can be the root Component of an Actor. A Component can only attach to one parent Component or Actor, but it may have many child Components attached to itself. Picture a tree of Components. Child Components have location, rotation, and scaling relative to their parent Component or Actor.

While there are many ways to use Actors and Components, one way to think of the Actor-Component relationship is that Actors might answer the question, "What is this thing?" while Components might answer, "What is this thing made of?"

- `RootComponent` — this is the member of `AActor` that holds the top level Component in the Actor's tree of Components
- Ticking — Components are ticked as part of the owning Actor's `Tick` function. (Be sure to call `Super::Tick` when writing your own `Tick` function.)

##### Dissecting the First Person Character

In order to illustrate the relationship between an `AActor` and its `UActorComponents`, let us dig into the Blueprint that is created when you generate a new project based off of the First Person Template. The image below is the Component tree for the **FirstPersonCharacter** Actor. The **RootComponent** is the **CapsuleComponent**. Attached to the **CapsuleComponent** is the **ArrowComponent**, the **Mesh** component, and the **FirstPersonCameraComponent**. The leaf Component is "Mesh1P", which is parented to the **FirstPersonCameraComponent**, meaning that the first person mesh is relative to the first person camera.

![image_14.png](https://docs.unrealengine.com/Images/Programming/Introduction/image_14.jpg)

Visually, this tree of Components looks like the image below, where you see all of the Components in 3D space except for the **Mesh** component.

![image_15.png](https://docs.unrealengine.com/Images/Programming/Introduction/image_15.jpg)

This tree of Components is attached to the one Actor class. As you can see from this example, you can build complex gameplay objects using both inheritance and composition. Use inheritance when you want to customize an existing `AActor` or `UActorComponent`. Use composition when you want many different `AActor` types to share the functionality.

#### UStruct

To use a `UStruct`, you do not have to extend from any particular class, you just have mark the struct with USTRUCT() and our build tools will do the base work for you. Unlike a `UObject`, `UStruct` instances are not garbage collected. If you create dynamic instances of them, you must manage their lifecycle yourself. A `UStruct` should be a plain data type that has `UObject` reflection support for editing within the Unreal Editor, Blueprint manipulation, serialization, networking, and so on.

Now that we have talked about the basic hierarchy used in our gameplay class construction, it is time to choose your path again. You can read about our gameplay classes [here](https://docs.unrealengine.com/en-US/Programming/UnrealArchitecture/Reference/Classes/index.html) , head out to our samples in the launcher armed with more information, or continue digging deeper into our C++ features for building games.

## Diving Deeper Still

Alright, it is clear you want to know more. Let us keep on going deeper into how the engine works.

### Unreal Reflection System

[Blog Post: Unreal Property System (Reflection)](https://www.unrealengine.com/blog/unreal-property-system-reflection)

Gameplay classes make use of special markup, so before we go over them, let us cover some of the basics of the Unreal property system. UE4 uses its own implementation of reflection that enables dynamic features such as garbage collection, serialization, network replication, and Blueprint/C++ communication. These features are opt-in, meaning you have to add the correct markup to your types, otherwise Unreal will ignore them and not generate the reflection data for them. Here is a quick overview of the basic markup:

- `UCLASS()` — Used to tell Unreal to generate reflection data for a class. The class must derive from `UObject`.
- `USTRUCT()` — Used to tell Unreal to generate reflection data for a struct.
- `GENERATED_BODY()` — UE4 replaces this with all the necessary boilerplate code that gets generated for the type.
- `UPROPERTY()` — Enables a member variable of a UCLASS or a USTRUCT to be used as a UPROPERTY. A UPROPERTY has many uses. It can allow the variable to be replicated, serialized, and accessed from Blueprints. They are also used by the garbage collector to keep track of how many references there are to a `UObject`.
- `UFUNCTION()` — Enables a class method of a UCLASS or a USTRUCT to be used as a UFUNCTION. A UFUNCTION can allow the class method to be called from Blueprints and used as RPCs, among other things.

Here is an example declaration of a UCLASS:

```
#include "MyObject.generated.h"

UCLASS(Blueprintable)
class UMyObject : public UObject
{
    GENERATED_BODY()

public:
    MyUObject();

    UPROPERTY(BlueprintReadOnly, EditAnywhere)
    float ExampleProperty;

    UFUNCTION(BlueprintCallable)
    void ExampleFunction();
};
```

You'll first notice the inclusion of `MyObject.generated.h`. UE4 will generate all the reflection data and put it into this file. You must include this file as the last include in the header file that declares your type.

The `UCLASS`, `UPROPERTY`, and `UFUNCTION` markups in this example include additional specifiers. These are not required, but some common specifiers have been added for demonstration purposes. These allow us to specify certain behaviors or properties.

- `Blueprintable` — This class can be extended by a Blueprint.
- `BlueprintReadOnly` — This property can be read from a Blueprint, but not written to.
- `EditAnywhere` — This property can be edited by property windows, on archetypes and instances.
- `Category` — Defines what section this property appears under in the Details view of the Editor. This is helpful for organizational purposes.
- `BlueprintCallable` — This function can be called from Blueprints.

There are too many specifiers to list here, but the following links can be used as reference:

[List of UCLASS Specifiers](https://docs.unrealengine.com/en-US/Programming/UnrealArchitecture/Reference/Classes/Specifiers/index.html)

[List of UPROPERTY Specifiers](https://docs.unrealengine.com/en-US/Programming/UnrealArchitecture/Reference/Properties/Specifiers/index.html)

[List of UFUNCTION Specifiers](https://docs.unrealengine.com/en-US/Programming/UnrealArchitecture/Reference/Functions/Specifiers/index.html)

[List of USTRUCT Specifiers](https://docs.unrealengine.com/en-US/Programming/UnrealArchitecture/Reference/Structs/Specifiers/index.html)

### Object/Actor Iterators

Object iterators are a very useful tool to iterate over all instances of a particular `UObject` type and its subclasses.

```
// Will find ALL current UObject instances
for (TObjectIterator<UObject> It; It; ++It)
{
    UObject* CurrentObject = *It;
    UE_LOG(LogTemp, Log, TEXT("Found UObject named: %s"), *CurrentObject->GetName());
}
```

You can limit the scope of the search by providing a more specific type to the iterator. Suppose you had a class called UMyClass that derived from `UObject`. You could find all instances of that class (and those that derive from it) like this:

```
for (TObjectIterator<UMyClass> It; It; ++It)
{
    // ...
}
```

Using object iterators in PIE (Play In Editor) can lead to unexpected results. Since the Editor is loaded, the object iterator will return all `UObject` instances created for your game world instance, in addition to those that are just being used by the Editor.

Actor iterators work in much the same way as object iterators, but only work for objects that derive from AActor. Actor iterators do not have the problem noted above, and will only return objects being used by the current game world instance.

When creating an Actor iterator, you need to give it a pointer to a **UWorld** instance. Many `UObject` classes, such as **APlayerController**, provide a **GetWorld** method to help you. If you are not sure, you can check the **ImplementsGetWorld** method on a `UObject` to see if it implements the GetWorld method.

```
APlayerController* MyPC = GetMyPlayerControllerFromSomewhere();
UWorld* World = MyPC->GetWorld();

// Like object iterators, you can provide a specific class to get only objects that are
// or derive from that class
for (TActorIterator<AEnemy> It(World); It; ++It)
{
    // ...
}
```

Since AActor derives from `UObject`, you can use **TObjectIterator** to find instances of `AActors` as well. Just be careful in PIE!

## Memory Management and Garbage Collection

In this section, we will go over basic memory management and the garbage collection system in UE4.

[Wiki: Garbage Collection & Dynamic Memory Allocation](https://wiki.unrealengine.com/Garbage_Collection_%26_Dynamic_Memory_Allocation)

### UObjects and Garbage Collection

UE4 uses the reflection system to implement a garbage collection system. With garbage collection, you will not have to manually manage deleting `UObject` instances, you just need to maintain valid references to them. Your classes need to derive from `UObject` in order to be enabled for garbage collection. Here is the simple example class we will be using:

```
UCLASS()
class MyGCType : public UObject
{
    GENERATED_BODY()
};
```

In the garbage collector, there is a concept called the root set. The root set is a list of objects that the collector knows will never be garbage collected. An object will not be garbage collected as long as there is a path of references from an object in the root set to the object in question. If no such path to the root set exists for an object, it is called unreachable and will be collected (deleted) the next time the garbage collector runs. The engine runs the garbage collector at certain intervals.

Any `UObject` pointer stored in a `UPROPERTY` or in a UE4 container class (such as `TArray`) is considered a "reference" for the purposes of garbage collection. Let us start with a simple example.

```
void CreateDoomedObject()
{
    MyGCType* DoomedObject = NewObject<MyGCType>();
}
```

The above function creates a new `UObject`, but does not store a pointer to it in any `UPROPERTY` or UE4 container, and it isn't a part of the root set. Eventually, the garbage collector will detect that this object is unreachable, and destroy it.

### Actors and Garbage Collection

Actors are not usually garbage collected, aside from during a Level's shutdown. Once spawned, you must manually call `Destroy` on them to remove them from the Level without ending the Level. They will be removed from the game immediately, and then fully deleted during the next garbage collection phase.

This is a more common case, where you have Actors with `UObject` properties.

```
UCLASS()
class AMyActor : public AActor
{
    GENERATED_BODY()

public:
    UPROPERTY()
    MyGCType* SafeObject;

    MyGCType* DoomedObject;

    AMyActor(const FObjectInitializer& ObjectInitializer)
        : Super(ObjectInitializer)
    {
        SafeObject = NewObject<MyGCType>();
        DoomedObject = NewObject<MyGCType>();
    }
};

void SpawnMyActor(UWorld* World, FVector Location, FRotator Rotation)
{
    World->SpawnActor<AMyActor>(Location, Rotation);
}
```

When we call the above function, we spawn an Actor into the world. The Actor's constructor creates two objects. One gets assigned to a UPROPERTY, the other to a bare pointer. Since Actors are automatically a part of the root set, `SafeObject` will not be garbage collected because it can be reached from a root set object. `DoomedObject`, however, will not fare so well. We didn't mark it with UPROPERTY, so the collector does not know it is being referenced, and will eventually destroy it, leaving a dangling pointer.

When a `UObject` is garbage collected, all UPROPERTY references to it will be set to null for you. This makes it safe for you to check if an object has been garbage collected or not.

```
if (MyActor->SafeObject != nullptr)
{
    // Use SafeObject
}
```

This is important since, as mentioned before, actors that have had `Destroy` called on them are not removed until the garbage collector runs again. You can check the `IsPendingKill` method to see if a `UObject` is awaiting its deletion. If that method returns true, you should consider the object dead and not use it.

### UStructs

`UStructs`, as mentioned earlier, are meant to be a lightweight version of a `UObject`. As such, `UStructs` cannot be garbage collected. If you must use dynamic instances of `UStructs`, you may want to use smart pointers instead, which we will go over later.

### Non-UObject References

Normal C++ objects (not derived from `UObject`) can also have the ability to add a reference to an object and prevent garbage collection. To do that, your object must derive from **FGCObject** and override its **AddReferencedObjects** method.

```
class FMyNormalClass : public FGCObject
{
public:
    UObject* SafeObject;

    FMyNormalClass(UObject* Object)
        : SafeObject(Object)
    {
    }

    void AddReferencedObjects(FReferenceCollector& Collector) override
    {
        Collector.AddReferencedObject(SafeObject);
    }
};
```

We use the **FReferenceCollector** to manually add a hard reference to the `UObject` we need and do not want garbage collected. When the object is deleted and its destructor is run, the object will automatically clear all references that it added.

### Class Naming Prefixes

Unreal Engine provides tools that generate code for you during the build process. These tools have some class-naming expectations and will trigger warnings or errors if the names do not match the expectations. The list of class prefixes below delineates what the tools are expecting.

- Classes derived from **Actor** prefixed with **A**, such as `AController`.
- Classes derived from **Object** are prefixed with **U**, such as `UComponent`.
- **Enums** are prefixed with **E**, such as `EFortificationType`.
- **Interface** classes are usually prefixed with **I**, such as `IAbilitySystemInterface`.
- **Template** classes are prefixed by **T**, such as `TArray`.
- Classes that derive from **SWidget** (Slate UI) are prefixed by **S**, such as `SButton`.
- Everything else is prefixed by the [letter F](https://forums.unrealengine.com/showthread.php?60061-Unreal-trivia-What-does-the-F-prefix-on-classes-and-structs-stand-for) , such as `FVector`.

### Numeric Types

Since different platforms have different sizes for basic types such as **short**, **int**, and **long**, UE4 provides the following types which you should use as an alternative:

- `int8`/`uint8`: 8-bit signed/unsigned integer
- `int16`/`uint16`: 16-bit signed/unsigned integer
- `int32`/`uint32`: 32-bit signed/unsigned integer
- `int64`/`uint64`: 64-bit signed/unsigned integer

Floating point numbers are also supported with the standard `float` (32-bit) and `double` (64-bit) types.

Unreal Engine has a template, `TNumericLimits<T>`, for finding the minimum and maximum ranges a value type can hold. For more information follow this [link](https://docs.unrealengine.com/latest/INT/API/Runtime/Core/Math/TNumericLimits/index.html) .

### Strings

UE4 provides several different classes for working with strings, depending on your needs.

[Full Topic: String Handling](https://docs.unrealengine.com/en-US/Programming/UnrealArchitecture/StringHandling/index.html)

#### FString

`FString` is a mutable string, analogous to `std::string`. `FString` has a large suite of methods for making it easy to work with strings. To create a new `FString`, use the `TEXT` macro:

```
FString MyStr = TEXT("Hello, Unreal 4!").
```

[Full Topic: FString API](https://docs.unrealengine.com/latest/INT/API/Runtime/Core/Containers/FString/index.html)

#### FText

`FText` is similar to FString, but it is meant for localized text. To create a new `FText`, use the `NSLOCTEXT` macro. This macro takes a namespace, key, and a value for the default language:

```
FText MyText = NSLOCTEXT("Game UI", "Health Warning Message", "Low Health!")
```

You could also use the `LOCTEXT` macro, so you only have to define a namespace once per file. Make sure to undefine it at the bottom of your file.

```
// In GameUI.cpp
#define LOCTEXT_NAMESPACE "Game UI"

//...
FText MyText = LOCTEXT("Health Warning Message", "Low Health!")
//...

#undef LOCTEXT_NAMESPACE
// End of file
```

[Full Topic: FText API](https://docs.unrealengine.com/latest/INT/API/Runtime/Core/Internationalization/FText/index.html)

#### FName

An `FName` stores a commonly recurring string as an identifier in order to save memory and CPU time when comparing them. Rather than storing the complete string many times across every object that references it, `FName` uses a smaller storage footprint index that maps to a given string. This stores the contents of the string once, saving memory when that string is used across many objects. `FName` comparison is fast because UE4 can simply check their index values to see if they match, without having to do a character-by-character comparison.

[Full Topic: FName API](https://docs.unrealengine.com/latest/INT/API/Runtime/Core/UObject/FName/index.html)

#### TCHAR

The `TCHAR` type is used as a way of storing characters independent of the character set being used, which may differ between platforms. Under the hood, UE4 strings use `TCHAR` arrays to store data in the **UTF-16** encoding. You can access the raw data by using the overloaded dereference operator, which returns `TCHAR`.

[Full Topic: Character Encoding](https://docs.unrealengine.com/en-US/Programming/UnrealArchitecture/StringHandling/CharacterEncoding/index.html)

This is needed for some functions, such as `FString::Printf`, where the "%s" string format specifier expects a `TCHAR` instead of an `FString`.

```
FString Str1 = TEXT("World");
int32 Val1 = 123;
FString Str2 = FString::Printf(TEXT("Hello, %s! You have %i points."), *Str1, Val1);
```

The `FChar` type provides a set of static utility functions for working with individual `TCHAR` characters.

```
TCHAR Upper('A');
TCHAR Lower = FChar::ToLower(Upper); // 'a'
```

The `FChar` type is defined as `TChar<TCHAR>` (as it is listed in the API).

[Full Topic: TChar API](https://docs.unrealengine.com/latest/INT/API/Runtime/Core/Misc/TChar/index.html)

### Containers

Containers are classes whose primary function is to store collections of data. The most common of these classes are `TArray`, `TMap`, and `TSet`. Each of these are dynamically sized, and so will grow to whatever size you need.

[Full Topic: Containers API](https://docs.unrealengine.com/latest/INT/API/Runtime/Core/Containers/index.html)

#### TArray

Of these three containers the primary container you'll use in Unreal Engine 4 is TArray, it functions much like **std::vector** does, but offers a lot more functionality. Here are some common operations:

```
TArray<AActor*> ActorArray = GetActorArrayFromSomewhere();

// Tells how many elements (AActors) are currently stored in ActorArray.
int32 ArraySize = ActorArray.Num();

// TArrays are 0-based (the first element will be at index 0)
int32 Index = 0;
// Attempts to retrieve an element at the given index
AActor* FirstActor = ActorArray[Index];

// Adds a new element to the end of the array
AActor* NewActor = GetNewActor();
ActorArray.Add(NewActor);

// Adds an element to the end of the array only if it is not already in the array
ActorArray.AddUnique(NewActor); // Won't change the array because NewActor was already added

// Removes all instances of 'NewActor' from the array
ActorArray.Remove(NewActor);

// Removes the element at the specified index
// Elements above the index will be shifted down by one to fill the empty space
ActorArray.RemoveAt(Index);

// More efficient version of 'RemoveAt', but does not maintain order of the elements
ActorArray.RemoveAtSwap(Index);

// Removes all elements in the array
ActorArray.Empty();
```

`TArray` has the added benefit of having its elements garbage collected. This assumes that the `TArray` stores `UObject`-derived pointers.

```
UCLASS()
class UMyClass : UObject
{
    GENERATED_BODY();

    // ...

    UPROPERTY()
    AActor* GarbageCollectedActor;

    UPROPERTY()
    TArray<AActor*> GarbageCollectedArray;

    TArray<AActor*> AnotherGarbageCollectedArray;
};
```

We'll cover the garbage collection in depth in a later section.

[Full Topic: TArrays](https://docs.unrealengine.com/en-US/Programming/UnrealArchitecture/TArrays/index.html)

[Full Topic: TArray API](https://docs.unrealengine.com/latest/INT/API/Runtime/Core/Containers/TArray/index.html)

#### TMap

A `TMap` is a collection of key-value pairs, similar to `std::map`. `TMap` has quick methods for finding, adding, and removing elements based on their key. You can use any type for the key, as long as it has a `GetTypeHash` function defined for it, which we will go over later.

Let us say you were creating a grid-based board game and needed to store and query what piece is on each square. A `TMap` would provide you with an easy way to do that. If the board is small and always the same size, there may be more efficient ways at going about this, but for this example, let's assume a larger board with relatively few pieces.

```
enum class EPieceType
{
    King,
    Queen,
    Rook,
    Bishop,
    Knight,
    Pawn
};

struct FPiece
{
    int32 PlayerId;
    EPieceType Type;
    FIntPoint Position;

    FPiece(int32 InPlayerId, EPieceType InType, FIntVector InPosition) :
        PlayerId(InPlayerId),
        Type(InType),
        Position(InPosition)
    {
    }
};

class FBoard
{
private:

    // Using a TMap, we can refer to each piece by its position
    TMap<FIntPoint, FPiece> Data;

public:
    bool HasPieceAtPosition(FIntPoint Position)
    {
        return Data.Contains(Position);
    }
    FPiece GetPieceAtPosition(FIntPoint Position)
    {
        return Data[Position];
    }

    void AddNewPiece(int32 PlayerId, EPieceType Type, FIntPoint Position)
    {
        FPiece NewPiece(PlayerId, Type, Position);
        Data.Add(Position, NewPiece);
    }

    void MovePiece(FIntPoint OldPosition, FIntPoint NewPosition)
    {
        FPiece Piece = Data[OldPosition];
        Piece.Position = NewPosition;
        Data.Remove(OldPosition);
        Data.Add(NewPosition, Piece);
    }

    void RemovePieceAtPosition(FIntPoint Position)
    {
        Data.Remove(Position);
    }

    void ClearBoard()
    {
        Data.Empty();
    }
};
```

[Full Topic: TMaps](https://docs.unrealengine.com/en-US/Programming/UnrealArchitecture/TMap/index.html)

[Full Topic: TMap API](https://docs.unrealengine.com/latest/INT/API/Runtime/Core/Containers/TMapBase/index.html)

#### TSet

A `TSet` stores a collection of unique values, similar to `std::set`. Although `TArray` supports set-like behavior with its `AddUnique` and `Contains` methods, `TSet` has faster implementations of these operations and protects against adding non-unique elements automatically.

```
TSet<AActor*> ActorSet = GetActorSetFromSomewhere();

int32 Size = ActorSet.Num();

// Adds an element to the set, if the set does not already contain it
AActor* NewActor = GetNewActor();
ActorSet.Add(NewActor);

// Check if an element is already contained by the set
if (ActorSet.Contains(NewActor))
{
    // ...
}

// Remove an element from the set
ActorSet.Remove(NewActor);

// Removes all elements from the set
ActorSet.Empty();

// Creates a TArray that contains the elements of your TSet
TArray<AActor*> ActorArrayFromSet = ActorSet.Array();
```

[Full Topic: TSet API](https://docs.unrealengine.com/latest/INT/API/Runtime/Core/Containers/TSet/index.html)

#### Container Iterators

Using iterators, you can loop through each element of a container. Here is an example of what the iterator syntax looks like, using a `TSet`.

```
void RemoveDeadEnemies(TSet<AEnemy*>& EnemySet)
{
    // Start at the beginning of the set, and iterate to the end of the set
    for (auto EnemyIterator = EnemySet.CreateIterator(); EnemyIterator; ++EnemyIterator)
    {
        // The * operator gets the current element
        AEnemy* Enemy = *EnemyIterator;
        if (Enemy.Health == 0)
        {
            // 'RemoveCurrent' is supported by TSets and TMaps
            EnemyIterator.RemoveCurrent();
        }
    }
}
```

Other supported operations you can use with iterators:

```
// Moves the iterator back one element
--EnemyIterator;

// Moves the iterator forward/backward by some offset, where Offset is an integer
EnemyIterator += Offset;
EnemyIterator -= Offset;

// Gets the index of the current element
int32 Index = EnemyIterator.GetIndex();

// Resets the iterator to the first element
EnemyIterator.Reset();
```

#### For-each Loop

Iterators are nice, but can be a bit cumbersome if you just want to loop through each element once. Each container class also supports the "for each" style syntax to loop over the elements. `TArray` and `TSet` return each element, whereas `TMap` returns a key-value pair.

```
// TArray
TArray<AActor*> ActorArray = GetArrayFromSomewhere();
for (AActor* OneActor : ActorArray)
{
    // ...
}

// TSet - Same as TArray
TSet<AActor*> ActorSet = GetSetFromSomewhere();
for (AActor* UniqueActor : ActorSet)
{
    // ...
}

// TMap - Iterator returns a key-value pair
TMap<FName, AActor*> NameToActorMap = GetMapFromSomewhere();
for (auto& KVP : NameToActorMap)
{
    FName Name = KVP.Key;
    AActor* Actor = KVP.Value;

    // ...
}
```



Remember that the `auto` keyword does not automatically specify pointers or references. As in the preceding example, you may need to add the appropriate notation if you use `auto`.



#### Using Your Own Types with TSet/TMap (Hash Functions)

`TSet` and `TMap` require the use of hash functions internally. Most UE4 types that you would commonly store in a `TSet` or use as a key in a `TMap` already define their own hash functions. If you create your own class and would like to use it in a `TSet`, or as the key in a `TMap`, you will need to provide a hash function that takes a const pointer or reference to your type and returns a `uint32`. This return value is known as the object's *hash code*, and should uniquely identify the object. This means that two objects of your type that are considered equal should always return the same hash code.

```
class FMyClass
{
    uint32 ExampleProperty1;
    uint32 ExampleProperty2;

    // Hash Function
    friend uint32 GetTypeHash(const FMyClass& MyClass)
    {
        // HashCombine is a utility function for combining two hash values.
        uint32 HashCode = HashCombine(MyClass.ExampleProperty1, MyClass.ExampleProperty2);
        return HashCode;
    }

    // For demonstration purposes, two objects that are equal
    // should always return the same hash code.
    bool operator==(const FMyClass& LHS, const FMyClass& RHS)
    {
        return LHS.ExampleProperty1 == RHS.ExampleProperty1
            && LHS.ExampleProperty2 == RHS.ExampleProperty2;
    }
};
```

Now, `TSet<FMyClass>` and `TMap<FMyClass, ...>` will use the proper hash function when hashing keys. If you using pointers as keys (i.e. `TSet<FMyClass*>`) implement `uint32 GetTypeHash(const FMyClass* MyClass)` as well.

[Blog Post: UE4 Libraries You Should Know About](https://www.unrealengine.com/blog/ue4-libraries-you-should-know-about)

Tags

[Getting Started](https://docs.unrealengine.com/en-US/SiteIndex/index.html?tags=Getting Started)

 

[Programming](https://docs.unrealengine.com/en-US/SiteIndex/index.html?tags=Programming)