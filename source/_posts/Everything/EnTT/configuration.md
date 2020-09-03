# Crash Course: configuration



Michele Caini edited this page 14 days ago · [2 revisions](https://github.com/skypjack/entt/wiki/Crash-Course:-configuration/_history)

# Crash Course: configuration

# Table of Contents

- [Introduction](#introduction)
- [Definitions](#Definitions)
  - [ENTT_STANDALONE](#entt_standalone)
  - [ENTT_NOEXCEPT](#entt_noexcept)
  - [ENTT_HS_SUFFIX and ENTT_HWS_SUFFIX](#entt_hs_suffix_and_entt_hws_suffix)
  - [ENTT_USE_ATOMIC](#entt_use_atomic)
  - [ENTT_ID_TYPE](#entt_id_type)
  - [ENTT_PAGE_SIZE](#entt_page_size)
  - [ENTT_ASSERT](#entt_assert)
  - [ENTT_NO_ETO](#entt_no_eto)
  - [ENTT_STANDARD_CPP](#entt_standard_cpp)

# Introduction

`EnTT` doesn't offer many hooks for customization but it certainly offers some.
In the vast majority of cases, users will have no interest in changing the default parameters. For all other cases, the list of possible configurations with which it's possible to adjust the behavior of the library at runtime can be found below.
EnTT`没有提供很多用于自定义的挂钩，但是它确实提供了一些挂钩。
在大多数情况下，用户对更改默认参数没有兴趣。 对于所有其他情况，可以在下面找到可以用来在运行时调整库行为的可能配置的列表。

# Definitions

All options are intended as parameters to the compiler (or user-defined macros within the compilation units, if preferred).
所有选项均用作编译器的参数（或编译单元中用户定义的宏，如果需要的话）。

Each parameter can result in internal library definitions. It's not recommended to try to also modify these definitions, since there is no guarantee that they will remain stable over time unlike the options below.
每个参数都可以导致内部库定义。 不建议尝试也修改这些定义，因为不能保证它们会随着时间的推移保持稳定，这与下面的选项不同。

## ENTT_STANDALONE

`EnTT` is designed in such a way that it works (almost) everywhere out of the box. However, this is the result of many refinements over time and a compromise regarding some optimizations.
EnTT`的设计方式使其可以（几乎）在所有开箱即用的地方工作。 但是，这是随着时间的推移进行了许多改进以及在某些优化方面有所妥协的结果。

It's worth noting that users can get a small performance boost by passing this definition to the compiler when the library is used in a standalone application.
值得注意的是，当在独立应用程序中使用该库时，通过将此定义传递给编译器，用户可以获得较小的性能提升。

## ENTT_NOEXCEPT

The purpose of this parameter is to suppress the use of `noexcept` by this library.
To do this, simply define the variable without assigning any value to it.
该参数的目的是禁止该库使用`noexcept`。
为此，只需定义变量而无需为其分配任何值。

## ENTT_HS_SUFFIX and ENTT_HWS_SUFFIX

The `hashed_string` class introduces the `_hs` and `_hws` suffixes to accompany its user defined literals.
`hashed_string`类引入了`_hs`和`_hws`后缀，以附带其用户定义的文字。

In the case of conflicts or even just to change these suffixes, it's possible to do so by associating new ones with these definitions.
在发生冲突或什至只是更改这些后缀的情况下，可以通过将新的后缀与这些定义相关联来实现。

## ENTT_USE_ATOMIC

In general, `EnTT` doesn't offer primitives to support multi-threading. Many of the features can be split over multiple threads without any explicit control and the user is the only one who knows if and when a synchronization point is required.
However, some features aren't easily accessible to users and can be made thread-safe by means of this definition.
通常，`EnTT`不提供支持多线程的原语。 许多功能可以拆分为多个线程，而无需任何显式控制，并且用户是唯一知道是否以及何时需要同步点的用户。
但是，某些功能对于用户来说并不容易访问，并且可以通过此定义使其成为线程安全的。

## ENTT_ID_TYPE

`entt::id_type` is directly controlled by this definition and widely used within the library.
By default, its type is `std::uint32_t`. However, users can define a different default type if necessary.
entt :: id_type直接受此定义控制，并在库中广泛使用。
默认情况下，其类型为`std :: uint32_t`。 但是，如果需要，用户可以定义其他默认类型。

## ENTT_PAGE_SIZE

As is known, the ECS module of `EnTT` is based on *sparse sets*. What is less known perhaps is that these are paged to reduce memory consumption in some corner cases.
众所周知，“ EnTT”的ECS模块基于*稀疏集*。 也许鲜为人知的是，在某些特殊情况下，对它们进行分页以减少内存消耗。
The default size of a page is 32kB but users can adjust it if appropriate. In all case, the chosen value **must** be a power of 2.
页面的默认大小为32kB，但用户可以根据需要对其进行调整。 在所有情况下，所选值**必须**为2的幂。

## ENTT_ASSERT

For performance reasons, `EnTT` doesn't use exceptions or any other control structures. In fact, it offers many features that result in undefined behavior if not used correctly.
出于性能原因，`EnTT`不使用异常或任何其他控制结构。 实际上，它提供了许多功能，如果使用不正确，将导致不确定的行为。

To get around this, the library relies on a lot of `assert`s for the purpose of detecting errors in debug builds. However, these assertions may in turn affect performance to an extent.
为了解决这个问题，该库依赖于许多`assert`来检测调试版本中的错误。 但是，这些断言可能反过来在一定程度上影响性能。
This option is meant to disable all controls. 此选项旨在禁用所有控件(control structure)。

## ENTT_NO_ETO

In order to reduce memory consumption and increase performance, empty types are never stored by the ECS module of `EnTT`.
Use this variable to treat these types like all others and therefore to create a dedicated storage for them.
为了减少内存消耗并提高性能，EnTT的ECS模块从不存储空类型。
使用此变量将这些类型与所有其他类型一样对待，并因此为它们创建专用存储。

## ENTT_STANDARD_CPP

After many adventures, `EnTT` finally works fine across boundaries.
To do this, the library mixes some non-standard language features with others that are perfectly compliant.
This definition will prevent the library from using non-standard techniques, that is, functionalities that aren't fully compliant with the standard C++.
经过许多冒险之后，`EnTT`最终可以跨边界正常工作。
为此，该库将一些非标准语言功能与其他完全兼容的功能混合在一起。
此定义将阻止该库使用非标准技术，即与标准C ++不完全兼容的功能。