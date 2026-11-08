# Cuebo 界面组件库

此仓库存放由用户主动安装的声明式界面组件。App 读取 `index.json`，按组件路径下载 `component.json` 并核对 SHA-256。安装后仍需在每个 Agent 的“界面展示”能力中添加，才会提供专用工具。

## 当前组件

- `server-load/`：服务器负载卡。Agent 先通过已授权的运行环境命令读取真实 `uptime`、1/5/15 分钟负载，再调用 `ui_server_load` 展示。卡片使用 App 内置的可交互指标网格。

## 包规范（v1）

`component.json` 是数据清单，字段为 `schemaVersion`、`id`、`name`、`description`、`version`、`kind`、`toolName`。本版只接受 `kind: server_load`、`id: server-load` 和 `toolName: ui_server_load`。界面组件不能携带 JavaScript、Swift 代码、网络地址或自定义 Agent 工具定义；App 和运行内核负责校验与渲染。新增种类需要先在 App 中实现、审核并发布。

仓库维护者更新组件时，需同步更新 `index.json` 中的版本及文件 SHA-256。这样下载文件与清单不一致时安装会失败。
