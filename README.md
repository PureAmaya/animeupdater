

# animeupdater

一个为 [autofilm](https://github.com/Akimio521/AutoFilm) 项目设计的智能、自动化的管理工具，特别适用于需要触发 cron 更新计划的场景。

-----

## 📖 简介

这个项目最初是为了自动化管理一个基于 [autofilm](https://github.com/Akimio521/AutoFilm) 的 Docker 服务而创建的，但它的设计足够通用，可以轻松适配任何有类似需求的项目。

## ✨ 主要特性

  - **交互式菜单**：提供清爽的命令行菜单，轻松管理您的服务。
  - **一键安装**：单行命令即可完成工具的下载、安装和系统级命令注册。
  - **智能依赖管理**：安装时自动检测并安装所需依赖（如 `yq`），支持主流 Linux 发行版和 macOS。
  - **动态 CRON 更新**：核心功能，一键将配置文件中**所有**的 `cron` 计划更新为指定时间（目前设定为“两分钟后”）。
  - **服务管理**：快捷执行 `docker-compose` 的 `up`, `down`, `logs` 等常用操作。
  - **高度可配置**：初次安装时指定项目路径，工具将自动记忆。
  - **安全可靠**：使用 `yq` 安全地修改 YAML 文件，避免了 `sed` 等工具的格式风险。

## 🎯 环境要求

在开始之前，请确保您的系统满足以下条件：

1.  **操作系统**：Linux 或 macOS。
2.  **Docker 环境**：已安装并正在运行的 Docker 和 Docker Compose。
3.  **必要工具**：已安装 `curl` 或 `wget` 以下载安装脚本。
4.  **管理员权限**：拥有 `sudo` 权限，用于安装工具和依赖。
5.  **项目文件**：您自己的 Docker Compose 项目文件已准备就绪（详见下文）。

## 🚀 一键安装

我们提供了一行命令来完成所有安装工作。它会自动下载最新的 `animeupdater` 脚本，并执行其内置的安装程序。

> **重要提示**：请将下方命令中的 `YOUR_USERNAME` 和 `animeupdater` 替换为您的实际 GitHub 用户名和仓库名！

### 使用 cURL (推荐)

```bash
bash -c "$(curl -LfsS https://raw.githubusercontent.com/PureAmaya/animeupdater/main/animeupdater)" -- install
```

### 使用 Wget

```bash
bash -c "$(wget -O- https://raw.githubusercontent.com/PureAmaya/animeupdater/main/animeupdater)" -- install
```

安装过程中，脚本会：

1.  检查并安装 `Docker` / `yq` 等依赖。
2.  请求您输入 Docker 项目的**绝对路径**。
3.  将 `animeupdater` 安装到 `/usr/local/bin`，使其成为一个全局命令。

## 🛠️ 如何使用

安装成功后，您可以在任何目录下打开终端，直接运行：

```bash
animeupdater
```

您将看到一个清晰的交互式菜单：

```text
========================================
         animeupdater v1.0
========================================
 项目路径: /path/to/your/docker/project
----------------------------------------
  1. 手动关闭 Docker Compose 服务
  2. 手动开启 Docker Compose 服务
  3. 更新所有 CRON 并重启服务
  4. 查看服务日志 (实时)
  5. 进入项目目录 (开启新Shell)
  0. 退出脚本
----------------------------------------
请输入选项 [0-5]:
```

根据提示输入数字，即可执行相应操作。

### 卸载工具

如果您想移除本工具，只需运行：

```bash
sudo animeupdater uninstall
```

## 📁 项目文件结构要求

`animeupdater` 要求您的项目遵循特定的文件结构，以便脚本能够正确找到配置文件。在运行安装程序**之前**，请确保您的项目目录如下所示：

```
/path/to/your/project/    <-- 安装时您需要输入的路径
├── docker-compose.yml
└── config/
    └── config.yaml       <-- 包含 cron 计划的配置文件
```

其中，`config.yaml` 的内容应类似于autofilm的[配置文件](https://github.com/Akimio521/AutoFilm/blob/main/config/config.yaml.example)：

```yaml
Settings:
  DEV: False
Alist2StrmList:
  - id: 动漫
    cron: '11 22 * * *'  # 脚本会自动修改此处的 cron 值
    url: https://pan.example.com
    # ... 其他配置
  - id: 电影
    cron: '15 03 * * *'  # 脚本也会自动修改此处的 cron 值
    url: https://pan.example.com
    # ... 其他配置
```

## 💡 技术实现

本项目是一个纯 `Bash` 脚本，其核心功能依赖于以下工具：

  - `docker-compose`：用于管理 Docker 服务的生命周期。
  - `yq`：一个强大的命令行 YAML 处理器，用于安全、准确地读取和修改 `.yaml` 文件内容。
  - `curl` / `wget`：用于实现一键安装功能。

## 🤝 贡献

欢迎各种形式的贡献！如果您有任何好的想法或发现了 Bug，请随时提交 Pull Request 或创建 Issue。

## 📄 许可证

本项目基于 [MIT License](https://opensource.org/licenses/MIT) 授权。
