[**中文**](./README.md) | [**English**](./README_EN.md)

<h1 align="center">
    <br>
    OpenAI Forward
    <br>
</h1>
<p align="center">
    <b> OpenAI API 接口转发服务 <br/>
    The fastest way to deploy openai api forward proxy! </b>
</p>

<p align="center">
    <img alt="pypi" src="https://img.shields.io/badge/Pypi-0.0.8-green.svg">
    <img alt="License" src="https://img.shields.io/github/license/beidongjiedeguang/openai-forward.svg?color=blue&style=flat-square">
    <img alt="Release (latest by date)" src="https://img.shields.io/github/v/release/beidongjiedeguang/openai-forward">
    <img alt="GitHub repo size" src="https://img.shields.io/github/repo-size/beidongjiedeguang/openai-forward">
    <img alt="GitHub top language" src="https://img.shields.io/github/languages/top/beidongjiedeguang/openai-forward">
    <img alt="name" src="https://img.shields.io/badge/author-@kunyuan-orange.svg?style=flat-square&logo=appveyor">
</p>

[![Build Status](https://www.travis-ci.org/beidongjiedeguang/openai-forward.svg?branch=main)](https://www.travis-ci.org/beidongjiedeguang/openai-forward)
[![codecov](https://codecov.io/gh/beidongjiedeguang/openai-forward/branch/main/graph/badge.svg)](https://codecov.io/gh/beidongjiedeguang/openai-forward)


解决国内无法直接访问OpenAI的问题，将该服务部署在海外服务器上，通过该服务转发OpenAI的请求。即搭建反向代理服务  
测试访问：https://caloi.top/v1/chat/completions 将完全等价于 https://api.openai.com/v1/chat/completions


### 快速部署
```bash
docker run --name="openai-forward" -d -p 9999:8000 beidongjiedeguang/openai-forward:latest 
```
通过`http://{ip}:9999`访问服务。 部署详情见下文。

## 特点
- [x] 支持转发OpenAI所有接口
- [x] 请求ip验证
- [x] 支持流式转发
- [x] 支持默认api key
- [x] pip安装
- [x] docker部署
- [x] 自定义转发端口
- [ ] 自定义转发路由
- [ ] SSL证书支持
- [ ] 聊天内容安全：聊天内容流式过滤

## 应用
> 这里以个人搭建好的代理地址 https://caloi.top 为例
 
### [chatgpt-web](https://github.com/Chanzhaoyu/chatgpt-web)
修改 [Docker Compose](https://github.com/Chanzhaoyu/chatgpt-web#docker-compose) 中的`OPENAI_API_BASE_URL`为我们搭建的代理服务地址:
```bash
OPENAI_API_BASE_URL: https://caloi.top 
```

### [ChatGPT-Next-Web](https://github.com/Yidadaa/ChatGPT-Next-Web)
替换docker启动命令中的 `BASE_URL`为我们搭建的代理服务地址
```bash
docker run -d -p 3000:3000 -e OPENAI_API_KEY="sk-xxx" -e CODE="<your password>" -e BASE_URL="caloi.top" yidadaa/chatgpt-next-web
```

### Image Generation (DALL-E):
```bash
curl --location 'https://caloi.top/v1/images/generations' \
--header 'Authorization: Bearer sk-***[OUR_API_KEY]***' \
--header 'Content-Type: application/json' \
--data '{
    "prompt": "A photo of a cat",
    "n": 1,
    "size": "512x512",
}'
```

### 在模块中使用

**Used in JS/TS**
```typescript
import { Configuration } from "openai";

const configuration = new Configuration({
    basePath: "https://caloi.top",
    apiKey: "sk-******",
    
});
```
**Used in Python**  
```python
import openai
openai.api_base = "https://caloi.top"
openai.api_key = "sk-******"
```

## 服务部署
提供两种服务部署方式,选择一种即可

### 方式一:  pip
**安装**
```bash
pip install openai-forward
```
**运行转发服务**  
可通过`--port`指定端口号，默认为`8000`，可通过`--workers`指定工作进程数，默认为`1`
```bash
openai_forward run --port=9999 --workers=1
```
服务就搭建完成了，使用方式只需将`https://api.openai.com` 替换为服务所在端口`http://{ip}:{port}` 即可。  

当然也可以将 OPENAI_API_KEY 作为环境变量传入作为默认api key， 这样客户端在请求相关路由时可以无需在Header中传入Authorization。
带默认api key的启动方式：
```bash
OPENAI_API_KEY="sk-xxx" openai_forward run --port=9999 --workers=1
```
注: 如果既存在默认api key又在请求头中传入了api key，则以请求头中的api key会覆盖默认api key.


### 方式二: Docker(推荐) 
```bash
docker run --name="openai-forward" -d -p 9999:8000 beidongjiedeguang/openai-forward:latest 
```
将映射宿主机的9999端口，通过`http://{ip}:9999`访问服务。  
注：同样可以在启动命令中通过-e传入环境变量OPENAI_API_KEY=sk-xxx作为默认api key

## 服务调用
替换openai的api地址为该服务的地址即可，如：
```bash
https://api.openai.com/v1/chat/completions
```
替换为
```bash
http://{ip}:{port}/v1/chat/completions
```

## Https 支持
TODO