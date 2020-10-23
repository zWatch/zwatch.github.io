
来自 https://docs.unrealengine.com/en-US/Programming/UnrealArchitecture/Reference/Properties/index.html


# Properties
Reference for creating and implementing properties for gameplay classes.Unreal Engine 4.23 Beginner
## Property Declaration
Properties are declared using standard C++ variable syntax, preceded by the UPROPERTY macro which defines property metadata and variable specifiers.使用标准C ++变量语法声明属性，然后使用UPROPERTY宏定义属性元数据和变量说明符。

```
UPROPERTY([specifier, specifier, ...], [meta(key=value, key=value, ...)])
Type VariableName;
```

## Core Data Types
### Integers
The convention for integral data types is "int" or "uint" followed by the size in bits.
整数数据类型的约定为“ int”或“ uint”，后跟以位为单位的大小。

Variable Type Description
uint8   8-bit unsigned
uint16  16-bit unsigned
uint32  32-bit unsigned
uint64  64-bit unsigned
int8    8-bit signed
int16   16-bit signed
int32   32-bit signed
int64   64-bit signed

### As Bitmasks
Integer properties can now be exposed to the Editor as bitmasks. To mark an integer property as a bitmask, just add "bitmask" to the meta section, as follows:
现在可以将整数属性作为位掩码公开给编辑器。 要将整数属性标记为位掩码，只需将“ bitmask”添加到meta部分，如下所示：

```
UPROPERTY(EditAnywhere, Meta = (Bitmask))
int32 BasicBits;
```

Adding this meta tag will cause the integer to be editable as a drop-down list of generically-named flags ("Flag 1", "Flag 2", "Flag 3", etc.) that can be turned on or off individually.
添加此元标记将使该整数作为可单独打开或关闭的通用标志（“标志1”，“标志2”，“标志3”等）的下拉列表可编辑。

GenericBitmask.png[!img](https://docs.unrealengine.com/Images/Programming/UnrealArchitecture/Reference/Properties/GenericBitmask.webp)

In order to customize the bitflags' names, we must first create a UENUM with the "bitflags" meta tag:
为了自定义位标记的名称，我们必须首先使用“ bitflags”元标记创建一个UENUM：

```
UENUM(Meta = (Bitflags))
enum class EColorBits
{
    ECB_Red,
    ECB_Green,
    ECB_Blue
};
```

After creating this UENUM, we can reference it with the "BitmaskEnum" meta tag, like this:
创建此UENUM之后，我们可以使用“ BitmaskEnum”元标记来引用它，如下所示：

```
UPROPERTY(EditAnywhere, Meta = (Bitmask, BitmaskEnum = "EColorBits"))
int32 ColorFlags;
```

Following this change, the bitflags listed in the drop-down box will take on the names and values of the enumerated class entries. In the example above, ECB_Red is value 0, meaning it will activate bit 0 (adding 1 to ColorFlags) when checked. ECB_Green corresponds to bit 1 (adding 2 to ColorFlags), and ECB_Blue corresponds to bit 2 (adding 4 to ColorFlags).
进行此更改后，下拉框中列出的位标志将采用枚举的类条目的名称和值。 在上面的示例中，ECB_Red的值为0，表示选中时它将激活位0（向ColorFlags加1）。 ECB_Green对应于位1（将2加到ColorFlags），而ECB_Blue对应于位2（将4加到ColorFlags）。

CustomBitmask.png[!img](https://docs.unrealengine.com/Images/Programming/UnrealArchitecture/Reference/Properties/CustomBitmask.webp)

*** Note: While enumerated types can contain more than 32 entries, only the first 32 values will be visible in a bitmask association in the Property Editor UI. Similarly, while explicit-value entries are accepted, entries with explicit values not between 0 and 31 will not be included in the drop-down. 虽然枚举类型可以包含32个以上的条目，但在属性编辑器UI的位掩码关联中仅可见前32个值。 同样，虽然接受显式值条目，但显式值不在0到31之间的条目将不包含在下拉列表中。 ***

### Floating Point Types
Unreal uses the standard C++ floating point types, float, and double.
虚幻使用标准的C ++浮点类型，float和double。

### Boolean Types
Boolean types can be represented either with the C++ bool keyword or as a bitfield.
布尔类型可以用C ++ bool关键字或位域表示。

```
uint32 bIsHungry : 1;
bool bIsThirsty;
```

### Strings
Unreal Engine 4 supports three core types of strings.
虚幻引擎4支持三种核心类型的字符串。

+ FString is a classic "dynamic array of chars" string type. FString是经典的“动态字符数组”字符串类型。
+ FName is a reference to an immutable case-insensitive string in a global string table. It is smaller and more efficient to pass around than an FString, but more difficult to manipulate. FName是对全局字符串表中不可变的不区分大小写的字符串的引用。 它比FString较小，传递效率更高，但更难操纵。
+ FText is a more robust string representation designed to handle localization.FText是设计用于处理本地化的更健壮的字符串表示形式。

For most uses, Unreal relies on the TCHAR type for characters. The TEXT() macro is available to denote TCHAR literals.对于大多数用途，虚幻依赖于字符的TCHAR类型。 TEXT（）宏可用于表示TCHAR文字。

```
MyDogPtr->DogName = FName(TEXT("Samson Aloysius"));
```

For more on the three string types, when to use each one, and how to work with them, see the String Handling documentation .有关这三种字符串类型，何时使用每种字符串以及如何使用它们的更多信息，请参见[String Handling文档](https://docs.unrealengine.com/en-US/Programming/UnrealArchitecture/StringHandling/index.html)。

## Property Specifiers
When declaring properties, Property Specifiers can be added to the declaration to control how the property behaves with various aspects of the Engine and Editor.
声明属性时，可以将属性说明符添加到声明中，以控制属性在引擎和编辑器各个方面的行为方式。

Property Tag        Effect

AdvancedDisplay         The property will be placed in the advanced (dropdown) section of any panel where it appears.该属性将放置在显示该属性的任何面板的高级（下拉）部分中。

AssetRegistrySearchable     The AssetRegistrySearchable Specifier indicates that this property and its value will be automatically added to the Asset Registry for any Asset class instances containing this as a member variable. It is not legal to use on struct properties or parameters. AssetRegistrySearchable说明符指示对于包含此属性作为成员变量的任何Asset类实例，此属性及其值将自动添加到Asset Registry。 在结构属性或参数上使用是非法的。

BlueprintAssignable     Usable with Multicast Delegates only. Exposes the property for assigning in Blueprints.仅可用于多播代表。 公开要分配给“蓝图”的属性。

BlueprintAuthorityOnly      This property must be a Multicast Delegate. In Blueprints, it will only accept events tagged with BlueprintAuthorityOnly.此属性必须是多播代理。 在Blueprints中，它将仅接受标记有BlueprintAuthorityOnly的事件。

BlueprintCallable       Multicast Delegates only. Property should be exposed for calling in Blueprint code.仅用于组播代表。 应该公开属性以用于调用蓝图代码。

BlueprintGetter=GetterFunctionName      This property specifies a custom accessor function. If this property isn't also tagged with BlueprintSetter or BlueprintReadWrite, then it is implicitly BlueprintReadOnly.此属性指定自定义访问器函数。 如果此属性也未用BlueprintSetter或BlueprintReadWrite标记，则它隐式为BlueprintReadOnly。

BlueprintReadOnly       This property can be read by Blueprints, but not modified. This Specifier is incompatible with the BlueprintReadWrite Specifier.蓝图可以读取此属性，但不能修改。 该说明符与BlueprintReadWrite说明符不兼容。

BlueprintReadWrite      This property can be read or written from a Blueprint. This Specifier is incompatible with the BlueprintReadOnly Specifier.可以从蓝图读取或写入此属性。 该说明符与BlueprintReadOnly说明符不兼容。

BlueprintSetter=SetterFunctionName      This property has a custom mutator function, and is implicitly tagged with BlueprintReadWrite. Note that the mutator function must be named and part of the same class.此属性指定自定义访问器函数。 如果此属性也未用BlueprintSetter或BlueprintReadWrite标记，则它隐式为BlueprintReadOnly。

Category="TopCategory|SubCategory|..."      Specifies the category of the property when displayed in Blueprint editing tools. Define nested categories using the | operator.当在蓝图编辑工具中显示时，指定属性的类别。 使用|定义嵌套类别。 操作员。

Config      This property will be made configurable. The current value can be saved to the .ini file associated with the class and will be loaded when created. Cannot be given a value in default properties. Implies BlueprintReadOnly.此属性将变为可配置的。 当前值可以保存到与该类关联的.ini文件中，并在创建时加载。 无法在默认属性中提供值。 表示BlueprintReadOnly。

DuplicateTransient      Indicates that the property's value should be reset to the class default value during any type of duplication (copy/paste, binary duplication, etc.).指示在任何类型的复制（复制/粘贴，二进制复制等）期间，属性值应重置为类默认值。？？？

EditAnywhere        Indicates that this property can be edited by property windows, on archetypes and instances. This Specifier is incompatible with any of the the "Visible" Specifiers.指示可以通过属性窗口在原型和实例上编辑此属性。 该说明符与任何“可见”说明符都不兼容。

EditDefaultsOnly        Indicates that this property can be edited by property windows, but only on archetypes. This Specifier is incompatible with any of the "Visible" Specifiers.指示此属性可以由属性窗口编辑，但只能在原型上编辑。 该说明符与任何“可见”说明符都不兼容。

EditFixedSize       Only useful for dynamic arrays. This will prevent the user from changing the length of an array via the Unreal Editor property window.仅对动态数组有用。 这样可以防止用户通过“虚幻编辑器”属性窗口更改数组的长度。

EditInline      Allows the user to edit the properties of the Object referenced by this property within Unreal Editor's property inspector (only useful for Object references, including arrays of Object reference).允许用户在虚幻编辑器的属性检查器中编辑此属性引用的对象的属性（仅对对象引用（包括对象引用数组）有用）。？？？

EditInstanceOnly        Indicates that this property can be edited by property windows, but only on instances, not on archetypes. This Specifier is incompatible with any of the "Visible" Specifiers.指示可以通过属性窗口来编辑此属性，但只能在实例上，而不能在原型上。 该说明符与任何“可见”说明符都不兼容。

Export      Only useful for Object properties (or arrays of Objects). Indicates that the Object assigned to this property should be exported in its entirety as a subobject block when the Object is copied (such as for copy/paste operations), as opposed to just outputting the Object reference itself.仅对对象属性（或对象数组）有用。 指示分配给该属性的对象在复制对象时（例如用于复制/粘贴操作）应整体导出为子对象块，而不是仅输出对象引用本身。

GlobalConfig        Works just like Config except that you cannot override it in a subclass. Cannot be given a value in default properties. Implies BlueprintReadOnly.除了无法在子类中覆盖它外，其工作方式与Config相同。 无法在默认属性中提供值。 隐示BlueprintReadOnly。

Instanced       Object (UCLASS) properties only. When an instance of this class is created, it will be given a unique copy of the Object assigned to this property in defaults. Used for instancing subobjects defined in class default properties. Implies EditInline and Export.仅对象（UCLASS）属性。 创建此类的实例时，默认情况下会为它分配一个分配给该属性的对象的唯一副本。 用于实例化类默认属性中定义的子对象。 暗示EditInline和Export。？？？

Interp      Indicates that the value can be driven over time by a Track in Matinee.指示该值可以由Matinee中的Track随时间驱动。？？？

Localized       The value of this property will have a localized value defined. Mostly used for strings. Implies ReadOnly.此属性的值将定义一个本地化的值。 主要用于字符串。 表示只读。

Native      Property is native: C++ code is responsible for serializing it and exposing to Garbage Collection .属性是本机的：C ++代码负责对其进行序列化并暴露给Garbage Collection。

NoClear     Prevents this Object reference from being set to none from the editor. Hides clear (and browse) button in the editor. 防止在编辑器中将此“对象”引用设置为“ none”。 在编辑器中隐藏清除（和浏览）按钮。

NoExport        Only useful for native classes. This property should not be included in the auto-generated class declaration.仅对本机类有用。 此属性不应包含在自动生成的类声明中。

NonPIEDuplicateTransient        The property will be reset to the default value during duplication, except if it's being duplicated for a Play In Editor (PIE) session.该属性将在复制过程中重置为默认值，除非要在“播放编辑器”（PIE）会话中复制该属性。？？？

NonTransactional        Indicates that changes to this property's value will not be included in the editor's undo/redo history.指示对该属性值的更改将不包括在编辑器的撤消/重做历史中。

NotReplicated       Skip replication. This only applies to struct members and parameters in service request functions.跳过复制。 这仅适用于服务请求函数中的结构成员和参数。

Replicated      The property should be replicated over the network.该属性应通过网络复制。

ReplicatedUsing=FunctionName        The ReplicatedUsing Specifier specifies a callback function which is executed when the property is updated over the network. ReplicatedUsing说明符指定在通过网络更新属性时执行的回调函数。

RepRetry        Only useful for struct properties. Retry replication of this property if it fails to be fully sent (for example, Object references not yet available to serialize over the network). For simple references, this is the default, but for structs, this is often undesirable due to the bandwidth cost, so it is disabled unless this flag is specified.仅对结构属性有用。 如果无法完全发送此属性，请重试该属性的复制（例如，对象引用尚不可用于通过网络进行序列化）。 对于简单引用，这是默认设置，但对于结构，由于带宽成本，通常不希望这样做，因此除非指定此标志，否则将其禁用。

SaveGame        This Specifier is a simple way to include fields explicitly for a checkpoint/save system at the property level. The flag should be set on all fields that are intended to be part of a saved game, and then a proxy archiver can be used to read/write it.此说明符是在属性级别显式包括检查点/保存系统字段的简单方法。 该标志应该在打算成为已保存游戏一部分的所有字段上设置，然后可以使用代理存档器对其进行读取/写入。

SerializeText       Native property should be serialized as text (ImportText, ExportText).本机属性应序列化为文本（ImportText，ExportText）。

SkipSerialization       This property will not be serialized, but can still be exported to a text format (such as for copy/paste operations).该属性不会被序列化，但是仍然可以导出为文本格式（例如用于复制/粘贴操作）。

SimpleDisplay       Visible or editable properties appear in the Details panel and are visible without opening the "Advanced" section.可见或可编辑的属性出现在“详细信息”面板中，并且在不打开“高级”部分的情况下可见。

TextExportTransient     This property will not be exported to a text format (so it cannot, for example, be used in copy/paste operations).此属性不会导出为文本格式（因此，例如，不能在复制/粘贴操作中使用）。

Transient       Property is transient, meaning it will not be saved or loaded. Properties tagged this way will be zero-filled at load time.属性是瞬态的，这意味着将不会保存或加载它。 用这种方式标记的属性将在加载时填充零。

VisibleAnywhere     Indicates that this property is visible in all property windows, but cannot be edited. This Specifier is incompatible with the "Edit" Specifiers.指示此属性在所有属性窗口中都是可见的，但是无法进行编辑。 该说明符与“编辑”说明符不兼容。

VisibleDefaultsOnly     Indicates that this property is only visible in property windows for archetypes, and cannot be edited. This Specifier is incompatible with any of the "Edit" Specifiers.指示此属性仅在原型的属性窗口中可见，并且无法编辑。 该说明符与任何“编辑”说明符都不兼容。

VisibleInstanceOnly     Indicates that this property is only visible in property windows for instances, not for archetypes, and cannot be edited. This Specifier is incompatible with any of the "Edit" Specifiers.指示此属性仅在实例的属性窗口中可见，对于原型不可见，并且无法编辑。 该说明符与任何“编辑”说明符都不兼容。

## Metadata Specifiers
When declaring Classes, interfaces, structs, enums, enum values, functions, or properties, you can add Metadata Specifiers to control how they behave with various aspects of the engine and the editor. Each type of data structure or member has its own list of Metadata Specifiers.在声明类，接口，结构，枚举，枚举值，函数或属性时，可以添加元数据说明符来控制它们在引擎和编辑器各个方面的行为。 每种类型的数据结构或成员都有其自己的元数据说明符列表。

Property Meta Tag       Effect

AllowAbstract="true/false"      Used for Subclass and SoftClass properties. Indicates whether abstract Class types should be shown in the Class picker.用于子类和SoftClass属性。 指示是否应在类选择器中显示抽象的类类型。

AllowedClasses="Class1, Class2, .."     Used for FSoftObjectPath properties. Comma delimited list that indicates the Class type(s) of assets to be displayed in the Asset picker.用于FSoftObjectPath属性。 以逗号分隔的列表，指示要在资产选择器中显示的资产的类类型。？？？

AllowPreserveRatio      Used for FVector properties. It causes a ratio lock to be added when displaying this property in details panels.用于FVector属性。 在详细信息面板中显示此属性时，将导致比率锁定被添加。？？？

ArrayClamp="ArrayProperty"      Used for integer properties. Clamps the valid values that can be entered in the UI to be between 0 and the length of the array property named. 用于整数属性。 将可以在UI中输入的有效值限制在0到指定的数组属性的长度之间。

AssetBundles        Used for SoftObjectPtr or SoftObjectPath properties. List of Bundle names used inside Primary Data Assets to specify which Bundles this reference is part of. 用于SoftObjectPtr或SoftObjectPath属性。 主要数据资产中用于指定此引用属于哪些捆绑软件的捆绑软件名称列表。？？？

BlueprintBaseOnly       Used for Subclass and SoftClass properties. Indicates whether only Blueprint Classes should be shown in the Class picker.用于子类和SoftClass属性。 指示是否仅在类选择器中显示“蓝图类”。

BlueprintCompilerGeneratedDefaults      Property defaults are generated by the Blueprint compiler and will not be copied when the CopyPropertiesForUnrelatedObjects function is called post-compile.属性默认值是由Blueprint编译器生成的，当将CopyPropertiesForUnrelatedObjects函数称为后编译时，将不会复制该属性默认值。

ClampMin="N"        Used for float and integer properties. Specifies the minimum value N that may be entered for the property.用于float和integer属性。 指定可为该属性输入的最小值N。

ClampMax="N"        Used for float and integer properties. Specifies the maximum value N that may be entered for the property.用于float和integer属性。 指定可以为属性输入的最大值N。

ConfigHierarchyEditable     This property is serialized to a config (.ini) file, and can be set anywhere in the config hierarchy.此属性被序列化到config（.ini）文件，并且可以在config层次结构中的任何位置进行设置。

ContentDir      Used by FDirectoryPath properties. Indicates that the path will be picked using the Slate-style directory picker inside the Content folder.由FDirectoryPath属性使用。 指示将使用Content文件夹中的Slate样式目录选择器选择路径。

DisplayAfter="PropertyName"     This property will show up in the Blueprint Editor immediately after the property named PropertyName, regardless of its order in source code, as long as both properties are in the same category. If multiple properties have the same DisplayAfter value and the same DisplayPriority value, they will appear after the named property in the order in which they are declared in the header file.不论其在源代码中的顺序如何，只要两个属性都在同一类别中，该属性就会立即在“蓝图编辑器”中显示，该属性名为PropertyName。 如果多个属性具有相同的DisplayAfter值和相同的DisplayPriority值，它们将按照在头文件中声明的顺序出现在命名属性的后面。

DisplayName="Property Name"     The name to display for this property, instead of the code-generated name.要为此属性显示的名称，而不是代码生成的名称。

DisplayPriority="N"     If two properties feature the same DisplayAfter value, or are in the same category and do not have the DisplayAfter Meta Tag, this property will determine their sorting order. The highest-priority value is 1, meaning that a property with a DisplayPriority value of 1 will appear above a property with a DisplayProirity value of 2. If multiple properties have the same DisplayAfter value, they will appear in the order in which they are declared in the header file.如果两个属性具有相同的DisplayAfter值，或者属于同一类别，并且没有DisplayAfter元标记，则此属性将确定其排序顺序。 最高优先级值为1，这意味着DisplayPriority值为1的属性将出现在DisplayProirity值为2的属性之上。如果多个属性具有相同的DisplayAfter值，则它们将按照声明的顺序出现 在头文件中。

DisplayThumbnail="true"     Indicates that the property is an Asset type and it should display the thumbnail of the selected Asset.指示该属性是资产类型，并且应显示所选资产的缩略图。

EditCondition="BooleanPropertyName"     Names a boolean property that is used to indicate whether editing of this property is disabled. Putting "!" before the property name inverts the test.命名一个布尔属性，该布尔属性用于指示是否禁用此属性的编辑。 在属性名称前放入“！” 反转测试。
The EditCondition meta tag is no longer limited to a single boolean property. It is now evaluated using a full-fledged expression parser, meaning you can include a full C++ expression. 元标记EditCondition不再限于单个布尔属性。 现在使用完整的表达式解析器对其进行评估，这意味着您可以包含完整的C ++表达式。

EditFixedOrder      Keeps the elements of an array from being reordered by dragging.防止通过拖动对数组的元素进行重新排序。

ExactClass="true"       Used for FSoftObjectPath properties in conjunction with AllowedClasses. Indicates whether only the exact Classes specified in AllowedClasses can be used, or if subclasses are also valid.用于FSoftObjectPath属性和AllowedClasses。 指示是否只能使用AllowedClasses中指定的确切类，或者子类也有效。

ExposeFunctionCategories="Category1, Category2, .."     Specifies a list of categories whose functions should be exposed when building a function list in the Blueprint Editor.指定在“蓝图编辑器”中构建功能列表时应公开其功能的类别列表。

ExposeOnSpawn="true"        Specifies whether the property should be exposed on a Spawn Actor node for this Class type. 指定是否应在此Class类型的Spawn Actor节点上公开该属性。

FilePathFilter="FileType"       Used by FFilePath properties. Indicates the path filter to display in the file picker. Common values include "uasset" and "umap", but these are not the only possible values.由FFilePath属性使用。 指示要在文件选择器中显示的路径过滤器。 常见的值包括“ uasset”和“ umap”，但这不是唯一可能的值。

GetByRef        Makes the "Get" Blueprint Node for this property return a const reference to the property instead of a copy of its value. Only usable with Sparse Class Data, and only when NoGetter is not present.使此属性的“获取”蓝图节点返回对该属性的const引用，而不是其值的副本。 仅适用于稀疏类数据，并且仅当不存在NoGetter时可用。

HideAlphaChannel        Used for FColor and FLinearColor properties. Indicates that the Alpha property should be hidden when displaying the property widget in the details.用于FColor和FLinearColor属性。 指示在详细信息中显示属性窗口小部件时应隐藏Alpha属性。

HideViewOptions     Used for Subclass and SoftClass properties. Hides the ability to change view options in the Class picker.用于子类和SoftClass属性。 隐藏在类选择器中更改视图选项的功能。

InlineEditConditionToggle       Signifies that the boolean property is only displayed inline as an edit condition toggle in other properties, and should not be shown on its own row.表示布尔属性仅在其他属性中作为编辑条件切换时以内联方式显示，并且不应在其自己的行上显示。

LongPackageName     Used by FDirectoryPath properties. Converts the path to a long package name.由FDirectoryPath属性使用。 将路径转换为长包名称。

MakeEditWidget      Used for Transform or Rotator properties, or Arrays of Transforms or Rotators. Indicates that the property should be exposed in the viewport as a movable widget.用于“变换”或“旋转器”属性，或“变换或旋转器数组”。 指示该属性应作为可移动窗口小部件公开在视口中。

NoGetter        Causes Blueprint generation not to generate a "get" Node for this property. Only usable with Sparse Class Data.使蓝图生成不为此属性生成“获取”节点。 仅可用于稀疏类别数据。

Tags        
Variable UPROPERTY UENUM Enum Bitmask Bitfield