# Nginx 配置 WebSocket


<!--more-->

Nginx 反向代理 WebSocket 请求，以 `ws` 明文协议为例：

通常我们使用 `ws://xxx.com` 连接 WebSocket 服务，因为 WebSocket 服务也是通过 http 协议来完成部分握手的，所以只需要配置 http 代理的 `Upgrade` 和 `Connection` 响应头。

首先在 `nginx.conf` 中配置自定义变量：
```nginx
http {
  # ...

  # 如果请求头有 Upgrade，$connection_upgrade 设置为 upgrade，否则为 close
  map $http_upgrade $connection_upgrade {
    default upgrade;
    '' close;
  }

  # ...
}
```

然后在 `/conf.d/xxx.conf` 下配置具体代理
```nginx
server {
  listen 80;
  server_name xxx.xxx.com;

  location / {
    proxy_pass http://127.0.0.1:xxxx;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection $connection_upgrade;
  }
}
```
