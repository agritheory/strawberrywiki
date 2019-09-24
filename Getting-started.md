Demo schema:

```py
import asyncio

import typing

import strawberry


@strawberry.type
class User:
    name: str
    age: int

    @strawberry.field
    async def test(self, info) -> str:
        await asyncio.sleep(1)

        return "wait a"


@strawberry.type
class Query:
    @strawberry.field
    def hello(self, info, times: int) -> str:
        # TODO: self is None here

        return f"Hello times: {times}"

    @strawberry.field
    def image(self, info, width: int, height: int) -> str:
        return f"image {width}x{height}"

    @strawberry.field
    def user(self, info) -> User:
        return User(name="Patrick", age=123)


schema = strawberry.Schema(query=Query)
```

```
pip install strawberry-graphql
```

```sh
strawberry server app
```

## Basic usage of the graphql interface with this example

You should be able to see the server running and interface that looks this:

![](strawberrygraphqlgui.png)


Once the server is running, you try out the GraphQL endpoint with the following queries:

`hello`, where you can provide an integer argument, like `5`.
```graphql
# Write your query or mutation here
{
  hello(times: 5)
}
```
Which should return output that looks like:
```json
{
  "data": {
    "hello": "Hello times: 5"
  }
}
```

`image`, which takes two integer arguments:
```graphql
{
  image(height:100, width: 100)
}
```

`user`, which returns an object with two fields, `age` and `name`.
```graphql
# Write your query or mutation here
{
  user {
    name
    age
  }
}
```
The values you specify in the query are the ones that will be returned, but to return "all" of the user fields you would have to write a [fragment](https://graphql.org/learn/queries/#fragments).
