## 打包步骤

```yaml
docker build -t jwstar/up .
```

## 主服务配置

```yaml
version: "3.7"

services:
  redis:
    image: redis:alpine
    container_name: redis
    restart: always
    command: redis-server --requirepass test --appendonly yes
    environment:
      TZ: Asia/Shanghai
    ports:
      - "16379:6379"
    volumes:
      - ./data:/data
  # bot业务
  up:
    image: jwstar/up
    depends_on:
      - redis
    network_mode: host
    container_name: up
    restart: always
    volumes:
      - ./config:/config

    environment:
      TZ: Asia/Shanghai
      REDIS_HOST: 72.44.65.86
      REDIS_PORT: 16379
      REDIS_PASS: test

      API_ID: 11216861
      API_HASH: 4d89994f340ecc92f2334b9453805840
      BOT_TOKEN: 5586139788:AAEJOxPGv65no-lHzeWZlk0SxQn3wAJ1h3g
      #掉线节点发送的群id或者自己id
      ADMIN_ID: -1001607514516
      #节点名称
      NODE_NAME: 'racknerd-洛杉矶'
      #是否是主服务
      IS_SERVER: 'True'
      ONLINE_RATE: 0.9
```

启动服务

```yaml
docker compose up -d
```

查看日志

```yaml
docker logs -f up
```

删除容器

```yaml
docker rm -f up
```

## 监控节点配置

```yaml
version: "3.7"

services:
  # bot业务
  up:
    image: jwstar/up
    network_mode: host
    container_name: up
    restart: always
    environment:
      TZ: Asia/Shanghai
      #配置主服务器ip
      REDIS_HOST: 72.44.65.86
      REDIS_PORT: 16379
      REDIS_PASS: test

      API_ID: 11216861
      API_HASH: 4d89994f340ecc92f2334b9453805840
      BOT_TOKEN: 5586139788:AAEJOxPGv65no-lHzeWZlk0SxQn3wAJ1h3g
      ADMIN_ID: -1001607514516
      #节点名称
      NODE_NAME: '百度BGP-广州'
      #监控节点 填 False
      IS_SERVER: 'False'
      ONLINE_RATE: 0.9
```

启动服务

```yaml
docker compose up -d
```

查看日志

```yaml
docker logs -f up
```

删除容器

```yaml
docker rm -f up
```


