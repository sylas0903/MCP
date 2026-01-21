### JSON-RPC
함수 호출을 문자열 메세지(JSON)형식으로 변환해서, 수신측 프로그램에게 호출하도록 요청하는 통신 규칙으로,
JSON-RPC는 서버에 메세지를 전송하고 응답을 기다리지않는 notification 방식을 지원하고, 다수의 호출에 대한 응답 순서를 규정하지않는다.

JSON-RPC에서 주고받는 모든 메세지는 JSON형식으로 변환된 하나의 JSON 객체 {} 여야 한다. 
다음 코드의 jsonrpc, method, params, id가 각각의 함수 호출이며, JSON-RPC Request에는 반드시 jsonrpc, method, id가 포함되어야 한다.
```
{
  "jsonrpc": "2.0",
  "method": "add",
  "params": {
    "a": 3,
    "b": 5
  },
  "id": 1
}
```                
### notification vs  JSON-RPC Request
```
notification
{
  "jsonrpc": "2.0",
  "method": "log",
  "params": {
    "msg": "서버 시작"
  }
}

JSON-RPC Request
{
  "jsonrpc": "2.0",
  "result": 8,
  "id": 1
}
```
notification과 JSON-RPC Request의 차이는 id의 유무이다. id에 대해 서버는 반드시 응답 해야하며 notification의 경우 id가 없기 때문에 서버는 아무런 응답도 보내지 않는다.

### id의 필요성

JSON-RPC Request는 다수의 호출을 동시에 전송할 수 있으며, 이에 대한 응답의 순서를 보장하지 않는다.
따라서 서버는 각 응답이 어떤 요청에 대한 결과인지 식별할 수 있는 정보가 필요하다.

아래의 예시처럼 JSON-RPC Request에 id가 없다면, JSON-RPC Response를 수신했을 때 해당 응답이 add 요청의 결과인지 sub 요청의 결과인지 구분할 수 없다.
이 때문에 JSON-RPC에서는 id를 사용하여 서버가 각 요청에 대한 응답을 식별하고, 클라이언트가 이를 올바른 요청과 매칭할 수 있도록 한다.
 ```
JSON-RPC Request
{ "jsonrpc": "2.0", "method": "add", "params": [1,2] }
{ "jsonrpc": "2.0", "method": "sub", "params": [5,3] }

JSON-RPC Response
{ "jsonrpc": "2.0", "result": 2 }
{ "jsonrpc": "2.0", "result": 3 }
```






