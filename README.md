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

## Transaction
```js
   await getConnection().transaction(async (entityManager) => {
          const query = await entityManager.getRepository("user");
          const findRes = await query.find({
            where: {
              id: 1,
            },
            relations: ['posts'],
          });
          const dbres = await query.softRemove(findRes);
          return {
            affected: dbres.length,
          };
        }),
```
## Using select in SQL query
```js
const query = getConnection().createQueryBuilder("user", "user");
const findOneUserWithNestedRelations =  await query.innerJoinAndSelect("user.posts", "post")
          .innerJoinAndSelect("post.comments", "comment")
          .where({
            id : 1
          })
          .select([
              'user.id',
              'post',  //Note: to get the `comment` you have to add parent relationship  `post` as well.
              'comment.id'
          ])
          getOneOrFail();
```
