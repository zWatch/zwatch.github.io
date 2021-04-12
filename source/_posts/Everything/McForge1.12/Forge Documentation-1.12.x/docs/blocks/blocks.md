Blocks
======

Blocks are, obviously, essential to the Minecraft world. They make up all of the terrain, structures, and machines. Chances are if you are interested in making a mod, then you will want to add some blocks. This page will guide you through the creation of blocks, and some of the things you can do with them.

显然，区块(方块)对Minecraft世界至关重要。 它们构成了所有的地形，结构和机器。 如果您有兴趣制作一个模块，那么您可能想添加一些模块。 该页面将指导您完成块的创建以及您可以使用它们执行的一些操作。

Creating a Block
----------------

### Basic Blocks

For simple blocks, which need no special functionality (think cobblestone, wood planks, etc.), a custom class is not necessary. By simply instantiating the `Block` class and calling some of the many setters, one can create many different types of blocks. For instance:
对于不需要特殊功能（例如鹅卵石，木板等）的简单块，则不需要自定义类。 通过简单地实例化“ Block”类并调用许多设置器中的一些，就可以创建许多不同类型的块。 例如：

- `setHardness` - Controls the time it takes to break the block. It is an arbitrary value. For reference, stone has a hardness of 1.5, and dirt 0.5. If the block should be unbreakable, a convenience method `setBlockUnbreakable` is provided.
  setHardness`-控制破坏该块所花费的时间。 它是一个任意值。 作为参考，石材的硬度为1.5，脏物（泥土）的硬度为0.5。 如果该块是不可破坏的，则提供一种便捷方法“ setBlockUnbreakable”。
- `setResistance` - Controls the explosion resistance of the block. This is separate from hardness, but `setHardness` will also set the resistance to 5 times the hardness value, if the resistance is any lower than this value.
  `setResistance`-控制块的防爆性。 这与硬度是分开的，但是如果电阻值低于此值，则 `setHardness`还将电阻值设置为硬度值的5倍。
- `setSoundType` - Controls the sound the block makes when it is punched, broken, or placed. Requires a `SoundType` argument, see the [sounds][] page for more details.
  setSoundType-控制块在被打孔，折断或放置时发出的声音。 需要一个“ SoundType”参数，有关更多详细信息，请参见[sounds] []页面。
- `setLightLevel` - Controls the light emission of the block. **Note:** This method takes a value from zero to one, not zero to fifteen. To calculate this value, take the light level you wish your block to emit and divide by 16. For instance a block which emits level 5 light should pass `5 / 16f` to this method.
  `setLightLevel`-控制块的发光。 **注意：**此方法的取值范围是零到一，而不是零到十五。 要计算该值，请取您希望您的块发出的光强度并除以16。例如，发出5级光的块应将“ 5 / 16f”传递给此方法。
- `setLightOpacity` - Controls the amount light passing through this block will be dimmed. Unlike `setLightLevel` this value is on a scale from zero to 15. For example, setting this to `3` will lower light by 3 levels every time it passes through this type of block.

  setLightOpacity-控制通过此块的光的数量将变暗。 与“ setLightLevel”不同，此值的范围是从0到15。例如，将其设置为“ 3”会在每次穿过此类块时将光线降低3级。
- `setUnlocalizedName` - Mostly self explanatory, sets the unlocalized name of the block. This name will be prepended with "tile." and appended with ".name" for localization purposes. For instance `setUnlocalizedName("foo")` will cause the block's actual localization key to be "tile.foo.name". For more advanced localization control, a custom Item will be needed. We'll get into this more later.
  `setUnlocalizedName`-大部分用于自我说明，设置块的未本地化名称。 该名称将以“ tile”开头。 并附加“ .name”以进行本地化。 例如，`setUnlocalizedName（“ foo”）`将导致块的实际本地化键为“ tile.foo.name”。 对于更高级的本地化控制，将需要一个自定义项。 我们稍后再讨论。
- `setCreativeTab` - Controls which creative tab this block will fall under. This must be called if the block should be shown in the creative menu. Tab options can be found in the `CreativeTabs` class.
  setCreativeTab-控制该块属于哪个广告素材标签。 如果应在广告素材菜单中显示该广告块，则必须调用此方法。 选项卡选项可在`CreativeTabs`类中找到。

All these methods are *chainable* which means you can call them in series. See `Block#registerBlocks` for examples of this.
所有这些方法都是*chainable*的，这意味着您可以串联调用它们。 有关此示例，请参见`Block#registerBlocks`。

### Advanced Blocks

Of course, the above only allows for extremely basic blocks. If you want to add functionality, like player interaction, a custom class is required. However, the `Block` class has many methods and unfortunately not every single one can be documented here. See the rest of the pages in this section for things you can do with blocks.
当然，以上内容仅允许使用极其基本的块。 如果要添加功能（例如玩家互动），则需要一个自定义类。 然而，`Block`类有很多方法，不幸的是，这里没有记录每个方法。 请参阅本节的其余页面，以了解可以使用块执行的操作。

Registering a Block 注册方块
-------------------

Blocks must be [registered][registering] to function.
必须对块进行[注册] [注册]才能起作用。

!!! important

    A block in the world and a "block" in an inventory are very different things. A block in the world is represented by an `IBlockState`, and its behavior defined by an instance of `Block`. Meanwhile, an item in an inventory is an `ItemStack`, controlled by an `Item`. As a bridge between the different worlds of `Block` and `Item`, there exists the class `ItemBlock`. `ItemBlock` is a subclass of `Item` that has a field `block` that holds a reference to the `Block` it represents. `ItemBlock` defines some of the behavior of a "block" as an item, like how a right click places the block. It's possible to have a `Block` without an `ItemBlock`. (E.g. `minecraft:water` exists a block, but not an item. It is therefore impossible to hold it in an inventory as one.)
    世界上的一个区块和库存中的一个“区块”是完全不同的东西。 世界上的一个块由一个“ IBlockState”表示，其行为由一个“块”的实例定义。 同时，库存中的项目是由“项目”控制的“项目堆栈”。 作为“ Block”和“ Item”的不同世界之间的桥梁，存在类“ ItemBlock”。 ItemBlock是Item的子类，它具有一个字段block，该字段保存对其表示的Block的引用。 “ ItemBlock”将“块”的某些行为定义为一项，例如右键单击放置块的方式。 有可能没有`ItemBlock`的`Block`。 （例如，“ minecraft：water”是一个块，但不是一个项目。因此，不可能将其作为一个整体保存在清单中。）
    
    When a block is registered, *only* a block is registered. The block does not automatically have an `ItemBlock`. To create a basic `ItemBlock` for a block, one should use `new ItemBlock(block).setRegistryName(block.getRegistryName())`. The unlocalized name is the same as the block's. Custom subclasses of `ItemBlock` may be used as well. Once an `ItemBlock` has been registered for a block, `Item.getItemFromBlock` can be used to retrieve it. `Item.getItemFromBlock` will return `null` if there is no `ItemBlock` for the `Block`, so if you are not certain that there is an `ItemBlock` for the `Block` you are using, check for `null`.
    当注册一个块时，*仅*注册一个块。 该块不会自动具有“ ItemBlock”。 要为一个块创建一个基本的“ ItemBlock”，应使用“ new ItemBlock（block）.setRegistryName（block.getRegistryName（））”。 未本地化的名称与块的名称相同。 也可以使用ItemBlock的自定义子类。 一旦为块注册了“ ItemBlock”，就可以使用“ Item.getItemFromBlock”来检索它。 如果没有针对该Block的ItemBlock，那么Item.getItemFromBlock将返回null，因此，如果不确定使用的Block的ItemBlock，请检查`null`。 。

Further Reading
---------------

For information about block properties, such as those used for vanilla blocks like wood types, fences, walls, and many more, see the section on [blockstates][].

有关块属性的信息，例如用于香草块的属性，如木材类型，栅栏，墙壁等，请参见[blockstates] []部分。

[sounds]: ../effects/sounds.md
[registering]: ../concepts/registries.md#registering-things
[blockstates]: states.md
