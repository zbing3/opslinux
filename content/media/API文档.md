[TOC]

# 基本说明

params 为HTTP请求为GET时需要传的参数

data 为HTTP请求为POST/PUT时需要传的参数

headers 为HTTP请求设置的header值

url_params 为URL中的参数

## 域名

http://127.0.0.1/backstage

## 1. 用户登陆相关

### 1.1登陆接口
`post /login`

**请求参数**:

```python
data = {
    "uid": "",  # 账号
    "password": "", # 密码
}
```

**返回结果**:

成功

```python
{
    "status": 200,
    "msg": "success"
}
```


##  2.课程相关

`get /lesson`

**请求参数**:

```python
data = {

}
```

**返回结果**:

成功

```python
{
    "data": [
        {
            "id": 1,
            "name": "计算机网络基础",
            "description": "计算机网络基础",
            "cover_url": "",
            "status": 1
        }
    ],
    "status": 200,
    "msg": "success"
}
```