---
aliases: 
tags: 
description:
title: save related entities only with PK {typeorm}
created: 2024-12-18T18:26:03
updated: 2024-12-18T18:27:46
---

> TypeORM 엔티티 인스턴스를 생성할 때 연관 인스턴스 없이 식별키만 가지고 만들 수 있나요?

Yes, you can create an instance of a TypeORM entity and assign a primary key directly without fetching the related relational entity. This approach is useful when you only have the primary key of a related entity and do not need the full relational object.

Here’s how you can do it:

### Example Scenario

Assume you have two entities: `User` and `Post`, where a `Post` entity has a many-to-one relationship with the `User` entity.

```typescript
@Entity()
export class User {
    @PrimaryGeneratedColumn()
    id: number;

    @Column()
    name: string;

    @OneToMany(() => Post, (post) => post.user)
    posts: Post[];
}

@Entity()
export class Post {
    @PrimaryGeneratedColumn()
    id: number;

    @Column()
    title: string;

    @ManyToOne(() => User, (user) => user.posts)
    user: User;
}
```

### Creating a `Post` with only the `User` ID

You can create an instance of the `Post` entity by assigning the `user` property with only the `User` entity's primary key, like this:

```typescript
const userId = 1; // Assume this is the ID of an existing user

// Create a new Post instance
const post = new Post();
post.title = "New Post";
post.user = { id: userId } as User; // Assign only the primary key

// Save the Post
await postRepository.save(post);
```

### Explanation

- In the above code, `post.user` is assigned a partial `User` object containing only the `id`.
- TypeORM understands this and uses the `id` to set up the relationship without fetching the full `User` entity from the database.

### Notes

1. **Validation**: Ensure the referenced primary key exists in the database to avoid foreign key constraint errors.
2. **Performance**: This method avoids an extra query to fetch the related entity, improving performance when the primary key is already known.
3. **Type Safety**: Using `as User` ensures TypeScript does not complain about assigning a partial object.

This approach is clean and efficient when you do not need to load the full relational object.
