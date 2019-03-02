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