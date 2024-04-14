---
aliases: 
tags: 
description:
title: save both relations {typeorm}
created: 2024-04-14T22:58:05
updated: 2024-04-14T22:58:06
---

> Let's save a photo, and its metadata and attach them to each other.

```ts
const photo = new Photo();
const metadata = new PhotoMetadata();
...
// get entity relationships
const photoRepository = AppDataSource.getRepository(Photo);
const metadataRepository = AppDataSource.getRepository(PhotoMetadata);

// first we should save a photo
await photoRepository.save(photo);

// photo is saved. Now we need to save a photo metadata
await metadataRepository.save(metadata);
```
