# Vault Registering
10/08/2022
赵振华

## Role
| #    | Role    | Goal    |
|---------------- | --------------- | --------------- |
| Alice    | End User    | 希望自己的Vault能够安全地托管，以便能够存取数据    |
| Bob    | Serivce Provider    | 为用户提供安全、便捷的托管服务    |

## Preparing
- Alice必须已经知道Bob作为Service Provider的公钥，以能够安全地与Bob进行通信。
- Alice已经准备好自己的DID，包含公私钥
## Procedure
1. Alice将自己的DID发送给Bob
2. Bob收到Alice发送的数据之后，验证完整性。假定除了公钥之外，还必须包含Alice的电话号码和邮箱，暂时不要求其他信息。
3. Bob在自己的节点上注册Alice的Vault，并且按照Vault ID的生成规则，生成ID，连同节点地址和端口发送给Alice。

## Acceptance Criteria
Alice能够根据Bob返回的Vault ID，节点地址/端口访问自己的Vault。

