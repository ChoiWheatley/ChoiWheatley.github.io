---
aliases: 
tags: 
description:
title: DataSource {typeorm}
created: 2024-04-14T22:59:22
updated: 2024-04-14T22:59:23
---

> Let's create our `DataSource`

```ts
import 'reflect-metadata';
import {DataSource} from 'typeorm';
import {Photo} from './entity/Photo';

const AppDataSource = new DataSource({
	type: 'postgres',
	host: 'localhost',
	port: 5432,
	username: 'root',
	password: 'admin',
	database: 'test',
	entities: [Photo],
	synchronize: true,
	logging: false,
});

// call `initialize` method of a newly created database
// ONCE in your application bootstrap
AppDataSource.initialize()
	.then(() => {
		// here you can start to work with your database
	})
	.catch((error) => console.log(error));
```
