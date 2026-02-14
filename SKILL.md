---
name: Web搜索工具
description: 需要搜索网络资料时优先使用此skill,使用（exa、grep-app、Context7、MiniMax）进行网络搜索、代码搜索、文档查询和GitHub查询。通过mcporter命令调用。适用于：搜索,学习,研究、查找代码示例、GitHub代码模式、官方文档查询、图片分析。
metadata:
  openclaw:
    requires:
      bins:
        - mcporter
---

# 通过搜索工具进行网络搜索

使用（exa、grep-app、Context7、MiniMax）通过执行bash命令`mcporter`进行网络信息搜索、各种资讯、新闻都可以、代码搜索、文档查询和GitHub查询。

## 快速开始（bash命令直接执行）

```bash
mcporter call <server>.<tool> query="..."
```

## 可用的MCP服务器

### 1. exa - 网络和代码搜索

适用于：通用各种网络信息的搜索（新闻、资讯）、代码示例、公司研究

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

### 3. Context7 - 官方文档查询

适用于：获取最新、版本特定的官方文档和代码示例。**无需 API Key**。

**⚠️ 重要：必须两步流程**

**第一步：解析库 ID**
```bash
mcporter call 'context7.resolve-library-id(query: "你的问题或用途", libraryName: "库名")'
```

**第二步：查询文档**
```bash
mcporter call 'context7.query-docs(libraryId: "/expressjs/express", query: "如何创建 HTTP 服务器")'
```

**使用示例**：
```bash
# 查询 React 最新文档
mcporter call 'context7.resolve-library-id(query: "如何使用 React 创建组件", libraryName: "react")'
# 返回：/facebook/react

mcporter call 'context7.query-docs(libraryId: "/facebook/react", query: "如何创建函数组件")'

# 查询特定版本文档
mcporter call 'context7.query-docs(libraryId: "/expressjs/express/v4.21.2", query: "中间件使用方法")'

# 查询 Python 库
mcporter call 'context7.resolve-library-id(query: "如何使用 FastAPI 创建 API", libraryName: "fastapi")'
mcporter call 'context7.query-docs(libraryId: "/tiangolo/fastapi", query: "路由和请求参数")
```

**优势**：
- ✅ 获取最新官方文档
- ✅ 版本精确匹配
- ✅ 减少代码幻觉
- ✅ 包含真实代码示例

### 4. MiniMax - 网络搜索和图片分析

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

### 搜索图片（通用方法）

**仅部分返回图片 URL**：

| 搜索工具                | 返回结构                                 | 可能包含图片的字段 |
| ------------------- | ------------------------------------ | --------- |
| MiniMax.web_search  | `{ "organic": [{...}] }`             | `snippet` |
| exa.web_search_exa  | `[{...}]` 数组                         | `text`    |
| grep-app.grep_query | `{ "results_by_repository": [...] }` | ❌ 无       |
| context7            | 文本列表                                 | ❌ 无       |

**步骤**：
1. 使用 MiniMax 或 exa 搜索
2. 检查返回结果中的 `snippet` 或 `text` 字段
3. 提取 `<img src="...">` 或 `![]]` 中的 URL
4. curl 下载

## 检查状态

```bash
mcporter list  # 列出所有MCP服务器
mcporter list exa --schema  # 显示exa的工具
mcporter list grep-app --schema  # 显示grep-app的工具
mcporter list context7 --schema  # 显示Context7的工具
mcporter list MiniMax --schema  # 显示MiniMax的工具
```

## 注意事项

- exa：无需API密钥，返回URL和内容
- grep-app：GitHub代码搜索，可能有速率限制
- Context7：**无需API Key**，必须先调用 `resolve-library-id` 获取库ID
- MiniMax：网络搜索无需密钥，图片分析需要API Key（见minimax-mcp Skill）

## 使用场景

| 任务 | 工具 |
|------|------|
| 通用网络搜索 | `exa.web_search_exa` 或 `MiniMax.web_search` |
| 查找代码示例 | `exa.get_code_context_exa` |
| 公司信息 | `exa.company_research_exa` |
| GitHub代码搜索 | `grep-app.grep_query` |
| 官方文档查询 | `context7.resolve-library-id` → `context7.query-docs` |
| 图片分析（AI识别） | `MiniMax.understand_image` |
| 图片搜索（提取URL） | 任意MCP搜索 → 从返回文本字段提取URL → curl下载 |

## 技巧

- 使用具体的查询以获得更好的结果
- 根据需要调整`numResults`/`maxResults`（通常5-10个足够）
- 对于代码搜索，指定`language`参数
- 对于仓库特定搜索，使用`repo: "owner/repo"`格式
- **Context7 必须先解析库ID**，再查询文档
- 图片分析使用MiniMax需要单独配置（见minimax-mcp Skill）

## 要求

- 注意多渠道搜索,搜索尽量多的信息源,要去权威网站或者官方网站或者专业类网站搜索,交叉对比,确保信息的可靠性
- 不能只看搜索api返回的摘要,还要去对应的URL看具体内容,用搜索工具或浏览器工具
- 注意如果是新闻/官方规则等等的,一定要注意时效性
- 建议的步骤:先去相关新闻网站或者社交媒体搜索,找出热门的"对象",然后再去对应的渠道搜索他们的具体内容
- 最终返回给用户的结果一定要确保真实性,时效性,不能自己乱编造,要标注来源和发布时间

## 相关Skill

- **minimax-mcp** - MiniMax图片识别专用Skill（包含详细配置说明）
