### [CORS](https://robyn.tech/documentation/api_reference/cors#cors)

每次蝙蝠侠尝试访问 API 时，都会遇到 CORS 错误，这让他苦不堪言。

### [扩展应用程序](https://robyn.tech/documentation/api_reference/cors#scaling-the-application)

为了启用跨源资源共享（CORS），您可以在应用程序中添加以下代码：

```python
  from robyn import Robyn, ALLOW_CORS

  app = Robyn(__file__)
  ALLOW_CORS(app, origins = ["http://localhost:<PORT>/"])
```

### [下一步](https://robyn.tech/documentation/api_reference/cors#whats-next)

修复了 CORS 问题后，蝙蝠侠感到非常满意，现在他希望了解如何在服务器中集成小型前端页面。

于是，Robyn 向他介绍了模板系统，以及如何使用模板来渲染 HTML 页面。

- [模板](https://robyn.tech/documentation/api_reference/templating)
