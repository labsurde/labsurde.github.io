---
layout: post
title: "Range Based For loop"
date:   2021-01-03 17:00:00 +0900
categories: C++
---

# Range-based for loop

From [cppreference](https://en.cppreference.com/w/cpp/language/range-for), syntax is like the following:

```cpp
attr(optional) for ( range_declaration : range_expression ) loop_statement		(until C++20)
```

_range_expression_ works when object in range meets one of the following:
0. object's type is array
1. object has member functions, `begin()`, and `end()`, or
2. `std::begin()` and `std::end()` exist

Initializer list can be used as a _range_expression_.
(`std::begin()` and `std::end()` exists.)

```cpp
    int a[] = {0, 1, 2, 3, 4, 5};
    for (int n : a) // the initializer may be an array
        std::cout << n << ' ';
```
