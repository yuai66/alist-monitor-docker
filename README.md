# Alist Monitor System

![Framework: Flask](https://img.shields.io/badge/Framework-Flask-blue)
![Deployment: Docker](https://img.shields.io/badge/Deployment-Docker-blueviolet)
![License: MIT](https://img.shields.io/badge/License-MIT-green)

## 项目简介

**Alist 监控系统** 是一个基于 Flask 框架开发的 Docker 工具，专用于监控 [Alist](https://alist.nn.ci/) 存储的健康状态。

本系统可以定期检查 Alist 所有存储的挂载状态，一旦发现异常，会立即通过企业微信机器人发送通知。同时，系统提供了一个简洁美观的前端界面，方便用户实时查看存储状态、动态配置监控参数以及回顾历史通知记录。

  ---

## ✨ 功能特性

  - **定期监控**：可按用户设定的时间间隔（如10分钟、30分钟、1小时等）自动执行检查任务。
  - **异常通知**：当检测到存储状态异常时，能即时通过企业微信机器人推送告警信息。
  - **Web前端界面**：提供直观的图形化界面，集成了所有核心功能，操作简单便捷。
  - **状态速览**：在前端清晰地展示所有 Alist 存储的详细状态信息（名称、路径、驱动、状态等）。
  - **监控统计**：实时显示监控任务的启动时间、累计运行时长、总检查次数等关键统计数据。
  - **灵活配置**：支持在线配置 Alist 地址、API 令牌和企业微信 Webhook，无需修改配置文件或重启服务。
  - **手动操作**：支持手动触发检查、测试通知发送、一键启停监控等操作。

-----

## 🚀 安装与部署

本项目推荐使用 Docker 进行部署，以获得最佳的兼容性和最简便的部署体验。

### 依赖安装

所有项目依赖均已在 `requirements.txt` 文件中列出。在构建 Docker 镜像的过程中，依赖项会自动安装，您无需进行任何手动操作。

### Docker 部署步骤

以下是在您的服务器或 NAS 环境中部署本项目的详细步骤：

1.  **获取项目文件**

    ```bash
    git clone https://github.com/your-username/alist-monitor-docker.git # 请替换为你的项目仓库地址
    ```

2.  **进入项目目录**

    ```bash
    cd alist-monitor-docker
    ```

3.  **构建 Docker 镜像**
    在项目根目录下执行以下命令来构建镜像：

    ```bash
    docker build -t alistmonitor .
    ```

4.  **运行 Docker 容器**
    镜像构建完成后，使用以下命令启动容器。请**务必替换** `your_api_key` 为一个您自己设定的、足够复杂的密码。

    ```bash
    docker run -d \
      --name alistmonitor \
      -p 5000:5000 \
      -e DEBUG=False \
      -e API_KEY=your_api_key \
      --restart=always \
      alistmonitor
    ```

    **参数说明:**

      - `-d`: 后台运行容器。
      - `--name`: 为容器指定一个名称。
      - `-p 5000:5000`: 将主机的 5000 端口映射到容器的 5000 端口。您可以根据需要更改主机端口。
      - `-e DEBUG=False`: 设置为非调试模式以提高安全性。
      - `--restart=always`: 推荐添加，确保 Docker 服务重启后容器能自动启动。

-----

## 📖 使用说明

### 访问前端

部署成功后，通过浏览器访问 `http://<您的服务器IP>:5000` 即可打开前端界面。

### 登录信息

  - **默认账号**: `admin`
  - **默认密码**: `admin`

> **⚠️ 注意事项**
>
> 本系统**不支持**在前端界面直接修改登录密码。如需修改，请自行修改 `config.py` 文件中的 `save_password('admin')` admin 值并重新构建镜像。

### ⚙️ 配置与操作

在前端界面的 **配置信息** 模块中，您可以设置以下核心参数：

  - **Alist 地址**: 您的 Alist 实例的完整访问地址，例如 `http://192.168.1.10:5244`。
  - **Alist 令牌**: 用于访问 Alist API 的令牌。请在 Alist 后台个人资料页查看。
  - **企业微信机器人 Webhook**: 用于接收异常通知的机器人 Webhook 地址。
  - **监控间隔**: 选择一个合适的监控检查周期。

**操作按钮:**

  - **保存配置**: 保存您填写的配置信息。
  - **测试通知**: 向您配置的企业微信机器人发送一条测试消息，以验证其连通性。
  - **手动检查**: 立即触发一次对 Alist 存储状态的全面检查。
  - **开启监控 / 停止监控**: 启动或停止后台的定时监控任务。
  - **清除通知**: 清空前端界面上的历史通知记录。

-----

## 📂 代码结构

```
.
├── app/                  # 主要应用代码目录
│   ├── api.py            # API 接口，处理前端请求
│   ├── config.py         # 配置文件的加载与保存
│   ├── main.py           # Flask 应用入口，包含路由和定时任务
│   ├── monitor.py        # 核心监控逻辑（获取状态、发送通知等）
│   ├── static/           # 静态文件（CSS, JS）
│   └── templates/        # 前端 HTML 模板
├── Dockerfile            # Docker 镜像构建文件
├── requirements.txt      # Python 依赖列表
└── run.sh                # (可选) 启动脚本
```

-----

## 🤝 贡献

非常欢迎您对本项目做出贡献！如果您有任何好的建议或发现了 Bug，请随时在 GitHub 上提交 [Issue](https://www.google.com/search?q=https://github.com/your-username/alist-monitor-docker/issues) 或 [Pull Request](https://www.google.com/search?q=https://github.com/your-username/alist-monitor-docker/pulls)。

-----

## 📜 许可证

本项目遵循 [MIT License](https://www.google.com/search?q=https://github.com/your-username/alist-monitor-docker/blob/main/LICENSE) 开源许可证。
