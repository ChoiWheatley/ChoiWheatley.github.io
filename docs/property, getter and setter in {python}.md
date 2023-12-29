---
aliases: 
tags: 
description:
title: property, getter and setter in {python}
created: 2023-08-23T15:35:19
updated: 2023-08-23T15:35:20
---
<https://realpython.com/python-property/>


```python
# circle.py
class Circle:
    def __init__(self, radius):
        self._radius = radius

    @property
    def radius(self):
        """The radius property."""
        print("Get radius")
        return self._radius

    @radius.setter
    def radius(self, value):
        print("Set radius")
        self._radius = value

    @radius.deleter
    def radius(self):
        print("Delete radius")
        del self._radius
```
