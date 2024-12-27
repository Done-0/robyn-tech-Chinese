### [WebSocket](https://robyn.tech/documentation/api_reference/websockets#websockets)

为了实现实时通信，蝙蝠侠学习了如何使用 WebSocket。他创建了一个 WebSocket 类，并将其集成到他的 Robyn 应用中：

```python
from robyn import Robyn, jsonify, WebSocket

app = Robyn(__file__)
websocket = WebSocket(app, "/web_socket")

@websocket.on("message")
def connect():
    return "Hello world, from ws"

@websocket.on("close")
def close():
    return "Goodbye world, from ws"

@websocket.on("connect")
def message():
    return "Connected to ws"


```

为了向客户端发送消息，蝙蝠侠使用了 `sync_send_to` 方法。

```python

  @websocket.on("message")
  def message(ws, msg, global_dependencies) -> str:
      websocket_id = ws.id
      ws.sync_send_to(websocket_id, "This is a message to self")
      return ""

```

为了异步向客户端发送消息，蝙蝠侠使用了 `async_send_to` 方法。

```python

  @websocket.on("message")
  async def message(ws, msg, global_dependencies) -> str:
      websocket_id = ws.id
      await ws.async_send_to(websocket_id, "This is a message to self")
      return ""

```

为了向所有客户端发送广播，蝙蝠侠使用了 `sync_broadcast` 方法。

```python

  @websocket.on("message")
  def message(ws, msg, global_dependencies) -> str:
      websocket_id = ws.id
      ws.sync_broadcast("This is a message to self")
      return ""

```

为了异步发送广播，蝙蝠侠使用了 `async_broadcast` 方法。

```python

  @websocket.on("message")
  async def message(ws, msg, global_dependencies) -> str:
      websocket_id = ws.id
      await ws.async_broadcast("This is a message to self")
      return ""

```

此外，Robyn 还向蝙蝠侠展示了如何处理 WebSocket 查询参数。

```python

  @websocket.on("message")
  async def message(ws, msg, global_dependencies) -> str:
      websocket_id = ws.id
      if (ws.query_params.get("name") == "gordon" and ws.query_params.get("desg") == "commissioner"):
        ws.sync_broadcast("Gordon authorized to login!")
      return ""

```

蝙蝠侠还学习了如何通过 `close()` 方法从服务端关闭 WebSocket 连接。`close()` 方法将执行以下操作：

- 1. 向客户端发送关闭消息
- 2. 从 WebSocket 注册表中移除客户端
- 3. 关闭 WebSocket 连接

这种方法适用于需要根据服务端某些条件或事件来结束 WebSocket 连接的场景。

```python
@websocket.on("message")
def message(ws, msg):
    if msg == "disconnect":
        ws.close()
        return "Closing connection"
    return "Message received"
```

### [下一步](https://robyn.tech/documentation/api_reference/websockets#whats-next)

随着代码库的扩展，蝙蝠侠希望正义联盟的成员能够参与管理应用程序。

Robyn 向他介绍了应用扩展的最佳实践，并展示了如何通过视图和子路由器来提升代码的可读性和可维护性。

- [视图与子路由器](https://robyn.tech/documentation/api_reference/views)
