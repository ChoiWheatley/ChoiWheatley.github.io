---
description:
aliases: 
created: 2023-02-10T16:37:18
tags: []
source: https://stackoverflow.com/questions/32911729/understanding-how-lambda-closure-type-has-deleted-default-constructor
author: AntiElephant
date created: Friday, February 10th 2023, 4:37:18 pm
date modified: Monday, February 27th 2023, 6:20:45 pm
updated: 2023-07-11T15:21:09
title: c++ - Understanding how Lambda closure type has deleted default constructor - Stack Overflow
---

# c++ - Understanding how Lambda closure type has deleted default constructor - Stack Overflow

> ## Excerpt
> From 5.1.2 
  [19] The closure type associated with a lambda-expression has a deleted (8.4.3) default constructor and a deleted
  copy assignment operator. It has an implicitly-declared copy const...

---
From 5.1.2

> \[19\] The closure type associated with a lambda-expression **has a deleted (8.4.3) default constructor** and a deleted copy assignment operator. It has an implicitly-declared copy constructor (12.8) and may have an implicitlydeclared move constructor (12.8). \[ Note: The copy/move constructor is implicitly defined in the same way as any other implicitly declared copy/move constructor would be implicitly defined. —end note \]

I'm reading through C++ Primer 14.8.1 which explains lambda expressions being translated by the compiler in to an unnamed object of an unnamed class. How is it I can I define objects of lambda functions which do not contain a lambda capture, if the default constructor is deleted?

```c++
 auto g = [](){};
```

Is this not conceptually the same as...

``` c++
 class lambdaClass{
 public:
      lambdaClass() = delete;
      lambdaClass& operator=(const lambdaClass&) = delete;
      void operator()(){ }

      //copy/move constructor and destructor implicitly defined
};

auto g = lambdaClass(); //would be an error since default is deleted.
```

If there was a capture then a constructor _other_ than the default constructor would be defined and it would be okay to initialise objects of such (as long as a parameter was passed). But if there is no capture and the default constructor is deleted, it doesn't seem consistent conceptually that a lambda class object can be created.

Edit: Hmm, maybe the conception that the lambda class creates constructors depending on its lambda captures is unfounded despite this being how it's described in C++ Primer (I can find no quote of it in the standard), because the following code does not work even though I would expect it to conceptually:

```c++
int sz = 2;
auto a = [sz](){ return sz;};
decltype(a) b(10); //compiler error
decltype(a) b = a; //all good though
```
