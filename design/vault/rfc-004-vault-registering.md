# Vault Registering

Author: 赵振华

Date: 10/08/2022

用户注册过程是用户准入过程。在此过程中，用户和 Identity Provider 分别扮演了[W3 Verifiable Credential](https://www.w3.org/TR/vc-data-model/)的`Holder`和`Issuer`的角色。注册成功，用户获得 Identity Provider 的 VC。该 VC 可用于证明自己的身份。同时，Identity Provider 担任了 KYC 的角色。

![Figure1: VC Model](./did-ecosystem.svg)

用户注册前需要生成一对公私钥，并将自己的身份信息和公钥安全地发送给托管 Vault 的 Identity Provider。Identity Provider 为用户生成 DID 和 DID Document。Document 的`authentication`为该用户的公钥，用于授权签名验证。

Identity Provider 将托管地址写入 Document 的`ServiceEndpoint`，并对进行签名，然后返回给用户。

用户注册需要提交满足注册要求的信息。该要求应该是公开的，以便所有人都了解。用户提交的 Request 的`@context`字段表明需要满足的要求。

```json
{
  "@context": ["example.com/credential/v1.json"],
  "name": "Alice",
  "email": "alice@example.com",
  "phone": "+86 138 1234 5678",
  "authentication": [
    {
      "id": "did:example:123#key-1",
      "type": "Ed25519VerificationKey2020", // external (property value)
      "controller": "did:example:123", //to be filled or replaced by service provider with DID ID
      "publicKeyMultibase": "zAKJP3f7BD6W4iWEQ9jwndVTCBq8ua2Utt8EEjJ6Vxsf"
    }
  ]
}
```

Public Key in JWK format。

完成注册的 Response 是一个 VC，Identity Provider 将其保存在 Vault `Index`数据库，用户也可以保存在用户的 Vault 中，用户可以用于生成`Verifiable Presentation`(VP)证明自己的身份。

VC 示例：

```json
{

  "@context": [
    "https://www.w3.org/2018/credentials/v1",
    "https://www.w3.org/2018/credentials/examples/v1"
  ],

  "id": "http://example.edu/credentials/1872",

  "type": ["VerifiableCredential", "AlumniCredential"],

  "issuer": "https://example.edu/issuers/565049",

  "issuanceDate": "2010-01-01T19:23:24Z",

  "credentialSubject": {

    "id": "did:example:ebfeb1f712ebc6f1c276e12ec21",

    "alumniOf": {
      "id": "did:example:c276e12ec21ebfeb1f712ebc6f1",
      "name": [{
        "value": "Example University",
        "lang": "en"
      }, {
        "value": "Exemple d'Université",
        "lang": "fr"
      }]
    }
  },

  "proof": {

    "type": "RsaSignature2018",

    "created": "2017-06-18T21:19:10Z",

    "proofPurpose": "assertionMethod",

    "verificationMethod": "https://example.edu/issuers/565049#key-1",

    "jws": "eyJhbGciOiJSUzI1NiIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19..TCYt5X
      sITJX1CxPCT8yAV-TVkIEq_PbChOMqsLfRoPsnsgw5WEuts01mq-pQy7UJiN5mgRxD-WUc
      X16dUEMGlv50aqzpqh4Qktb3rk-BuQy72IFLOqV0G_zS245-kronKb78cPN25DGlcTwLtj
      PAYuNzVBAh4vGHSrQyHUdBBPM"
  }
}
```

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

## Apendix A - Generate DID

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
