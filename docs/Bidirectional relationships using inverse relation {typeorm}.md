---
aliases: 
tags: 
description:
title: Bidirectional relationships using inverse relation {typeorm}
created: 2024-04-14T22:56:11
updated: 2024-04-14T22:56:12
---

> Bidirectional relationships using *inverse relation*

```ts
// entity/PhotoMetadata.ts

@Entity()
export class PhotoMetadata {
	...
	@OneToOne(type => Photo, (photo) => photo.metadata)
	@JoinColumn()
	photo: Photo;
}

// entity/Photo.ts

@Entity()
export class Photo {
	...
	@OneToOne(type => PhotoMetadata, (photoMetadata) => photoMetadata.photo)
	metadata: PhotoMetadata;
}
```
