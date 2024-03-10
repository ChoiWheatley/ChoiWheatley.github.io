---
aliases: 
tags: 
description:
title: enum column type {typeorm}
created: 2024-03-10T13:58:08
updated: 2024-03-10T13:58:27
---
- <https://typeorm.io/entities#enum-column-type>

```typescript
export enum UserRole {
    ADMIN = "admin",
    EDITOR = "editor",
    GHOST = "ghost",
}

@Entity()
export class User {
    @PrimaryGeneratedColumn()
    id: number

    @Column({
        type: "enum",
        enum: UserRole,
        default: UserRole.GHOST,
    })
    role: UserRole
}
```
