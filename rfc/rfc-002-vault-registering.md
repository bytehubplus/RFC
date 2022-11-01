# Vault Registering


Author: 赵振华

Date: 10/08/2022

## Role
| #    | Role    | Goal    |
|---------------- | --------------- | --------------- |
| Alice    | End User    | 希望自己的Vault能够安全地托管，以便能够存取数据    |
| Bob    | Serivce Provider    | 为用户提供安全、便捷的托管服务    |

## Prerequest
- Alice必须已经知道Bob作为Service Provider的公钥，以能够安全地与Bob进行通信。
- Alice已经准备好自己的DID Document，其中包含公私钥

## Procedure
1. Alice将自己的DID发送给Bob
2. Bob收到Alice发送的数据之后，验证完整性。假定除了公钥之外，还必须包含Alice的电话号码和邮箱，暂时不要求其他信息。
3. Bob在自己的节点上注册Alice的Vault，并且按照Vault ID的生成规则，生成ID，连同节点地址和端口发送给Alice。



## Acceptance Criteria
Alice能够根据Bob返回的Vault ID，节点地址/端口访问自己的Vault。


# Apendix A - Generate DID

```shell
openssl genrsa -out private.pem 2048
openssl rsa -in private.pem -outform PEM -pubout -out public.pem

```

```javascript
npm install @decentralized-identity/did-auth-jose
npm pem-jwk

```

Save below code into **make-jws.js** file
```javascript
var fs = require('fs');
var path = require('path');
var didAuth = require('@decentralized-identity/did-auth-jose');

// load JWKs from files
const jwkPriv = JSON.parse(fs.readFileSync(path.resolve(__dirname, './private.jwk'), 'ascii'));
const jwkPub = JSON.parse(fs.readFileSync(path.resolve(__dirname, './public.jwk'), 'ascii'));

// load JWK into an EcPrivateKey object
const privateKey = didAuth.EcPrivateKey.wrapJwk(jwkPriv.kid, jwkPriv);

async function makeJws() {

    // construct the JWS payload
    const body = {
        "@context": "https://w3id.org/did/v1",
        publicKey: [
            {
                id: jwkPub.kid,
                type: "Secp256k1VerificationKey2018",
                publicKeyJwk: jwkPub
            }
        ],
        service: [
            {
                id: "IdentityHub",
                type: "IdentityHub",
                serviceEndpoint: {
                    "@context": "schema.identity.foundation/hub",
                    "@type": "UserServiceEndpoint",
                    instance: [
                        "did:test:hub.id",
                    ]
                }
            }
        ],
    };

    // Construct the JWS header
    const header = {
        alg: jwkPub.defaultSignAlgorithm,
        kid: jwkPub.kid,
        operation:'create',
        proofOfWork:'{}'
    };

    // Sign the JWS
    const cryptoFactory = new didAuth.CryptoFactory([new didAuth.Secp256k1CryptoSuite()]);
    const jwsToken = new didAuth.JwsToken(body, cryptoFactory);
    const signedBody = await jwsToken.signAsFlattenedJson(privateKey, {header});

    // Print out the resulting JWS to the console in JSON format
    console.log(JSON.stringify(signedBody));

}

makeJws();

```

The file can sign a registration request as jws.

```shell
node make-jws.js
```


