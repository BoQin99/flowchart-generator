# 安装指南

本文档提供详细的安装步骤，帮助你成功安装 Flowchart Generator Skill。

## 目录

1. [前置检查](#前置检查)
2. [安装 Claude Code](#安装-claude-code)
3. [安装 MCP draw.io 服务器](#安装-mcp-drawio-服务器)
4. [配置 MCP 服务器](#配置-mcp-服务器)
5. [安装 Skill](#安装-skill)
6. [验证安装](#验证安装)
7. [常见问题](#常见问题)

---

## 前置检查

在开始之前，请确保你的系统满足以下要求：

| 要求 | 最低版本 | 命令检查 |
|------|---------|---------|
| 操作系统 | macOS 12+, Windows 10+, Linux | `uname` (macOS/Linux) 或 `ver` (Windows) |
| Node.js | 18.0+ | `node --version` |
| npm | 9.0+ | `npm --version` |
| Claude Code | 最新版 | `claude --version` |

---

## 安装 Claude Code

如果你还没有安装 Claude Code，请访问 [claude.ai/code](https://claude.ai/code) 下载并安装。

安装完成后，在终端中运行：

```bash
claude --version
```

应该看到版本号输出。

---

## 安装 MCP draw.io 服务器

### 什么是 MCP draw.io 服务器？

MCP (Model Context Protocol) draw.io 服务器是 [Anthropic](https://www.anthropic.com/) 官方提供的 MCP 服务器实现，它封装了 [draw.io](https://www.drawio.com/) 流程图工具，允许 Claude Code 通过 MCP 协议创建、编辑和管理流程图。

**MCP draw.io 服务器信息：**

| 项目 | 信息 |
|------|------|
| **包名** | `@modelcontextprotocol/server-drawio` |
| **作者/维护者** | Anthropic |
| **仓库地址** | https://github.com/modelcontextprotocol/servers |
| **开源协议** | MIT |
| **功能** | 在浏览器中打开 draw.io 编辑器，支持创建、编辑和导出流程图 |

### MCP draw.io 服务器提供的工具

该 MCP 服务器提供以下工具，供 skill 调用：

| 工具名称 | 说明 |
|---------|------|
| `mcp__drawio__start_session` | 启动新的 draw.io 编辑会话，在浏览器中打开 |
| `mcp__drawio__create_new_diagram` | 创建新的流程图 |
| `mcp__drawio__get_diagram` | 获取当前流程图的内容 |
| `mcp__drawio__edit_diagram` | 编辑现有流程图（添加/更新/删除节点） |
| `mcp__drawio__export_diagram` | 导出流程图为 PNG、SVG 或 drawio 格式 |

### 安装 Node.js 和 npm

MCP 服务器需要 Node.js 运行环境。如果你还没有安装：

**macOS（使用 Homebrew）：**
```bash
brew install node
```

**Linux（Ubuntu/Debian）：**
```bash
sudo apt update
sudo apt install nodejs npm
```

**Windows：**
访问 [nodejs.org](https://nodejs.org/) 下载并安装。

**验证安装：**
```bash
node --version  # 应该显示 18.0 或更高版本
npm --version   # 应该显示 9.0 或更高版本
```

### 安装 MCP draw.io 服务器

有两种安装方式：

#### 方式 A：使用 npx（推荐）

使用 `npx` 可以直接运行 npm 包，无需预先安装。这是推荐的方式，因为它：

- 不需要全局安装
- 自动使用最新版本
- 不会污染全局 npm 模块

**验证 npx 可用：**
```bash
npx --version
```

**测试 MCP 服务器（可选）：**
```bash
npx -y @modelcontextprotocol/server-drawio
```
- `-y` 参数表示自动确认安装
- 如果成功，会看到 MCP 服务器的启动信息

#### 方式 B：全局安装

如果你希望全局安装以便其他项目使用：

```bash
npm install -g @modelcontextprotocol/server-drawio
```

**验证安装：**
```bash
npm list -g @modelcontextprotocol/server-drawio
```

---

## 配置 MCP 服务器

### 找到配置文件

Claude Desktop 的配置文件位于：

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

### 编辑配置文件

使用你喜欢的文本编辑器打开配置文件（如果文件不存在，创建它）。

然后添加或更新 MCP 服务器配置：

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

**配置说明：**

| 字段 | 值 | 说明 |
|------|-----|------|
| `"drawio"` | - | MCP 服务器的名称，skill 通过 `mcp__drawio__*` 调用 |
| `"command"` | `"npx"` | 运行命令，使用 npx 运行 npm 包 |
| `"args"` | `["-y", "@modelcontextprotocol/server-drawio"]` | 传递给命令的参数 |

**如果你使用了全局安装（方式 B），配置如下：**

```json
{
  "mcpServers": {
    "drawio": {
      "command": "mcp-server-drawio"
    }
  }
}
```

### 验证配置文件格式

确保配置文件是有效的 JSON：

```bash
# macOS/Linux 验证
cat ~/Library/Application\ Support/Claude/claude_desktop_config.json | python3 -m json.tool

# 或使用 jq（如果已安装）
cat ~/Library/Application\ Support/Claude/claude_desktop_config.json | jq .
```

### 重启 Claude Desktop

配置完成后，**必须完全退出并重新启动 Claude Desktop 应用**，配置才会生效。

### 验证 MCP 连接

重启后，在 Claude Desktop 中：

1. 打开设置或查看 MCP 选项卡
2. 应该能看到 `drawio` 服务器
3. 状态应该显示为 "已连接" 或 "Connected"

如果看到连接错误，请查看 [常见问题](#常见问题) 部分。

---

## 安装 Skill

### 方式 A：使用 Git 克隆（推荐）

```bash
# 1. 克隆仓库
git clone https://github.com/BoQin99/flowchart-generator.git

# 2. 进入目录
cd flowchart-generator

# 3. 创建 skills 目录（如果不存在）
mkdir -p ~/.claude/skills

# 4. 复制 skill
cp -r flowchart-generator ~/.claude/skills/
```

**Windows PowerShell:**
```powershell
# 1. 克隆仓库
git clone https://github.com/BoQin99/flowchart-generator.git

# 2. 进入目录
cd flowchart-generator

# 3. 创建 skills 目录
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude\skills"

# 4. 复制 skill
Copy-Item -Recurse -Force . "$env:USERPROFILE\.claude\skills\flowchart-generator"
```

### 方式 B：手动下载

1. 访问 [GitHub 仓库](https://github.com/BoQin99/flowchart-generator)
2. 点击 "Code" → "Download ZIP"
3. 解压到临时目录
4. 将解压后的内容复制到 `~/.claude/skills/flowchart-generator/`

### 方式 C：直接复制 SKILL.md（最小安装）

如果你只需要 SKILL.md 文件：

```bash
# macOS/Linux
mkdir -p ~/.claude/skills/flowchart-generator
curl -o ~/.claude/skills/flowchart-generator/SKILL.md \
  https://raw.githubusercontent.com/BoQin99/flowchart-generator/main/SKILL.md

# Windows PowerShell
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude\skills\flowchart-generator"
Invoke-WebRequest -Uri "https://raw.githubusercontent.com/BoQin99/flowchart-generator/main/SKILL.md" `
  -OutFile "$env:USERPROFILE\.claude\skills\flowchart-generator\SKILL.md"
```

**重要：**
- 目录名称 `flowchart-generator` 必须与 SKILL.md 中的 `name` 字段一致
- 确保 SKILL.md 文件格式正确（以 `---` 开头的 YAML frontmatter）

---

## 验证安装

### 步骤 1：检查文件结构

```bash
ls -la ~/.claude/skills/flowchart-generator/
```

应该至少包含：
```
SKILL.md
```

如果使用了完整克隆，还会看到：
```
SKILL.md
README.md
LICENSE
docs/
.github/
```

### 步骤 2：检查 SKILL.md 格式

```bash
# 查看前几行，应该有 YAML frontmatter
head -10 ~/.claude/skills/flowchart-generator/SKILL.md
```

应该看到：
```
---
name: flowchart-generator
description: ...
---
```

### 步骤 3：启动 Claude Code

```bash
claude
```

### 步骤 4：测试 Skill

在 Claude Code 中输入：

```
/flowchart-generator 创建一个简单的测试流程图
```

**预期结果：**

1. Claude Code 应该识别并调用 flowchart-generator skill
2. 应该看到浏览器中自动打开 draw.io 编辑器
3. 编辑器中应该显示一个简单的流程图

### 步骤 5：测试 MCP 连接

如果在 draw.io 浏览器窗口中能看到图表并可以编辑，说明 MCP 服务器连接成功。

---

## 常见问题

### Q1: 技能列表中没有显示 flowchart-generator

**症状：** 在 Claude Code 中看不到 flowchart-generator skill

**可能原因：**
- SKILL.md 文件不在正确的目录
- 目录名称与 skill 名称不匹配
- SKILL.md 格式不正确（缺少 YAML frontmatter）
- 文件权限问题

**解决方法：**

```bash
# 1. 检查文件是否存在
ls -la ~/.claude/skills/flowchart-generator/SKILL.md

# 2. 检查目录名称（必须是 flowchart-generator）
ls -la ~/.claude/skills/

# 3. 检查 SKILL.md 格式（前几行）
head -10 ~/.claude/skills/flowchart-generator/SKILL.md

# 4. 检查文件权限
ls -l ~/.claude/skills/flowchart-generator/SKILL.md

# 5. 如果权限不对，修复权限
chmod 644 ~/.claude/skills/flowchart-generator/SKILL.md
```

### Q2: MCP draw.io 服务器连接失败

**症状：**
- Claude Desktop MCP 选项卡中看不到 drawio
- drawio 状态显示 "未连接" 或错误信息

**可能原因：**
- Node.js 或 npm 未安装或版本过低
- 配置文件路径错误
- 配置文件 JSON 格式错误
- npx 命令不可用
- MCP 服务器包无法下载

**解决方法：**

```bash
# 1. 检查 Node.js 和 npm 版本
node --version  # 需要 18.0+
npm --version   # 需要 9.0+

# 2. 检查 npx 可用
npx --version

# 3. 验证配置文件存在
cat ~/Library/Application\ Support/Claude/claude_desktop_config.json

# 4. 验证配置文件 JSON 格式
cat ~/Library/Application\ Support/Claude/claude_desktop_config.json | python3 -m json.tool

# 5. 手动测试 MCP 服务器（这会尝试下载并运行）
npx -y @modelcontextprotocol/server-drawio

# 6. 检查网络连接（npm 仓库）
npm ping
```

### Q3: draw.io 浏览器窗口没有打开

**症状：** 调用 skill 后浏览器没有弹出，或弹出但页面空白

**可能原因：**
- 端口 6002 被占用
- 防火墙阻止了本地连接
- 浏览器设置问题
- MCP 会话创建失败

**解决方法：**

```bash
# 1. 检查端口 6002 是否被占用
lsof -i :6002  # macOS/Linux
netstat -ano | findstr :6002  # Windows

# 2. 如果被占用，结束占用进程或关闭 draw.io 会话

# 3. 检查防火墙设置（确保允许 localhost 连接）

# 4. 检查浏览器默认设置

# 5. 查看 Claude Code 日志获取详细错误信息
```

### Q4: npx 命令未找到

**症状：** 错误信息显示 "npx: command not found"

**解决方法：**

```bash
# 1. 重新安装 Node.js（完整版，包含 npm 和 npx）
#    访问 nodejs.org 下载完整安装包

# 2. 或使用全局安装方式
npm install -g @modelcontextprotocol/server-drawio

# 3. 然后修改配置文件为：
#    {
#      "mcpServers": {
#        "drawio": {
#          "command": "mcp-server-drawio"
#        }
#      }
#    }
```

### Q5: npm 无法下载 MCP 服务器包

**症状：** 错误信息显示网络问题、下载失败

**可能原因：**
- 网络连接问题
- npm 仓库访问受限
- 需要配置代理

**解决方法：**

```bash
# 1. 检查网络连接
ping registry.npmjs.org

# 2. 检查 npm 配置
npm config get registry

# 3. 如果使用代理，配置 npm 代理
npm config set proxy http://proxy-server:port
npm config set https-proxy http://proxy-server:port

# 4. 尝试切换 npm 镜像源（如使用淘宝镜像）
npm config set registry https://registry.npmmirror.com
```

### Q6: 图表显示中文乱码

**症状：** 生成的流程图中中文显示为方块或乱码

**解决方法：**

1. 在 draw.io 浏览器中：
   - 菜单 → Extras → Preferences
   - 修改字体设置，选择支持中文的字体（如 Arial Unicode MS、Microsoft YaHei）

2. 在创建图表时指定中文字体：
   ```xml
   style="rounded=1;fontFamily=Microsoft YaHei;"
   ```

### Q7: SKILL.md 更新后没有生效

**症状：** 修改了 SKILL.md 但 Claude Code 仍使用旧版本

**解决方法：**

```bash
# 1. 重启 Claude Code
# 退出并重新启动 claude 命令

# 2. 检查文件时间戳
stat ~/.claude/skills/flowchart-generator/SKILL.md

# 3. 验证修改已保存
tail -10 ~/.claude/skills/flowchart-generator/SKILL.md
```

---

## 卸载

如果需要卸载此 skill：

```bash
# macOS/Linux
rm -rf ~/.claude/skills/flowchart-generator

# Windows PowerShell
Remove-Item -Recurse -Force "$env:USERPROFILE\.claude\skills\flowchart-generator"
```

如果需要移除 MCP draw.io 服务器配置：

1. 编辑 `claude_desktop_config.json`
2. 删除 `mcpServers.drawio` 部分
3. 重启 Claude Desktop

---

## 需要帮助？

如果遇到其他问题：

1. 查看 [GitHub Issues](https://github.com/BoQin99/flowchart-generator/issues)
2. 搜索类似问题
3. 提交新的 Issue，请包含：
   - 操作系统及版本
   - Node.js 版本 (`node --version`)
   - npm 版本 (`npm --version`)
   - Claude Code 版本 (`claude --version`)
   - 完整的错误信息和日志
   - 相关配置文件内容（可脱敏）

---

## 参考资源

| 资源 | 链接 |
|------|------|
| Claude Code | https://claude.ai/code |
| MCP 协议 | https://modelcontextprotocol.io/ |
| MCP Servers 仓库 | https://github.com/modelcontextprotocol/servers |
| draw.io 官网 | https://www.drawio.com/ |

---

**祝你安装顺利！** 🚀
