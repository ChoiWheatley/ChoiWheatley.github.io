---
aliases: 
tags: 
description:
title: room, player, host ER {swjungle} {my own weapon}
created: 2023-11-30T15:14:41
updated: 2023-11-30T16:33:37
---
- [[week14-18 {swjungle}{my own weapon}{nestjs, socketio}]]
___

## README

ORM을 쓰는 이상, ERD와 완벽하게 호환이 되는 그림을 그릴 수 없다. 따라서, 간소화된 객체 관계도를 그려 지도를 그려보는 것으로 선회한다.

## Object Relation(?)

**room.entity**

```ts
class Room {
	room_id: number;
	host: Promise<Host>;
	status: string;
	user_num: nubmer;
	current_user_num: number;
	players: Promise<Player[]>;
}
```

**catch.game.entity**

```ts
class CatchGame extends Room {
    ans: string;
}
```

**redgreen.game.entity**

```ts
class RedGreenGame extends Room {
    length: number;
    win_num: number;
    /**
     * 영희가 고개를 돌렸는지 여부
     */
    killer_mode: boolean;
}

```

**host.entity**

```ts
export class Host {
    uuid: string;
    host_id: number;
    // 방과 일대 일 관계
    room: Promise<Room>;
}
```

**player.entity**

```ts

```