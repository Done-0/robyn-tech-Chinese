### [GraphQL 支持（with strawberry 🍓）](https://robyn.tech/documentation/api_reference/graphql-support#graphql-support-with-strawberry-)

目前，GraphQL 支持处于初步摸索阶段。当我们能为视图和视图控制器提供稳定的 API 时，将推出更加稳定的版本。

#### [第 1 步：创建虚拟环境](https://robyn.tech/documentation/api_reference/graphql-support#step-1-creating-a-virtualenv)

为了确保依赖关系互相独立，我们将使用虚拟环境。

```bash
python3 -m venv venv
```

#### [第 2 步：激活虚拟环境并安装 Robyn](https://robyn.tech/documentation/api_reference/graphql-support#step-2-activate-the-virtualenv-and-install-robyn)

```bash
source venv/bin/activate
```

```bash
pip install robyn strawberry-graphql
```

#### [第 3 步：编写 Robyn 应用](https://robyn.tech/documentation/api_reference/graphql-support#step-3-coding-the-app)

```python
from typing import List, Optional
from robyn import Robyn, jsonify
import json

import dataclasses
import strawberry
import strawberry.utils.graphiql


@strawberry.type
class User:
  name: str


@strawberry.type
class Query:
  @strawberry.field
  def user(self) -> Optional[User]:
      return User(name="Hello")


schema = strawberry.Schema(Query)

app = Robyn(__file__)


@app.get("/", const=True)
async def get():
  return strawberry.utils.graphiql.get_graphiql_html()


@app.post("/")
async def post(request):
  body = request.json()
  query = body["query"]
  variables = body.get("variables", None)
  context_value = {"request": request}
  root_value = body.get("root_value", None)
  operation_name = body.get("operation_name", None)

  data = await schema.execute(
      query,
      variables,
      context_value,
      root_value,
      operation_name,
  )

  return jsonify(
      {
          "data": (data.data),
          **({"errors": data.errors} if data.errors else {}),
          **({"extensions": data.extensions} if data.extensions else {}),
      }
  )


if __name__ == "__main__":
  app.start(port=8080, host="0.0.0.0")
```

接下来，让我们逐行解读以上代码。

这些语句导入了所需的依赖项。

```python
from typing import List, Optional
from robyn import Robyn, jsonify
import json
import dataclasses
import strawberry
import strawberry.utils.graphiql
```

在这里，我们实现了一个基本的 `User` 类，并为其定义了 `name` 属性。

然后，我们创建了一个 GraphQl 查询类（`Query` 类型），该查询返回一个 `User` 实例。

```python
@strawberry.type
class User:
    name: str


@strawberry.type
class Query:
    @strawberry.field
    def user(self) -> Optional[User]:
        return User(name="Hello")


schema = strawberry.Schema(Query)
```

接下来，我们初始化了一个 Robyn 应用。为了提供 GraphQl 应用，我们需要定义两个路由：一个 GET 路由用于返回 `GraphiQL(ide)`，另一个 POST 路由用于处理 `GraphQl` 请求。

```python
app = Robyn(__file__)
```

我们通过 GraphiQL IDE 使用 `strawberry` 填充 HTML 页面，并设置 `const=True` 以预计算页面内容，从而加快返回速度，避免了 GET 请求的额外执行开销。

```python
@app.get("/", const=True)
  async def get():
  return strawberry.utils.graphiql.get_graphiql_html()
```

最后，我们从 `request` 对象中获取参数（如 body、query、variables、context_value、root_value 和 operation_name）。

```python
@app.post("/")
async def post(request):
body = request.json()
query = body["query"]
variables = body.get("variables", None)
context_value = {"request": request}
root_value = body.get("root_value", None)
operation_name = body.get("operation_name", None)

data = await schema.execute(
    query,
    variables,
    context_value,
    root_value,
    operation_name,
)

return jsonify(
    {
        "data": (data.data),
        **({"errors": data.errors} if data.errors else {}),
        **({"extensions": data.extensions} if data.extensions else {}),
    }
)
```

上述代码展示了如何为 Robyn 应用添加 GraphQL 支持。您可以为任意数量的路由执行类似操作。

### [下一步](https://robyn.tech/documentation/api_reference/graphql-support#whats-next)

这就是目前的所有内容。:D 请留意我们页面上的更多更新，我们将继续添加更多示例和文档。
