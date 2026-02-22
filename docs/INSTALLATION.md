# 安装指南

本文档提供详细的安装步骤，帮助你成功安装 Flowchart Generator Skill。

## 目录

1. [前置检查](#前置检查)
2. [安装 Claude Code](#安装-claude-code)
3. [配置 MCP 服务器](#配置-mcp-服务器)
4. [安装 Skill](#安装-skill)
5. [验证安装](#验证安装)
6. [常见问题](#常见问题)

---

## 前置检查

在开始之前，请确保你的系统满足以下要求：

| 要求 | 最低版本 | 命令检查 |
|------|---------|---------|
| 操作系统 | macOS 12+, Windows 10+, Linux | `uname -a` (macOS/Linux) 或 `ver` (Windows) |
| Node.js | 18.0+ | `node --version` |
| npm | 9.0+ | `npm --version` |
| Claude Code | 最新版 | `claude --version` |

---

## 安装 Claude Code

如果你还没有安装 Claude Code，请访问 https://claude.ai/code 下载并安装。

安装完成后，在终端中运行：

```bash
claude --version
```

应该看到版本号输出。

---

## 配置 MCP 服务器

### 步骤 1：安装 Node.js 依赖

确保你有 Node.js 和 npm：

```bash
node --version
npm --version
```

如果没有，请访问 [nodejs.org](https://nodejs.org/) 下载并安装。

### 步骤 2：配置 MCP 服务器

找到 Claude Desktop 的配置文件：

**macOS:**
```bash
~/Library/Application Support/Claude/claude_desktop_config.json
```

**Windows:**
```
%APPDATA%\Claude\claude_desktop_config.json
```

**Linux:**
```bash
~/.config/Claude/claude_desktop_config.json
```

在配置文件中添加或更新 MCP 服务器配置：

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

### 步骤 3：重启 Claude Desktop

配置完成后，重启 Claude Desktop 应用。

### 步骤 4：验证 MCP 连接

在 Claude Desktop 中，你应该能看到 "MCP" 选项卡，其中包含 "drawio" 服务器。

---

## 安装 Skill

### 方式 A：使用 Git 克隆（推荐）

```bash
# 1. 克隆仓库
git clone https://github.com/your-username/flowchart-generator.git /tmp/flowchart-generator

# 2. 创建 skills 目录（如果不存在）
mkdir -p ~/.claude/skills

# 3. 复制 skill
cp -r /tmp/flowchart-generator ~/.claude/skills/
```

### 方式 B：手动下载

1. 访问仓库的 Releases 页面
2. 下载最新的 ZIP 文件
3. 解压到 `~/.claude/skills/flowchart-generator/`

### 方式 C：直接复制文件

如果你已经下载了 `SKILL.md`：

```bash
# macOS/Linux
mkdir -p ~/.claude/skills/flowchart-generator
cp SKILL.md ~/.claude/skills/flowchart-generator/

# Windows PowerShell
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude\skills\flowchart-generator"
Copy-Item SKILL.md -Destination "$env:USERPROFILE\.claude\skills\flowchart-generator\"
```

---

## 验证安装

### 步骤 1：检查文件结构

```bash
ls -la ~/.claude/skills/flowchart-generator/
```

应该看到：
```
SKILL.md
README.md
docs/
```

### 步骤 2：启动 Claude Code

```bash
claude
```

### 步骤 3：测试 Skill

在 Claude Code 中输入：

```
/flowchart-generator 创建一个简单的测试流程图
```

如果成功，你应该看到浏览器中打开 draw.io 编辑器。

---

## 常见问题

### Q1: 技能列表中没有显示 flowchart-generator

**可能原因：**
- 文件路径错误
- `SKILL.md` 格式不正确
- 目录名称与 skill 名称不匹配

**解决方法：**
```bash
# 检查文件是否存在
ls ~/.claude/skills/flowchart-generator/SKILL.md

# 检查 skill 名称是否正确
head -3 ~/.claude/skills/flowchart-generator/SKILL.md
```

### Q2: MCP 服务器连接失败

**可能原因：**
- Node.js 或 npm 未安装
- 配置文件路径错误
- MCP 服务器未安装

**解决方法：**
```bash
# 检查 Node.js
node --version
npm --version

# 手动测试 MCP 服务器
npx -y @modelcontextprotocol/server-drawio
```

### Q3: draw.io 浏览器窗口没有打开

**可能原因：**
- 端口被占用
- 防火墙阻止
- 浏览器设置问题

**解决方法：**
```bash
# 检查端口 6002 是否被占用
lsof -i :6002  # macOS/Linux
netstat -ano | findstr :6002  # Windows
```

### Q4: 图表显示中文乱码

**可能原因：**
- draw.io 字体设置

**解决方法：**
在 draw.io 中，选择：菜单 → Extras → Preferences → 修改字体设置

---

## 需要帮助？

如果遇到其他问题：

1. 查看 [GitHub Issues](https://github.com/your-username/flowchart-generator/issues)
2. 提交新的 Issue，包含：
   - 操作系统版本
   - Node.js 版本
   - Claude Code 版本
   - 错误信息和日志

---

**祝你安装顺利！** 🚀
