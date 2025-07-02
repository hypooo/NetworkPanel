# GitHub Actions Docker 构建和推送工作流

这个工作流会自动构建 Docker 镜像并推送到 Docker Hub。

## 🔧 配置要求

在使用此工作流之前，您需要在 GitHub 仓库中配置以下密钥：

### 1. 设置 Docker Hub 凭据

前往 **仓库设置 → Secrets and variables → Actions**，添加以下密钥：

- `DOCKER_USERNAME`: 您的 Docker Hub 用户名
- `DOCKER_PASSWORD`: 您的 Docker Hub 访问令牌（建议使用访问令牌而不是密码）

### 2. 获取 Docker Hub 访问令牌

1. 登录 [Docker Hub](https://hub.docker.com/)
2. 点击右上角头像 → **Account Settings**
3. 选择 **Security** 标签
4. 点击 **New Access Token**
5. 输入令牌描述并选择权限
6. 复制生成的令牌并保存到 GitHub Secrets

## 🚀 触发条件

工作流会在以下情况下自动触发：

- **推送到主分支**：当代码推送到 `main` 或 `master` 分支时
- **创建标签**：当创建以 `v` 开头的标签时（如 `v1.0.0`）
- **手动触发**：可以在 GitHub Actions 页面手动运行

## 🏷️ 镜像标签策略

- **latest**：主分支的最新构建
- **分支名**：对应分支的构建（如 `main`, `develop`）
- **版本标签**：Git 标签对应的版本（如 `v1.0.0`）
- **SHA 标签**：包含分支名和 Git SHA 的标签（如 `main-abc1234`）

## 🖥️ 多平台支持

工作流支持构建多平台镜像：
- `linux/amd64` (x86_64)
- `linux/arm64` (ARM64)

## 📋 使用示例

### 创建版本发布
```bash
git tag v1.0.0
git push origin v1.0.0
```

### 拉取构建的镜像
```bash
# 拉取最新版本
docker pull your-username/network-panel:latest

# 拉取特定版本
docker pull your-username/network-panel:v1.0.0

# 运行容器
docker run -p 8080:80 your-username/network-panel:latest
```

## 🔍 查看构建状态

1. 前往仓库的 **Actions** 标签页
2. 查看 "Build and Push Docker Image" 工作流
3. 点击具体的运行记录查看详细日志

## ⚙️ 自定义配置

如需修改配置，请编辑 `.github/workflows/docker-build-push.yml` 文件：

- 修改 `IMAGE_NAME` 环境变量更改镜像名称
- 调整触发条件（`on` 部分）
- 修改标签策略（`tags` 部分）
- 调整构建平台（`platforms` 部分） 