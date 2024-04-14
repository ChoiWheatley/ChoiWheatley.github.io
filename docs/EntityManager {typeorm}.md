---
aliases: 
tags: 
description:
title: EntityManager {typeorm}
created: 2024-04-14T22:59:04
updated: 2024-04-14T22:59:06
---

> Save data with `EntityManager`

```ts
import {Photo} from './entity/Photo';
import {AppDataSource} from './index';

const photo = new Photo();
await AppDataSource.manager.save(photo);
```
