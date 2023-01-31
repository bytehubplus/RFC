# Entry Management

该文档说明在某个对于Entry的操作以及request的参数格式。

URL     | Method    | Description
-------- | ----- | ------
localhost:8080/put  | POST | Put data into entryID if entryID exist. if entryID is nil, create a entryID and return it.
localhost:8080/get  | POST | Get data from entryID and return data
localhost:8080/entry_delete  | POST | Delete data according to entryID

## 1. Put

用户在自己注册的vault中向某个Entry添加数据。
*Request JSON Body*  
{  
  "Data": "Hello World",  
  "Entry_ID": "entry_42",
  "Token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJWYXVsdElEIjoiNzQyYzUzNTg3YzI1M2M3OTY1MzNlZTEwNGViYWI1M2QyNDQ0ODBmYSIsImV4cCI6MTY3MjY2OTAwMSwiaXNzIjoibm9kZSJ9.LplG8Nr8nC5UyjjTxnqolSoolzvg_KegkarhvLz3YhM"
}  

## 2. Get

请求返回Entry中的数据。
*Request JSON Body*  
{  
  "Entry_ID": "entry_42",
  "Token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJWYXVsdElEIjoiNzQyYzUzNTg3YzI1M2M3OTY1MzNlZTEwNGViYWI1M2QyNDQ0ODBmYSIsImV4cCI6MTY3MjQ5Mjc4MiwiaXNzIjoibm9kZSJ9.6VJc_LQfwA30cneoQcRbubg0ClMM43XM0VbmHjQxZgg"  
}  

## 3. entry_delete

用户删除某个Entry。
*Request JSON Body*  
{  
  "Entry_ID": "entry_42",
  "Token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJWYXVsdElEIjoiNzQyYzUzNTg3YzI1M2M3OTY1MzNlZTEwNGViYWI1M2QyNDQ0ODBmYSIsImV4cCI6MTY3MjQ5Mjc4MiwiaXNzIjoibm9kZSJ9.6VJc_LQfwA30cneoQcRbubg0ClMM43XM0VbmHjQxZgg"  
}
