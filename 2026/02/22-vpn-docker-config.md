# 在 NAS 上配置 OpenConnect VPN

> 使用 Docker 在群晖 NAS 上运行 OpenConnect VPN 客户端

## 问题背景

需要在 NAS 上配置 VPN，让 Docker 容器可以科学上网。

## 解决方案

### 1. 镜像选择

原计划用 `wazum/openconnect-proxy`，但该镜像是 ARM 架构，NAS 是 x86，不兼容。

最终使用：`zcalusic/openconnect:latest` (x86)

### 2. 镜像迁移

从 Mac 导出镜像到 NAS：

```bash
# Mac 上
docker pull --platform linux/amd64 zcalusic/openconnect
docker save zcalusic/openconnect:latest | gzip > ~/openconnect-x86.tar.gz
scp ~/openconnect-x86.tar.gz user@NAS_IP:/volume1/docker/openconnect/

# NAS 上
gunzip -c openconnect-x86.tar.gz | sudo docker load
```

### 3. Docker Compose 配置

```yaml
version: '3'
services:
  openconnect:
    image: zcalusic/openconnect:latest
    container_name: openconnect
    restart: unless-stopped
    privileged: true
    cap_add:
      - NET_ADMIN
    network_mode: host
    volumes:
      - ./certs:/etc/openconnect/certs:ro
    devices:
      - /dev/net/tun
    command: >
      openconnect
      --protocol=anyconnect
      --certificate=/etc/openconnect/certs/client.p12
      --key-password=YOUR_PASSWORD
      --non-inter
      --no-dtls
      YOUR_VPN_SERVER:PORT
```

### 4. 关键点

- `--no-dtls`: 解决 MTU 过大导致的 DTLS 连接失败
- `network_mode: host`: 让 VPN 流量走宿主机网络
- `privileged: true`: 需要创建 tun 设备

### 5. 其他容器使用 VPN

其他容器也需要 `--network host` 才能走 VPN 隧道。

## 参考

- [OpenConnect 官方文档](https://www.infradead.org/openconnect/)
- [zcalusic/openconnect Docker Hub](https://hub.docker.com/r/zcalusic/openconnect)

---
*学习日期: 2026-02-22*