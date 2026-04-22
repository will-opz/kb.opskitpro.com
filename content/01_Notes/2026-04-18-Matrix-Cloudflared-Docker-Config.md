---
title: Matrix Cloudflared Tunnel & OrbStack Docker Config (2026-04-18)
date: 2026-04-18
tags: [matrix, cloudflared, cloudflare-tunnel, docker, orbstack, conduit, nginx]
---

# Matrix Cloudflared Tunnel & OrbStack Docker Config

**日期**: 2026-04-18  
**用途**: 记录本机 Matrix 服务器（Conduit）的 Cloudflare Tunnel 代理配置，以及相关 Docker / OrbStack 配置。

---

## 1. Cloudflare Tunnel 配置

### 配置文件
- `~/.cloudflared/config.yml`
- Tunnel credentials:
  - `~/.cloudflared/dcf9beae-2653-47be-9ae0-86cc6a9a9a9c.json`

### 当前 ingress
```yaml
tunnel: dcf9beae-2653-47be-9ae0-86cc6a9a9a9c
credentials-file: /Users/will/.cloudflared/dcf9beae-2653-47be-9ae0-86cc6a9a9a9c.json
protocol: http2

ingress:
  - hostname: feishu-webhook.deops.org
    service: http://localhost:8080
  - hostname: ai.deops.org
    service: http://localhost:18789  # openclaw
  - hostname: dify.deops.org
    service: http://localhost:80     # dify
  - hostname: matrix.deops.org
    service: http://localhost:6167   # matrix conduit
  - service: http_status:404
```

### 验证结果
- `cloudflared --config ~/.cloudflared/config.yml tunnel ingress validate` → `OK`
- `https://matrix.deops.org/_matrix/client/versions` → 200 OK，Matrix client versions 正常返回

---

## 2. Matrix 服务（Conduit）

### 容器状态
- 容器名：`conduit`
- 镜像：`matrixconduit/matrix-conduit:latest`
- 状态：`running`
- 端口映射：`6167 -> 6167`

### Docker inspect 摘要
- 数据卷挂载：`/Users/will/conduit-data:/var/lib/matrix-conduit`
- `/_matrix/client/versions` 可本地正常返回
- `/_matrix/federation/v1/version` 返回 `Federation is disabled`

### 备注
- 当前 Matrix 服务是**本机 Conduit**，并由 Cloudflare Tunnel 对外暴露。
- 这台机器上的 `80/443` 不是 Matrix，而是 Dify 的 `nginx` / `web` 入口。

---

## 3. Docker / OrbStack 配置

### Dify compose 项目
- Compose 文件：`/Users/will/work/dify/docker/docker-compose.yaml`
- Nginx 配置模板：`/Users/will/work/dify/docker/nginx/conf.d/default.conf.template`

### Nginx 容器
- 容器名：`docker-nginx-1`
- Compose project：`docker`
- Working dir：`/Users/will/work/dify/docker`
- 配置文件由 compose 自动生成，不建议直接修改 `docker-compose.yaml`

### 关键路由
Dify 的 Nginx 模板里当前只有：
- `/console/api`
- `/api`
- `/v1`
- `/files`
- `/explore`
- `/e/`
- `/mcp`
- `/triggers`
- `/`

**没有** `/_matrix` 路由，所以 Matrix 必须走 Cloudflare Tunnel 直接打到 `localhost:6167`。

---

## 4. 结论

当前链路是：

`matrix.deops.org` → Cloudflare Tunnel → `http://localhost:6167` → `conduit`

这条链路已验证可用。

---

## 5. 运维备忘

- Matrix 没有 TURN 配置，Conduit 日志里会看到 `No TURN config set`
- Federation 当前是关闭的
- 如果未来要走统一 Nginx 入口，需要额外加 `/_matrix` 反代规则；目前不需要，因为 Tunnel 已经直连 Conduit
