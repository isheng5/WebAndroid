# 移动端交互文档
本文档仅用于前后端交互使用

# 用户认证
> 用户认证，使用token认证的方式认证，不采用cookies的方式认证

## 用户登陆

### 前端请求 Request

> 本数据包含在Request中的Body部分

```json
{
  "username": "admin",
  "password": "admin"
}
```

### 后端响应 Response

> 后台发送数据格式应该为json

```json
  "code":20,
  "message": "",
  "data": {
    "access_token": "sdklvjhaa68s4v5a6s4ef64F1c6ef43F4",
    "exp": 3600,
    "token_type": "Basic",
    "username": "user"
  }
```

```code``` 是状态代码，前后端做如下协定（暂行）：
- ```code = 20``` 用户登陆成功
- ```code = 21``` 用户注册成功
- ```code = 22``` 操作成功
- ```code = 40``` 用户名或者密码错误
- ```code = 41``` 用户权限错误
- ```code = 42``` 请求次数过多
- ```code = 43``` 非法注册

```message``` 是服务器对状态的说明
成功时做如下返回：
- ```message = 'success'``` 请求成功
失败时，应当将错误信息（例如权限错误）一起发送，例子如下：

```json
  "code":41,
  "message": "用户权限不足",
  "data": {}
```

!> 无论请求成功或者失败都必须包含```code```、```message```、```data```,```data```中只能包含前端请求所需要的资源，错误信息请放入到```message```中

## 用户注册

### 前端请求 Request

> 本数据包含在Request中的Body部分

```json
{
  "username": "admin",
  "password": "admin"
}
```

### 后端响应 Response

> 后台发送数据格式应该为json

注册成功做出如下响应

```json
  "code":21,
  "message": "",
  "data": {
    "access_token": "sdklvjhaa68s4v5a6s4ef64F1c6ef43F4",
    "exp": 3600,
    "token_type": "Basic",
    "username": "user"
  }
```

失败时，下面列举一个例子：
```json
  "code":43,
  "message": "非法注册",
  "data": {}
```

## 普通请求

> 所有用户在登陆成功后对服务器请求资源的请求为普通请求

### 请求方式

- ```GET``` 用于获取资源
- ```POST``` 用于更改、添加、删除服务器上的资源

### 请求的认证

> 所有的请求都必须经过服务器的严格认证与防范，前端只做大致判断，无法完全阻止接口泄露或者其他风险

前端会发送一个带有Token令牌认证的请求字段下面是一个请求头
```json
"Host": "easy-mock.com",
"Connection": "keep-alive"
"Content-Length": 39,
"Accept": "application/json, text/plain, */*"
"Origin": "http://localhost:8080"
"Authorization": "Basic yJFPrdkuo0FzaBhX4T4EpVmXQuxiMpUBlk406pUbUe3ePN2zW7UPQaxedMoDlrIVqNw18g1guep6sNNDuDmcJU4zTQ042HXs1sOTZgwaBkswqsFxcxCkoykN"
"User-Agent": "Mozilla/5.0 (Linux; Android 6.0; Nexus 5 Build/MRA58N) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/61.0.3163.100 Mobile Safari/537.36
Content-Type: application/json;charset=UTF-8"
"Referer": "http://localhost:8080/"
"Accept-Encoding": "gzip, deflate"
"Accept-Language": "zh-CN,zh;q=0.8"
```

其中的 ```Authorization``` 为token令牌，后台应该检查该令牌的权限，有效期，判断后才能返回所请求的数据

!> 后台除了鉴定令牌还需要做更多的事情来确保服务器安全，比如限制令牌的过期时间，以及令牌请求地点限制等等，或者考虑使用更安全的SSL。

## 请求标识码
> 包含所有的 ```code``` 正常或者错误标识

### 请求成功标识码

- ```code = 20``` 用户登陆成功
- ```code = 21``` 用户注册成功
- ```code = 22``` 操作成功

### 请求失败标识码

- ```code = 40``` 用户名或者密码错误
- ```code = 41``` 用户权限错误
- ```code = 42``` 请求次数过多
- ```code = 43``` 非法注册
