# 发布指南

## 前置条件

### GitHub Token 权限设置

仓库使用 `GITHUB_TOKEN` 自动发布到 GHCR，无需额外配置 Secret。但需要确保：

1. 进入仓库 **Settings → Actions → General**
2. 找到 **Workflow permissions**，选择 **Read and write permissions**
3. 保存

### 包可见性（可选）

默认发布的包是私有的。如需公开访问：

1. 进入 https://github.com/0xheliuni?tab=packages
2. 找到 `cliproxyapi` 包，点击进入
3. **Package settings → Danger Zone → Change visibility** → 改为 Public

---

## 发布流程

打 tag 即触发自动构建并推送到 `ghcr.io`，支持 amd64 和 arm64 双架构。

```bash
# 打 tag
git tag v1.0.0

# 推送 tag 触发发布
git push origin v1.0.0
```

发布完成后，镜像地址为：

- `ghcr.io/0xheliuni/cliproxyapi:latest`
- `ghcr.io/0xheliuni/cliproxyapi:1.0.0`
- `ghcr.io/0xheliuni/cliproxyapi:1.0`

---

## 服务器部署 / 更新

### 首次部署

1. 将 `docker-compose.yml` 和 `config.yaml` 放到服务器目录

2. 登录 GHCR（如果包是私有的）：

```bash
# 创建一个 Personal Access Token (classic)，勾选 read:packages 权限
# https://github.com/settings/tokens/new
echo "YOUR_GITHUB_PAT" | docker login ghcr.io -u 0xheliuni --password-stdin
```

3. 启动服务：

```bash
docker compose up -d
```

### 更新到最新版本

```bash
docker compose pull
docker compose up -d
```

### 更新到指定版本

```bash
CLI_PROXY_IMAGE=ghcr.io/0xheliuni/cliproxyapi:v1.2.0 docker compose up -d
```
