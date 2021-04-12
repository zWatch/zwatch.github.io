Block Interaction
=================

There are many different ways players (and other things) can interact with blocks, such as right clicking, left clicking, colliding, walking on, and of course mining.
玩家（以及其他事物）可以通过多种不同的方式与区块互动，例如右键单击，左键单击，碰撞，继续前进，当然还有挖掘。

This page will cover the basics of the most common types of interaction with blocks.
该页面将介绍与块交互的最常见类型的基础。

Player Right Click
------------------
Since left clicking, or "punching", a block does not generally result in any unique behavior, it is probably fair to say right clicking, or "activation", is *the* most common method of interaction. And thankfully, it is also one of the simplest to handle.
由于左击或“打孔”通常不会导致任何独特的行为，因此可以说，右击或“激活”是最常见的交互方法。 值得庆幸的是，它也是最简单的处理方法之一。

### `onBlockActivated`

This is the method that controls right click behavior, and it is a rather complicated one. Here is its full signature:
这是控制右键单击行为的方法，这是一种相当复杂的方法。 这是其完整签名：

```java
public boolean onBlockActivated(World worldIn,
                                BlockPos pos,
                                IBlockState state,
                                EntityPlayer playerIn,
                                EnumHand hand,
                                @Nullable ItemStack heldItem,
                                EnumFacing side,
                                float hitX,
                                float hitY,
                                float hitZ)
```

There's quite a bit to discuss here.
这里有很多讨论。

#### Parameters

The first few parameters are obvious, they are the current world, position, and state of the block. Next is the player that is activating the block, and the hand with which they are activating.
前几个参数是显而易见的，它们是块的当前世界，位置和状态。 接下来是正在激活方块的玩家以及他们正在使用的手牌。

The next parameter, `heldItem`, is the `ItemStack` which the player activated the block with. Note that this parameter is `@Nullable` meaning it can be passed as null (i.e. no item was in the hand).

!!! important
    The ItemStack passed to this method is the one you should use for checking what was in the player's hand. Grabbing the currently held stack from the player is unreliable as it may have changed since activation.

The last four parameters are all related. `side` is obviously the side which was activated, however `hitX`, `hitY`, and `hitZ` are a bit less obvious. These are the coordinates of the activation on the block's bounds. They are on the range 0 to 1, and represent where exactly "on" the block the player clicked.

#### Return Value

What is this magic boolean which must be returned? Simply put this, is whether or not the method "did" something. Return true if some action was performed, this will prevent further things from happening, such as item activation.


!!! important
    Returning `false` from this method on the client will prevent it being called on the server. It is common practice to just check `worldIn.isRemote` and return `true`, and otherwise go on to normal activation logic. Vanilla has many examples of this, such as the chest.

### Uses

The uses for activation are literally endless. However, there are some common ones which deserve their own section.

#### GUIs

One of the most common things to do on block activation is opening a GUI. Many blocks in vanilla behave this way, such as chests, hoppers, furnaces, and many more. More about GUIs can be found on [their page](GUIs).

#### Activation

Another common use for activation is, well, activation. This can be something like "turning on" a block, or triggering it to perform some action. For instance, a block could light up when activated. A vanilla example would be buttons or levers.

!!! important
    `onBlockActivated` is called on both the client and the server, so be sure to keep the [sidedness] of your code in mind. Many things, like opening GUIs and modifying the world, should only be done on the server-side.

Player Break/Destroy
--------------------
*Coming Soon*

Player Highlighting
-------------------
*Coming Soon*

Entity Collision
----------------
*Coming Soon*

`onBlockClicked`
----------------

```java
public void onBlockClicked(World worldIn, BlockPos pos, EntityPlayer playerIn)
```

Called on a block when it is clicked by a player.

!!! Note
    
    This method is for when the player *left-clicks* on a block.
    Don't get this confused with `onBlockActivated`, which is called when the player *right-clicks*.

### Parameters:
|      Type       |     Name     |                  Description                  |
|:---------------:|:------------:|:----------------------------------------------|
|     `World`     |  `worldIn`   | The world that the block was clicked in       |
|    `BlockPos`   |    `pos`     | The position of the block that was clicked    |
|  `EntityPlayer` |  `playerIn`  | The player who did the clicking               |

### Usage example
This method is perfect for adding custom events when a player clicks on a block.

By default this method does nothing.  
Two blocks that override this method are the **Note Block** and the **RedstoneOre Block**.

The Note block overrides this method so that when left-clicked, it plays a sound.  
The RedstoneOre block overrides method so that when left-clicked, it gives off emits faint light for a few seconds.

`onBlockDestroyedByPlayer`
----------------

```java
public void onBlockDestroyedByPlayer(World worldIn, BlockPos pos, IBlockState state)
```

Called on a block after it's destroyed by a player

### Parameters:
|      Type       |    Name     |                 Description                  |
|:---------------:|:-----------:|:---------------------------------------------|
|     `World`     |  `worldIn`  | The world that the block was destroyed       |
|   `BlockPos`    |    `pos`    | The position of the block that was destroyed |
|  `IBlockState`  |   `state`   | The state of the block that was destroyed    |

!!! Warning
    
    The `pos` parameter may not hold the state indicated

### Usage example
This method is perfect for adding custom events as a result of a player destroying a block

By default this method does nothing.  

The **TNT Block** overrides this method to cause it's explosion when a player destroys it.  
This method is used by extended pistons; since an extended piston is made up of two blocks (the extended head and the base), 
the **PistonMoving Block** makes use of this method to destroy the base block when the PistonMoving block is destroyed. 


[sidedness]: ../concepts/sides.md
