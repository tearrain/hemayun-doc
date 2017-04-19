# 通讯响应码说明

200：通讯成功；400：客户端错误；500:服务端错误

error\_code为本次通讯的错误码，error\_message为对应的中文描述。当result\_code不等于200的时候才会出现error\_code和error\_message。

result\_code:400，客户端错误。客户端请求错误。INVALID\_PARAMS/参数错误；INVALID\_TERMINAL/终端错误；ILLEGAL\_SIGN/签名错误。

result\_code:500，服务端错误。收钱吧服务端异常。可提示“服务端异常，请联系合码云客服”。

# 通讯错误码列表

error\_code为本次通讯的错误码

error\_message为对应的中文描述

当result\_code不等于200的时候才会出现

| result\_code | error\_code | error\_message |
| --- | --- | --- |
| **400** | INVALID\_PARAMS | 参数错误 |
| **400** | INVALID\_TERMINAL | 终端错误 |
| **400** | ILLEGAL\_SIGN | 签名错误 |
| **500** | UNKNOWN\_SYSTEM\_ERROR | 系统错误 |



