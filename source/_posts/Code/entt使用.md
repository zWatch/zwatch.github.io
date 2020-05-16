---
title: entt使用
date: 2019-11-23 20:25:34
updated:
tags:
categories:
  - C++
  - Third
  - entt
---

# ?????
## ??  

radix_sort<base, store>  
insertion_sort
std_sort

## family 
  using a_family = entt::family<struct a_family_type>;
  auto t1 = a_family::type<int>;//t1 is unsigned int  
  ??????????t1???????????
## hashed_string to uint32
  ?string???uint32
  entt::hashed_string::to_value(foobar)==0xbf9cf968
