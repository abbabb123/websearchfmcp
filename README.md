# Web Search MCP Tools

使用MCP服务器（exa、grep-app、context7）进行网络搜索、代码搜索和GitHub查询。

## 功能特性

- 🌐 **网络搜索** - 通过Exa进行高速网络搜索，各种资讯、新闻都可以
- 💻 **代码搜索** - 查找GitHub/Stack Overflow代码示例
- 📊 **公司研究** - 通过Exa研究公司信息
- 🔍 **GitHub搜索** - 使用grep-app搜索特定代码模式
- 📚 **库文档查询** - 使用context7查询官方文档

## 必要配置

### 1. mcporter 配置文件

**配置文件位置**：
- OpenClaw：`<workspace>/config/mcporter.json`
- OpenCode 本地：`~/.mcporter/mcporter.json`
- OpenCode sg1：`/root/.mcporter/mcporter.json`

### 2. 添加MCP服务器配置

```json
{
  "mcpServers": {
    "exa": {
      "baseUrl": "https://mcp.exa.ai/mcp"
    },
    "grep-app": {
      "command": "grep-mcp"
    },
    "context7": {
      "url": "https://mcp.context7.com/mcp"
    },
    "MiniMax": {
      "command": "uvx",
      "args": ["minimax-coding-plan-mcp", "-y"],
      "env": {
        "MINIMAX_API_KEY": "你的API Key",
        "MINIMAX_API_HOST": "https://api.minimaxi.com"
      }
    }
  }
}
```

**注意**：
- context7 可以免费使用，无需 API Key
- MiniMax 需要配置 API Key（见 minimax-mcp Skill）
- **Linux 服务器**需要软链接 uvx：`ln -s /root/.local/bin/uvx /usr/local/bin/uvx`

### 3. 验证安装

```bash
mcporter list
# 应该显示健康的服务器
```

---

## OpenCode 环境配置

### 本地 Mac

配置文件：`~/.mcporter/mcporter.json`

### sg1 服务器

1. 安装 uvx：
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

2. 添加 PATH 到 `~/.bashrc`：
```bash
echo 'export PATH="/root/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

3. 配置文件：`/root/.mcporter/mcporter.json`

## 使用方法

### 网络搜索
```bash
mcporter call 'exa.web_search_exa(query: "搜索词", numResults: 5)'
```

### 代码示例搜索
```bash
mcporter call 'exa.get_code_context_exa(query: "Python list comprehension", tokensNum: 5000)'
```

### 公司研究
```bash
mcporter call 'exa.company_research_exa(companyName: "Anthropic", numResults: 3)'
```

### GitHub代码搜索
```bash
mcporter call 'grep-app.grep_query(query: "console.log", language: "JavaScript", maxResults: 5)'
```

### 库文档查询（context7）
```bash
# 查询库ID
mcporter call 'context7.resolve-library-id' query="React documentation" libraryName="react"

# 查询具体文档
mcporter call 'context7.query-docs' libraryId="/websites/react_dev" query="How to use useState hook"
```

## 查看所有工具
```bash
mcporter list              # 列出所有服务器
mcporter list exa --schema # 查看exa的工具详情
mcporter list grep-app --schema # 查看grep-app的工具详情
mcporter list context7 --schema # 查看context7的工具详情
```

## 文件结构

```
websearchfmcp/
├── SKILL.md  # Skill说明文档（中文）
└── README.md # 使用指南
```

## 相关资源

- **Exa官网**：https://exa.ai
- **grep-app官网**：https://grep.app
- **Context7官网**：https://context7.com

## License

MIT
