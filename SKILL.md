---
name: Web搜索工具
description: 使用MCP服务器（exa、grep-app、MiniMax）进行网络搜索、代码搜索和GitHub查询。通过mcporter调用。适用于：研究、查找代码示例、GitHub代码模式、图片分析。
metadata: {"openclaw":{"requires":{"bins":["mcporter"]}}}
---

# 通过MCP工具进行网络搜索

使用MCP服务器（exa、grep-app、MiniMax）通过`mcporter`进行网络搜索、代码搜索和GitHub查询。

## 快速开始

```bash
mcporter call <server>.<tool> query="..."
```

## 可用的MCP服务器

### 1. exa - 网络和代码搜索

适用于：通用网络搜索、代码示例、公司研究。

```bash
# 网络搜索（新闻、事实、当前信息）
mcporter call 'exa.web_search_exa(query: "搜索词", numResults: 5)'

# 从GitHub/Stack Overflow查找代码示例
mcporter call 'exa.get_code_context_exa(query: "React useState hook", tokensNum: 5000)'

# 公司研究
mcporter call 'exa.company_research_exa(companyName: "Anthropic", numResults: 3)'
```

### 2. grep-app - GitHub代码搜索

适用于：在GitHub仓库中搜索特定的代码模式。

```bash
# 搜索GitHub代码
mcporter call 'grep-app.grep_query(query: "console.log", language: "JavaScript", maxResults: 5)'

# 搜索特定仓库
mcporter call 'grep-app.grep_query(query: "authentication middleware", repo: "expressjs/express", maxResults: 5)'
```

### 3. MiniMax - 网络搜索和图片分析

适用于：轻量级网络搜索、AI图片分析。

```bash
# 网络搜索
mcporter call 'MiniMax.web_search(query: "搜索词", numResults: 5)'

# 图片分析（需要配置API Key）
mcporter call 'MiniMax.understand_image(imageUrl: "https://example.com/image.jpg", prompt: "描述这张图片")'
```

## 示例

### 研究某个主题
```bash
mcporter call 'exa.web_search_exa(query: "iPhone notification sound制作", numResults: 5)'
```

### 查找代码模式
```bash
mcporter call 'exa.get_code_context_exa(query: "Python ffmpeg audio generation", tokensNum: 8000)'
```

### 搜索GitHub
```bash
mcporter call 'grep-app.grep_query(query: "async def", language: "Python", maxResults: 10)'
```

### 分析图片
```bash
mcporter call 'MiniMax.understand_image(imageUrl: "https://example.com/screenshot.png", prompt: "这个页面在显示什么")'
```

## 检查状态

```bash
mcporter list  # 列出所有MCP服务器
mcporter list exa --schema  # 显示exa的工具
mcporter list grep-app --schema  # 显示grep-app的工具
mcporter list MiniMax --schema  # 显示MiniMax的工具
```

## 注意事项

- exa：无需API密钥，返回URL和内容
- grep-app：GitHub代码搜索，可能有速率限制
- MiniMax：网络搜索无需密钥，图片分析需要API Key（见minimax-mcp Skill）

## 使用场景

| 任务 | 工具 |
|------|------|
| 通用网络搜索 | `exa.web_search_exa` 或 `MiniMax.web_search` |
| 查找代码示例 | `exa.get_code_context_exa` |
| 公司信息 | `exa.company_research_exa` |
| GitHub代码模式 | `grep-app.grep_query` |
| AI图片分析 | `MiniMax.understand_image` |

## 技巧

- 使用具体的查询以获得更好的结果
- 根据需要调整`numResults`/`maxResults`（通常5-10个足够）
- 对于代码搜索，指定`language`参数
- 对于仓库特定搜索，使用`repo: "owner/repo"`格式
- 图片分析使用MiniMax需要单独配置（见minimax-mcp Skill）

## 相关Skill

- **minimax-mcp** - MiniMax图片识别专用Skill（包含详细配置说明）
