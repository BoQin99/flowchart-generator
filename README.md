# Flowchart Generator Skill

一个用于 Claude Code 的专业技能，使用 MCP draw.io 服务器生成专业的流程图。

## 功能特性

- 🎨 生成专业的 draw.io 流程图
- 🔄 实时预览，支持浏览器中的交互式编辑
- 📦 支持多种流程图类型（业务流程、系统架构、决策流程等）
- 🌍 支持中文文本和自定义样式
- 💾 可导出为 SVG、PNG、PDF 等多种格式

## 前置要求

在安装此 skill 之前，你需要确保：

### 1. 已安装 Claude Code

确保你已安装 [Claude Code CLI](https://claude.ai/code)

### 2. 已配置 draw.io MCP 服务器

此 skill 依赖于 draw `mcp__drawio` MCP 服务器。请确保你的 `claude_desktop_config.json` 中已配置：

```json
{
  "mcpServers": {
    "drawio": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-drawio"]
    }
  }
}
```

**安装 MCP 服务器：**

```bash
# 方式 1：全局安装
npm install -g @modelcontextprotocol/server-drawio

# 方式 2：使用 npx 运行（无需安装，推荐）
npx -y @modelcontextprotocol/server-drawio
```

## 安装步骤

### 步骤 1：克隆或下载仓库

```bash
# 克隆仓库
git clone https://github.com/your-username/flowchart-generator.git

# 或下载 ZIP 文件并解压
```

### 步骤 2：复制 Skill 文件

将 `SKILL.md` 文件复制到你的 Claude Code skills 目录：

```bash
# macOS/Linux
cp SKILL.md ~/.claude/skills/flowchart-generator/SKILL.md

# Windows
copy SKILL.md %USERPROFILE%\.claude\skills\flowchart-generator\SKILL.md
```

**注意：** 确保目录名称与 skill 的 `name` 字段一致（这里是 `flowchart-generator`）。

### 步骤 3：重启 Claude Code

重启 Claude Code 或重新加载技能列表。

### 步骤 4：验证安装

在 Claude Code 中运行：

```
/flowchart-generator
```

如果安装成功，你应该看到 skill 的帮助信息。

## 快速开始

安装完成后，你可以这样使用：

```
创建一个用户登录流程图
```

或者：

```
/flowchart-generator 帮我设计一个支付处理系统的流程图
```

## 使用示例

### 示例 1：用户登录流程

```
创建一个用户登录流程图，包括：
1. 打开登录页面
2. 输入用户名和密码
3. 验证凭证
4. 验证成功：创建会话、跳转首页
5. 验证失败：显示错误、返回输入
```

### 示例 2：系统架构图

```
绘制一个微服务架构图，包含：
- API Gateway
- User Service
- Order Service
- Payment Service
- Database（每个服务一个）
- Redis 缓存层
```

### 示例 3：决策流程

```
创建一个贷款审批决策流程图：
- 检查信用评分
- 检查收入证明
- 检查债务收入比
- 根据条件决定批准/拒绝/需要人工审核
```

## 支持的流程图类型

| 类型 | 描述 |
|------|------|
| 业务流程图 | 描述业务操作的步骤 |
| 系统架构图 | 展示系统组件和关系 |
| 工作流程图 | 团队或个人的工作步骤 |
| 数据流图 | 展示数据在系统中的流动 |
| 决策流程图 | 基于条件的分支决策 |
| 时序图 | 展示组件之间的交互顺序 |
| 用户旅程图 | 展示用户在产品中的路径 |
| 网络拓扑图 | 展示网络设备连接关系 |

## 文件说明

| 文件 | 说明 |
|------|------|
| `SKILL.md` | Skill 定义文件，包含名称、描述和完整工作流程 |
| `README.md` | 本文件，项目说明和快速开始指南 |
| `docs/INSTALLATION.md` | 详细的安装指南（可选） |
| `docs/EXAMPLES.md` | 更多使用示例（可选） |

## 工作原理

1. **分析需求**：理解用户需要创建的流程图类型和内容
2. **文本预览**：先生成 ASCII 或 Markdown 格式的文本流程图
3. **用户确认**：确认设计是否符合预期
4. **生成图表**：调用 draw.io MCP 服务器生成实际图表
5. **实时编辑**：在浏览器中预览和编辑图表
6. **导出保存**：支持多种格式导出

## 故障排除

### 问题：找不到 skill

**解决方案：**
- 检查 `SKILL.md` 是否在正确的目录
- 确保目录名称与 skill 名称一致
- 重启 Claude Code

### 问题：draw.io 服务器未连接

**解决方案：**
- 检查 `claude_desktop_config.json` 配置
- 确保已安装 Node.js 和 npm
- 尝试手动运行 MCP 服务器验证

### 问题：浏览器没有打开

**解决方案：**
- 检查浏览器设置
- 确认 `localhost:6002` 端口未被占用
- 查看 Claude Code 日志获取错误信息

## 贡献

欢迎提交 Issue 和 Pull Request！

## 许可证

MIT License

## 致谢

- [Claude Code](https://claude.ai/code) - AI 编程助手
- [draw.io](https://www.drawio.com/) - 开源流程图工具
- [MCP Server for draw.io](https://github.com/modelcontextprotocol/servers) - MCP 服务器实现

---

**享受创建流程图的乐趣！** 🎉
