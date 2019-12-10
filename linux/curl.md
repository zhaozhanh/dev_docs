<!--
 * @Author: hanzhaozhan
 * @LastEditors: hanzhaozhan
 -->

# curl命令

## POST JSON数据

```sh
curl -H "Content-Type: application/json" -X POST  --data '{"userID":10001}' http://localhost:8085/user/info
```
