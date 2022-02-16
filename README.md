# Typeorm CheatSheet

## Casecade SoftDelete
```js
const query = entityManager.getRepository("user");
const userWithRelations = await query.findOneOrFail(
          { id: 1 }, // userid
          {
            relations: [
              'posts',
              'posts.comments'
            ],
          },
        );
await query.softRemove(userWithRelations);
```

## Get data including softDeleted
```js
  query.withDeleted().getMany();
```

## Include nested relation in sql query
```js
const query = entityManager.getRepository("user");
const findOneUserWithNestedRelations = await query.findOneOrFail(
          { id: 1 },
          {
            relations: ['posts', 'posts.comments'],
          },
        );
```
**or**
```js
const query = getConnection().createQueryBuilder("user", "user");
const findOneUserWithNestedRelations =  await query.innerJoinAndSelect("user.posts", "post")
          .innerJoinAndSelect("post.comments", "comment")
          .where({
            id : 1
          }).getOneOrFail();
```
