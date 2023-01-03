# Controller Management
该文档说明用户对其注册的vault进行操作的request请求格式，分为Append、List、Delete。  
三个请求中均包含jwt token，用户在注册成功后会获得一个jwt token，使用该token用户可以直接对自己的vault进行操作。需要注意的是，jwt token的签发存在过期时间，默认每一个token的有效期为24h。  

  
URL     | Method    | Description
-------- | ----- | ------
localhost:8080/append  | POST | Append a new Public key 
localhost:8080/list  | POST | List all of the Public key 
localhost:8080/pubkey_delete  | POST | Delete a Public key

***
**1. Append**   
用户向自己注册的vault新增一个公钥。
*Request JSON Body*  
{  
  "Keys":  
  "042ff4b2267135e7f4da8269cb94b82728ce5543346a82e7b878d3ceae52b46ed50561fd58d21abfb64147631354ae635755f65bc6a1bd48cbc46e5a23b8e384b7",  
  "Token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJWYXVsdElEIjoiNzQyYzUzNTg3YzI1M2M3OTY1MzNlZTEwNGViYWI1M2QyNDQ0ODBmYSIsImV4cCI6MTY3MjQ5Mjc4MiwiaXNzIjoibm9kZSJ9.6VJc_LQfwA30cneoQcRbubg0ClMM43XM0VbmHjQxZgg"  
}  

**2. List**  
请求返回已经添加的公钥列表 
*Request JSON Body*  
{  
  "Token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJWYXVsdElEIjoiNzQyYzUzNTg3YzI1M2M3OTY1MzNlZTEwNGViYWI1M2QyNDQ0ODBmYSIsImV4cCI6MTY3MjQ5Mjc4MiwiaXNzIjoibm9kZSJ9.6VJc_LQfwA30cneoQcRbubg0ClMM43XM0VbmHjQxZgg"  
}  

**3. Delete**  
用户删除某个已添加的公钥。
*Request JSON Body*  
{  
  "Keys":  
  "042ff4b2267135e7f4da8269cb94b82728ce5543346a82e7b878d3ceae52b46ed50561fd58d21abfb64147631354ae635755f65bc6a1bd48cbc46e5a23b8e384b7",  
  "Token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJWYXVsdElEIjoiNzQyYzUzNTg3YzI1M2M3OTY1MzNlZTEwNGViYWI1M2QyNDQ0ODBmYSIsImV4cCI6MTY3MjQ5Mjc4MiwiaXNzIjoibm9kZSJ9.6VJc_LQfwA30cneoQcRbubg0ClMM43XM0VbmHjQxZgg"  
} 