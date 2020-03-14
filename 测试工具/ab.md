# 压测工具ab

## 测试POST json格式接口

> data.json中是要传输的body内容，格式为json

```sh
ab -n 20 -c 5 -T 'application/json' -p 'data.json' http://localhost:8080/order/balance/charge/back
```
