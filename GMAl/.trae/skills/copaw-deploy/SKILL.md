---
name: "copaw-deploy"
description: "一键自动部署 CoPaw 桌面 Agent 工具。当用户需要安装 CoPaw 但不熟悉编程和命令行操作时使用。"
---

# CoPaw 一键部署技能

## 功能说明

此技能提供了 CoPaw（阿里开源的桌面 Agent 工具）的一键自动部署功能，适合完全不会编程的用户。

## 部署步骤

### 1. 执行部署脚本

脚本会自动完成以下操作：
- 检查 Python 环境
- 安装 CoPaw 包
- 初始化默认配置
- 启动 CoPaw 服务

### 2. 访问 CoPaw 控制台

服务启动后，在浏览器中访问：`http://127.0.0.1:8088`

## 技术要求

- **操作系统**：macOS 或 Linux
- **Python 版本**：3.10 到 3.13
- **网络连接**：需要访问 PyPI 源

## 部署脚本

脚本已保存至：`/Users/gm/Desktop/工作/S-人工智能/GMAl/04其他技能/CoPaw/install_copaw.sh`

## 使用方法

1. 打开终端
2. 进入脚本所在目录：
   ```bash
   cd /Users/gm/Desktop/工作/S-人工智能/GMAl/04其他技能/CoPaw
   ```
3. 赋予脚本执行权限：
   ```bash
   chmod +x install_copaw.sh
   ```
4. 运行脚本：
   ```bash
   ./install_copaw.sh
   ```

## 常见问题

### Q: 安装失败怎么办？
A: 检查网络连接是否正常，确保可以访问 PyPI 源。

### Q: 服务无法启动怎么办？
A: 检查端口 8088 是否被占用，或尝试使用其他端口启动。

### Q: 如何停止服务？
A: 在终端中按 Ctrl+C 停止服务。

## 配置说明

- 配置文件位置：`~/.copaw/config.json`
- 默认服务地址：`http://127.0.0.1:8088`

## 注意事项

- 首次运行会自动下载依赖包，可能需要一些时间
- 建议使用 Python 3.10 或 3.11 版本以获得最佳兼容性
- 如需自定义配置，可以在初始化时不使用 `--defaults` 参数