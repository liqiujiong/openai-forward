# 启动
**`python main.py run` 参数配置项**

| 配置项       | 说明 | 默认值 |
|-----------| --- | :---: |
| --port    | 服务端口号 | 8000 |
| --workers | 工作进程数 | 1 |

**环境变量配置项**  
参考项目根目录下`.env`文件

| 环境变量            | 说明                             |           默认值            |
|-----------------|--------------------------------|:------------------------:|
| OPENAI_API_KEY  | 默认api key，支持多个默认api key, 以空格分割 |            无             |
| OPENAI_BASE_URL | 转发base url                     | `https://api.openai.com` |
| LOG_CHAT        | 是否记录聊天内容                       |          `true`          |
| ROUTE_PREFIX    | 路由前缀                           |            无             |
| IP_WHITELIST    | ip白名单, 空格分开                    |           无            |
| IP_BLACKLIST    | ip黑名单, 空格分开                    |           无            | 


# 聊天日志
保存路径在当前目录下的`Log/`路径中。  
聊天日志以 `chat_`开头, 默认每5轮对话写入一次文件    
记录格式为
```text
{'host': xxx, 'model': xxx, 'message': [{'user': xxx}, {'assistant': xxx}]}
{'assistant': xxx}

{'host': ...}
{'assistant': ...}

...
```
