# RigidBody

## 什么是刚体？

刚体是直接由物理引擎控制以模拟物理对象行为的物体。为了定义物体的形状，必须分配一个或多个 [Shape](https://docs.godotengine.org/zh_CN/stable/classes/class_shape.html#class-shape) 对象。注意，设置这些形状的位置将影响物体的重心。

## How to control a rigid body如何控制

A rigid body's behavior can be altered by setting its properties, such as friction, mass, bounce, etc. These properties can be set in the Inspector or via code. See [RigidBody](https://docs.godotengine.org/zh_CN/stable/classes/class_rigidbody.html#class-rigidbody) for the full list of properties and their effects.
可以通过设置刚体的属性（例如，摩擦，质量，弹跳等）来更改刚体的行为。可以在检查器中或通过代码设置这些属性。 有关属性及其效果的完整列表，请参见[RigidBody](https://docs.godotengine.org/zh_CN/stable/classes/class_rigidbody.html#class-rigidbody)。

有几种方法可以控制刚体的运动，这取决于您的应用程序。

If you only need to place a rigid body once, for example to set its initial location, you can use the methods provided by the [Spatial](https://docs.godotengine.org/zh_CN/stable/classes/class_spatial.html#class-spatial) node, such as `set_global_transform()` or `look_at()`. However, these methods cannot be called every frame or the physics engine will not be able to correctly simulate the body's state. As an example, consider a rigid body that you want to rotate so that it points towards another object. A common mistake when implementing this kind of behavior is to use `look_at()` every frame, which breaks the physics simulation. Below, we'll demonstrate how to implement this correctly.
如果您只需要放置一次刚体，例如设置其初始位置，则可以使用[Spatial]（https://docs.godotengine.org/zh_CN/stable/classes/class_spatial.html ＃class-spatial）节点，例如`set_global_transform（）`或`look_at（）`。 但是，这些方法不能在每一帧都调用，否则物理引擎将无法正确模拟人体的状态。 例如，考虑要旋转的刚体，使其指向另一个对象。 实现这种行为的一个常见错误是每帧都使用look_at（），这会破坏物理模拟。 下面，我们将演示如何正确实现这一点。

The fact that you can't use `set_global_transform()` or `look_at()` methods doesn't mean that you can't have full control of a rigid body. Instead, you can control it by using the `_integrate_forces()` callback. In this method, you can add *forces*, apply *impulses*, or set the *velocity* in order to achieve any movement you desire.
您不能使用`set_global_transform()`或`look_at()`方法的事实并不意味着您无法完全控制刚体。 相反，您可以使用`_integrate_forces（）`回调来控制它。 在这种方法中，您可以添加*力*，施加*脉冲*或设置*速度*以获得所需的任何运动。

## The "look at" method

As described above, using the Spatial node's `look_at()` method can't be used each frame to follow a target. Here is a custom `look_at()` method that will work reliably with rigid bodies:
如上所述，无法使用空间节点的`look_at`方法在每一帧都遵循目标。 这是一个自定义的`look_at()`方法，该方法可以与刚体可靠地工作：

GDScriptC#

```
extends RigidBody

func look_follow(state, current_transform, target_position):
    var up_dir = Vector3(0, 1, 0)
    var cur_dir = current_transform.basis.xform(Vector3(0, 0, 1))
    var target_dir = (target_position - current_transform.origin).normalized()
    var rotation_angle = acos(cur_dir.x) - acos(target_dir.x)

    state.set_angular_velocity(up_dir * (rotation_angle / state.get_step()))

func _integrate_forces(state):
    var target_position = $my_target_spatial_node.get_global_transform().origin
    look_follow(state, get_global_transform(), target_position)
```

This method uses the rigid body's `set_angular_velocity()` method to rotate the body. It first calculates the difference between the current and desired angle and then adds the velocity needed to rotate by that amount in one frame's time.
该方法使用刚体的`set_angular_velocity()`方法旋转刚体。 它首先计算当前角度和所需角度之间的差，然后在一帧时间内添加旋转所需的速度。

注解

This script will not work with rigid bodies in *character mode* because then, the body's rotation is locked. In that case, you would have to rotate the attached mesh node instead using the standard Spatial methods.
该脚本不适用于*character角色模式*的刚体，因为那样会锁定刚体的旋转。 在这种情况下，您将不得不使用标准的Spatial方法来旋转附加的网格节点。