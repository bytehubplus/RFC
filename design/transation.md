# 什么是 Hub？

一个 Hub 由若干个 Service Provider（SP）组成，不同 SP 提供的服务应当具有关联性，以方便协助、共享数据。例如 SP1 是电商平台，SP2 是支付平台，SP3 是物流公司等，他们可共同组成一个电商的 Hub。每个 SP 为每个托管 Vault 的用户颁发证书，应当向外提供证书状态查询服务。但是，同一 Hub 内的其他 SP 可以向其他 SP 请求获取用户证书，但是该 SP 有权拒绝提供。

例如：

如果 SP1 收到了 User3 的某个请求，SP1 可以向 SP2 提供查询该证书是否有效。注意，此时 User3 的证书是由 User3 向 SP1 提供的，该证书由 SP2 颁发。

SP1 托管了 User1、User2 的 Vault，SP1 为用户颁发了相应的证书。
SP2 托管了 User3、User4 的 Vault，SP2 为用户颁发了相应的证书。

SP1 向 SP2 索要 User3 的证书，SP2 可以拒绝、也可以同意。完全取决于 SP2。

# Transation Request

此时，我们可以理解一个用户的 TX 应当包含。

```json
{
  "hub": "hub_id",
  "plugin": "plugin_id",
  "param": {
    "param1": "value1",
    "param2": "value2"
  },
  "signature": "xxx"
}
```

其中，`hub_id`为 Hub 的唯一 ID；`plugin_id`为 SP 处理该请求业务逻辑的 Plugin；`parma`为相应的参数；`signature`为用户的签名。

# Transation Response

SP 收到用户的请求后，执行`plugin`完成业务处理，生成`transation_id`，并将结果签名后连同结果返回给用户。

```json
{
  "hub": "hub_id",
  "transation_id": "xxx",
  "result": {
    "result1": "value1",
    "result2": "value2"
  },
  "signature": "xxx"
}
```

**思考**

会出现需要多个 SP 签名的交易吗？应该怎么处理？
