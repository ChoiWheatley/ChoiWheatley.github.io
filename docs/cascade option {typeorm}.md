---
aliases: 
tags: 
description:
title: cascade option {typeorm}
created: 2024-04-14T22:56:42
updated: 2024-04-14T22:56:43
---

> `cascade` 옵션을 주어 부모 테이블에 일어난 변화를 자식 테이블도 같이 수정되도록 만들 수 있다.

```ts
export class Photo {
	...
	@OneToOne(type => PhotoMetadata, (metadata) => metadata.photo, {
		cascade: true,
	})
	metadata: PhotoMetadata;
}

///

const photo = new Photo();
const metadata = new PhotoMetadata();
...
photo.metadata = metadata; // this way we CONNECT them

await photoRepository.save(photo);
```
