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
