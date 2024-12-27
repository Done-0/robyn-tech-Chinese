### [使用中间件和事件](https://robyn.tech/documentation/api_reference/middlewares#working-with-middlewares-and-events)

随着蝙蝠侠的应用程序逐渐变得更加复杂，Robyn 向他介绍了中间件、启动和关闭事件，甚至 WebSocket 的使用。蝙蝠侠学会了如何创建中间件（在请求之前或之后执行的函数），如何管理应用程序的生命周期，并通过 WebSocket 实现与客户端的实时通信。

### [处理事件](https://robyn.tech/documentation/api_reference/middlewares#handling-events)

蝙蝠侠发现，通过添加启动和关闭事件，他能够有效地管理应用程序的生命周期。为此，他编写了以下代码来定义这些事件：

此外，蝙蝠侠也了解到，事件不仅可以通过函数来定义，还可以使用装饰器的方式来实现。

对于同步请求，蝙蝠侠使用如下代码：

```python
async def startup_handler():
  print("Starting up")

app.startup_handler(startup_handler)


```

对于异步请求，蝙蝠侠则使用如下代码：

```python
@app.get("/")
async def h(request):
    return "Hello, world"

```

### [处理中间件](https://robyn.tech/documentation/api_reference/middlewares#handling-middlewares)

蝙蝠侠学会了如何在中间件中同时使用同步和异步函数。他编写了以下代码，为每个请求添加了在请求之前和之后执行的中间件。中间件可以分为两类：请求前中间件和请求后中间件。该函数在每个请求处理之前执行，可以修改请求对象或执行其他操作。请求后中间件：该函数在每个请求处理之后执行，可以修改响应对象或执行其他操作。

每个请求前中间件都应接收并返回一个请求对象；每个请求后中间件都应接收并返回一个响应对象。

如果某个请求前中间件返回了响应对象，执行将被中止，响应对象直接返回给客户端，后续的请求后中间件和主处理函数将不再执行。

```python
@app.before_request("/")
async def hello_before_request(request: Request):
    request.headers["before"] = "sync_before_request"
    return request

@app.after_request("/")
def hello_after_request(response: Response):
    response.headers.set("after", "sync_after_request"")
    return response
```

### [下一步](https://robyn.tech/documentation/api_reference/middlewares#whats-next)

**Robyn**：太好了，中间件是 Robyn 框架的重要组件之一，您现在已经掌握了 Robyn 的一些高级概念。

**蝙蝠侠**：身份验证！我想了解身份验证，确保只有经过身份验证的的人才能访问我的应用程序。

**Robyn**：好的，接下来我将为您介绍身份验证！

- [鉴权](https://robyn.tech/documentation/api_reference/authentication)