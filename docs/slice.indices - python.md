---
description:
aliases: 
tags: 
created: 2023-05-17T16:09:52
updated: 2023-07-15T21:33:03
title: slice.indices - python
---
- https://stackoverflow.com/questions/22151335/implementing-getitem
- [slice.indices(self, length) - python doc](https://docs.python.org/3/reference/datamodel.html?highlight=slice#slice.indices)

> This method takes a single integer argument length and computes information about the slice that the slice object would describe if applied to a sequence of length items. It returns a tuple of three integers; respectively these are the start and stop indices and the step or stride length of the slice. Missing or out-of-bounds indices are handled in a manner consistent with regular slices.

```python
start, stop, step = slice_object.indices(len(data))
return [data[i] for i in range(start, stop, step)]
```
