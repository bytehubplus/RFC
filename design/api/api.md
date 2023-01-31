# REST API Definitions

__作者：赵振华__

__日期：2022年11月1日__

__版本: 1.0__

## API 规范

遵循 OpenAPI V3.1 规范。
[OpenAPI Specification](https://spec.openapis.org/oas/v3.1.0)

错误定义遵循 HTTP Status Code。[HTTP Status Code Registry](https://www.iana.org/assignments/http-status-codes/http-status-codes.xhtml)

URL 规则

__https://api.node_domain.com/version/__

其中__node_domain.com__为节点域名，__version__为 API 版本，目前为 1。
例如：__https://api.bytehubplus.com/1/vault/register__

__Content-Type: application/json__

## Vault Management

| URL              | Method | Description          |
| ---------------- | ------ | -------------------- |
| vault/register   | POST   | Vault Registration   |
| vault/unregister | POST   | Vault unregistration |

### 1. Register

Key information in JSON format, for more information refer to [Verification Methods Properties](https://www.w3.org/TR/did-core/#verification-method-properties)

__Request JSON Body__

```json
{
  "keys": [
    {
      "publicKeyJwk": {
        "crv": "Ed25519",
        "x": "VCpo2LMLhn6iWku8MKvSLg2ZAoC-nlOyPVQaO3FxVeQ",
        "kty": "OKP",
        "kid": "_Qq0UL2Fq651Q0Fjd6TvnYE-faHiOpRlPVQcY_-tA4A"
      }
    },
    {
      "id": "did:example:123456789abcdefghi#keys-1",
      "type": "Ed25519VerificationKey2020",
      "controller": "did:example:pqrstuvwxyz0987654321",
      "publicKeyMultibase": "zH3C2AVvLMv6gmMNam3uVAjZpfkcJCwDwnZn6z3wXmqPV"
    }
  ],
  "sig": "xxxx"
}
```

### 2. Unregister

__Request JSON Body__

```json
{
  "vault-id": "xxx",
  "sig": "xxxx"
}
```

Sig 为其中一个 Key 的签名。

## Controller Management

| URL                      | Method | Description                           |
| ------------------------ | ------ | ------------------------------------- |
| /vault/controller/append | POST   | Appends a new key into vault.         |
| /vault/controller/list   | GET    | Lists the keys.                       |
| /vault/controller/delete | POST   | Deletes the specified key from vault. |

### 1. Append

与注册类似，Request 中包含添加的 Key 以及签名信息。

Request JSON body

```json
{
  "keys": [
    {
      "publicKeyJwk": {
        "crv": "Ed25519",
        "x": "VCpo2LMLhn6iWku8MKvSLg2ZAoC-nlOyPVQaO3FxVeQ",
        "kty": "OKP",
        "kid": "_Qq0UL2Fq651Q0Fjd6TvnYE-faHiOpRlPVQcY_-tA4A"
      }
    },
    {
      "id": "did:example:123456789abcdefghi#keys-1",
      "type": "Ed25519VerificationKey2020",
      "controller": "did:example:pqrstuvwxyz0987654321",
      "publicKeyMultibase": "zH3C2AVvLMv6gmMNam3uVAjZpfkcJCwDwnZn6z3wXmqPV"
    }
  ],
  "sig": "xxxx"
}
```

## Entry Management

| URL                         | Method | Description                                     |
| --------------------------- | ------ | ----------------------------------------------- |
| /vault/entry/put            | POST   | Write or update the specified entry into vault. |
| /vault/entry/get/{entry-id} | GET    | Gets the specified entry from vault.            |
| /vault/entry/delete         | POST   | Deletes the specified entry from vault.         |

### 1. Put

__Request JSON Body__

```json
{
  "vault-id": "xxx",
  "data": "xxx",
  "sig": "xxx"
}
```

__Response__

```json
{
  "entry-id": "xxx"
}
```

### 2. Get

```json
{
  "vault-id": "xxx",
  "entry-id": "xxx",
  "sig": "xxx"
}
```

__Response__

```json
{
  "data": "xxx"
}
```

### 3. Delete

```json
{
  "vault-id": "xxx",
  "entry-id": "xxx",
  "sig": "xxx"
}
```
