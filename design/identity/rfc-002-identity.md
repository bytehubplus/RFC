# Identity

Identity 用于唯一标识某个对象。

## 用户 ID

用户ID 是一个 Self-Sovereign ID。 用户生成一对公私钥对，并根据公钥哈希计算得到一个 __ID__，该 ID 即是用户的 ID。 使用该 ID 时，必须用对应的私钥签名，签名验证通过才能使用。

## Vault ID

Vault 是用户的账户，用于存储用户数据，Vault ID 是一个符合 __W3C DID__ 标准的ID，但是，由于只有一个网络，一次 DID 的 __Schema__ 和 __Method__ 都一样。

![W3C DID](https://www.w3.org/TR/did-core/diagrams/parts-of-a-did.svg)

Vault的 __Controller__ 是该 Vault 的持有人，持有人管理 Vault 的访问权限。

### 个人 Vault

个人 Vault 的 Controller 只有一个，Vault ID 与 用户 ID 相同，即只有该用户自己可以控制他的 Vault。

### 企业 Vault

企业 Vault 可以有多个 Controller， 每个 Controller 都可以访问和控制该 Vault。
