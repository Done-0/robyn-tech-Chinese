### [OpenAPI 文档（Swagger）](https://robyn.tech/documentation/api_reference/openapi#openapi-docs-aka-swagger)

部署应用程序后，蝙蝠侠收到了大量关于如何使用接口的问题。为了解决这一问题，Robyn 向他介绍了 OpenAPI 规范，该规范能够自动生成接口文档。

开箱即用，Robyn 为应用程序提供了以下默认接口：

- `/docs` - Swagger UI 界面

- `/openapi.json` - JSON 格式的 OpenAPI 文档

若需要使用自定义的 OpenAPI 配置，您可以：

- 将 `openapi.json` 配置文件放在应用程序的根目录中。

- 或者，将文件路径作为参数传递给 `Robyn()` 构造函数中的 `openapi_file_path` 参数（参数的优先级高于文件）。

如果您不希望生成 OpenAPI 文档，可以在启动应用程序时传递 `--disable-openapi` 参数来禁用该功能。

```python
python app.py --disable-openapi
```

### [如何使用?](https://robyn.tech/documentation/api_reference/openapi#how-to-use)

- 查询参数：通过继承 `QueryParams` 类来定义参数类型。
- 路径参数默认为字符串类型（参考：[https://en.wikipedia.org/wiki/Query_string](https://en.wikipedia.org/wiki/Query_string)）

```python
from robyn import Robyn
from robyn.robyn import QueryParams

app = Robyn(
    file_object=__file__,
    openapi=OpenAPI(
        info=OpenAPIInfo(
            title="Sample App",
            description="This is a sample server application.",
            termsOfService="https://example.com/terms/",
            version="1.0.0",
            contact=Contact(
                name="API Support",
                url="https://www.example.com/support",
                email="support@example.com",
            ),
            license=License(
                name="BSD2.0",
                url="https://opensource.org/license/bsd-2-clause",
            ),
            externalDocs=ExternalDocumentation(description="Find more info here", url="https://example.com/"),
            components=Components(),
        ),
    ),
)


@app.get("/")
async def welcome():
    """欢迎"""
    return "hi"


class GetRequestParams(QueryParams):
    appointment_id: str
    year: int


@app.get("/api/v1/name", openapi_name="Name Route", openapi_tags=["Name"])
async def get(r, query_params: GetRequestParams):
    """根据 ID 获取名称"""
    return r.query_params


@app.delete("/users/:name", openapi_tags=["Name"])
async def delete(r):
    """根据名称删除用户"""
    return r.path_params


if __name__ == "__main__":
    app.start()
```

### [如何在子路由下使用?](https://robyn.tech/documentation/api_reference/openapi#how-does-it-work-with-subrouters)

```python
from robyn import SubRouter
from robyn.robyn import QueryParams

subrouter = SubRouter(__name__, prefix="/sub")


@subrouter.get("/")
async def subrouter_welcome():
    """welcome subrouter"""
    return "hiiiiii subrouter"


class SubRouterGetRequestParams(QueryParams):
    _id: int
    value: str


@subrouter.get("/name")
async def subrouter_get(r, query_params: SubRouterGetRequestParams):
    """Get Name by ID"""
    return r.query_params


@subrouter.delete("/:name")
async def subrouter_delete(r):
    """Delete Name by ID"""
    return r.path_params


app.include_router(subrouter)
```

### [其他规范参数](https://robyn.tech/documentation/api_reference/openapi#other-specification-params)

Robyn 支持最新的 OpenAPI 规范（[https://swagger.io/specification/](https://swagger.io/specification/)）中提到的所有参数。下是请求和响应体的示例：

```python
from robyn.types import JSONResponse, Body

class Initial(Body):
    is_present: bool
    letter: Optional[str]


class FullName(Body):
    first: str
    second: str
    initial: Initial


class CreateItemBody(Body):
    name: FullName
    description: str
    price: float
    tax: float


class CreateResponse(JSONResponse):
    success: bool
    items_changed: int


@app.post("/")
def create_item(request: Request, body: CreateItemBody) -> CreateResponse:
    return CreateResponse(success=True, items_changed=2)
```

随着参考文档的成功部署，蝙蝠侠拥有了一个强大的新工具。Robyn 框架为他提供了创建高效打击犯罪应用所需的灵活性、可扩展性和性能，使他在保护哥谭市的持续战斗中获得了技术优势。

### [下一步](https://robyn.tech/documentation/api_reference/openapi#whats-next)

完成接口文档配置后，蝙蝠侠开始思考如何提升应用程序的并发处理能力。

Robyn 随即向他介绍了多进程执行的相关内容。

- [多进程执行](https://robyn.tech/documentation/api_reference/multiprocess_execution)
