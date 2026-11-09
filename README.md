# Cuebo 界面组件库

此仓库存放可下载的声明式卡片。App 从 `index.json` 下载并校验包；用户也可以在商店右上角粘贴 GitHub 上的组件目录或 `component.json` 链接。安装后，在 Agent 的卡片能力中添加该组件，模型才会看到它的专用展示工具。

## 示例

- [`server-load/`](server-load/component.json)：用 Cuebo 指标网格展示服务器负载。
- [`service-health/`](service-health/component.json)：用 A2UI 的 `Text`、`Row`、`Card` 和 Cuebo 信息条目组合服务状态。它与服务器负载使用不同的节点结构，用于验证下载包无需 App 专门适配即可预览和调用。

## 包格式（v2）

每个目录包含 `component.json`。根对象必须提供：

| 字段 | 作用 |
| --- | --- |
| `schemaVersion` | 固定为 `2`。 |
| `id`、`name`、`description`、`version` | 组件身份和商店展示信息；`id` 使用小写字母、数字与连字符。 |
| `toolName` | 模型工具名，固定由 `ui_` 加 `id` 的连字符转下划线形成。 |
| `toolDescription` | 注入给模型的工具用途和数据获取要求。 |
| `inputDescriptions` | 每个输入字段的说明，键必须与 `previewData` 一致。 |
| `previewData` | 示例输入；App 用它自动预览，也据此推断工具参数的类型和嵌套结构。 |
| `messages` | 两条 A2UI v0.9 消息：`createSurface` 与 `updateComponents`，surface ID 为 `card`，catalog ID 为 Basic Catalog。 |

在 A2UI 节点的内容属性中使用 `${fieldName}` 引用 `previewData` 的顶层字段。独立占位符可替换为字符串、数字、布尔值、对象或数组；嵌入文字时只接受标量，例如 `"已运行 ${uptimeDays} 天"`。节点 ID、组件类型、父子关系、对齐方式和图表类型必须是静态的。示例输入会自动生成预览；Agent 调用工具时提供同样形状的真实输入，运行内核复用相同的 A2UI 渲染路径。

可使用 App 已支持的 Basic Catalog 基础节点（如 `Text`、`Image`、`Icon`、`Row`、`Column`、`List`、`Card`、`Divider`、`Button`），以及 `CueboMetrics`、`CueboInfo`、`CueboLinkList`、`CueboChart`。下载包只提供数据，不运行 JavaScript、Swift、HTML 或系统命令。当前按钮只允许经校验的 `open_url` 动作；若卡片需要读取服务器或其他真实数据，应让 Agent 先调用已授权的能力，再把结果传给卡片工具。

包大小、节点数量、文本长度、输入深度和链接都会在安装与调用时校验。可参照两个示例直接写新的 `component.json`，无需给 App 增加专属组件分支。

## 更新商店清单

发布或修改组件时，更新 `index.json` 的名称、版本、相对路径和文件 SHA-256。macOS 下可用：

```sh
shasum -a 256 server-load/component.json
```

App 下载时核对清单中的 SHA-256。用户从自己的 GitHub 链接导入时直接校验包格式；请使用受信任的仓库链接。
