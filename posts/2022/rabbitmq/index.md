# RabbitMQ 笔记


## 安装

安装容器

```sh
docker run -d --name es -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -e ES_JAVA_OPTS="-Xms512m -Xmx512m" elasticsearch:8.3.3
```

进入容器

```sh
docker exec -it es /bin/bash
```

## 应用场景

## 消息分发策略

消息流转

![img](./images/webp-16609259621499.webp)

- 简单模式
- 路由模式
- 通配符模式
- 发布订阅模式
- 工作队模式

