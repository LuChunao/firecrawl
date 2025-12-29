# 🔥 Firecrawl

为您的AI应用提供来自任何网站的干净数据。提供先进的爬取、抓取和数据提取功能。

_此仓库正在开发中，我们仍在将自定义模块集成到monorepo中。目前还不能完全用于自托管部署，但您可以在本地运行它。_

## Firecrawl是什么？

[Firecrawl](https://firecrawl.dev?ref=github) 是一个API服务，它接收一个URL，爬取它，并将其转换为干净的markdown或结构化数据。我们爬取所有可访问的子页面，并为您提供每个页面的干净数据。不需要站点地图。请查看我们的[文档](https://docs.firecrawl.dev)。

寻找我们的MCP？请查看[这个仓库](https://github.com/firecrawl/firecrawl-mcp-server)。

_嘿，你，加入我们的star收藏者吧 :) _

## 如何使用？

我们提供易于使用的API与我们的托管版本。您可以在[这里](https://firecrawl.dev/playground)找到playground和文档。您也可以自托管后端。

查看以下资源开始使用：

- [x] **API**: [文档](https://docs.firecrawl.dev/api-reference/introduction)
- [x] **SDKs**: [Python](https://docs.firecrawl.dev/sdks/python), [Node](https://docs.firecrawl.dev/sdks/node)
- [x] **LLM框架**: [Langchain (python)](https://python.langchain.com/docs/integrations/document_loaders/firecrawl/), [Langchain (js)](https://js.langchain.com/docs/integrations/document_loaders/web_loaders/firecrawl), [Llama Index](https://docs.llamaindex.ai/en/latest/examples/data_connectors/WebPageDemo/#using-firecrawl-reader), [Crew.ai](https://docs.crewai.com/), [Composio](https://composio.dev/tools/firecrawl/all), [PraisonAI](https://docs.praison.ai/firecrawl/), [Superinterface](https://superinterface.ai/docs/assistants/functions/firecrawl), [Vectorize](https://docs.vectorize.io/integrations/source-connectors/firecrawl)
- [x] **低代码框架**: [Dify](https://dify.ai/blog/dify-ai-blog-integrated-with-firecrawl), [Langflow](https://docs.langflow.org/), [Flowise AI](https://docs.flowiseai.com/integrations/langchain/document-loaders/firecrawl), [Cargo](https://docs.getcargo.io/integration/firecrawl), [Pipedream](https://pipedream.com/apps/firecrawl/integrations)
- [x] **社区SDKs**: [Go](https://docs.firecrawl.dev/sdks/go), [Rust](https://docs.firecrawl.dev/sdks/rust)
- [ ] 想要SDK或集成？请提交issue告诉我们。

要在本地运行，请参考[此指南](https://github.com/firecrawl/firecrawl/blob/main/CONTRIBUTING.md)。

### API密钥

要使用API，您需要在[Firecrawl](https://firecrawl.dev)注册并获取API密钥。

### 功能特性

- [**抓取**](#抓取): 抓取URL并以LLM就绪格式获取其内容 (markdown, 结构化数据 via [LLM提取](#llm提取-beta), 截图, html)
- [**爬取**](#爬取): 抓取网页所有URL并以LLM就绪格式返回内容
- [**映射**](#映射): 输入网站并获取所有网站URLs - 速度极快
- [**搜索**](#搜索): 搜索网页并获取结果的完整内容
- [**提取**](#提取): 使用AI从单个页面、多个页面或整个网站获取结构化数据。

### 强大的功能

- **LLM就绪格式**: markdown, 结构化数据, 截图, HTML, 链接, 元数据
- **困难的部分**: 代理，反机器人机制，动态内容 (js渲染), 输出解析, 编排
- **可定制化**: 排除标签，在认证墙后爬取自定义headers, 最大爬取深度等...
- **媒体解析**: PDFs, docx, 图片
- **可靠性优先**: 设计用于获取您需要的数据 - 无论多么困难
- **动作**: 点击、滚动、输入、等待等，然后提取数据
- **批处理**: 同时抓取数千个URLs的新异步端点
- **变更跟踪**: 监控和检测网站内容随时间的变化

您可以在我们的[文档](https://docs.firecrawl.dev)中找到Firecrawl的所有功能和使用方法。

## 仓库代码文件结构总结

这个Firecrawl仓库是一个完整的monorepo项目，包含了完整的API服务、多个编程语言的SDK、辅助服务和测试设施。以下是主要代码文件的详细总结：

### 🏗️ 核心架构

#### **主API服务** (`apps/api/`)
- **语言**: Node.js/TypeScript
- **主要文件**:
  - `src/index.ts` - 应用入口点
  - `src/controllers/` - API控制器 (v0/v1/v2版本)
    - `v2/scrape.ts` - 网页抓取控制器
    - `v2/crawl.ts` - 网页爬取控制器
    - `v2/extract.ts` - 数据提取控制器
    - `v2/map.ts` - 网站映射控制器
    - `v2/search.ts` - 搜索功能控制器
  - `src/lib/` - 核心库函数
    - `scraper/` - 网页抓取逻辑
    - `extract/` - 数据提取功能
    - `search/` - 搜索功能实现
  - `src/services/` - 后台服务
    - `queue-service.ts` - 队列服务
    - `billing/` - 计费服务
    - `webhook/` - Webhook服务

#### **SDK集合**

1. **JavaScript SDK** (`apps/js-sdk/`)
   - **入口**: `firecrawl/src/index.ts`
   - **功能**: 完整的Firecrawl API客户端
   - **示例**: `example.js`, `example_v1.js`等

2. **Python SDK** (`apps/python-sdk/`)
   - **入口**: `firecrawl/__init__.py`
   - **核心**: `firecrawl/client.py`
   - **版本**: 支持v1和v2 API
   - **测试**: 完整的单元测试套件

3. **Rust SDK** (`apps/rust-sdk/`)
   - **入口**: `src/lib.rs`
   - **功能**: 原生Rust实现的API客户端
   - **示例**: `examples/`目录下的各种使用示例

4. **Go HTML转换服务** (`apps/go-html-to-md-service/`)
   - **语言**: Go
   - **功能**: HTML到Markdown的转换服务
   - **部署**: Docker容器化

#### **辅助服务**

- **Redis服务** (`apps/redis/`) - 缓存和队列存储
- **PostgreSQL** (`apps/nuq-postgres/`) - 数据库服务
- **Playwright服务** (`apps/playwright-service-ts/`) - 浏览器自动化

#### **测试和开发工具**

- **测试网站** (`apps/test-site/`) - Astro框架构建的测试站点
- **测试套件** (`apps/test-suite/`) - 完整的测试框架
- **UI界面** (`apps/ui/`) - 用户界面组件

### 📊 代码统计

- **总文件数**: 约800+个代码文件
- **主要语言**:
  - TypeScript: ~600+ 文件 (API服务和SDK)
  - Python: ~100+ 文件 (Python SDK)
  - Rust: ~20+ 文件 (Rust SDK)
  - Go: ~10+ 文件 (HTML转换服务)
- **测试覆盖**: 完整的单元测试和集成测试

### 🚀 核心功能实现

#### 网页抓取 (Scraping)
- 支持多种输出格式: markdown, HTML, JSON, 截图
- 智能内容提取和清理
- 反机器人机制处理
- 动态内容支持 (JavaScript渲染)

#### 网页爬取 (Crawling)
- 递归爬取所有可访问子页面
- 智能URL发现和过滤
- 深度控制和并发限制
- 进度跟踪和状态管理

#### 数据提取 (Extract)
- LLM驱动的结构化数据提取
- 支持自定义schema和prompt
- 批量处理能力
- 多URL并行处理

#### 搜索功能 (Search)
- 网页内容搜索
- 结果抓取和格式化
- 高级过滤和排序

### 🛠️ 技术栈

- **后端**: Node.js, TypeScript, Express.js
- **数据库**: PostgreSQL, Redis
- **消息队列**: Redis Queue
- **容器化**: Docker, Docker Compose
- **测试**: Jest, 单元测试和集成测试
- **部署**: 支持云部署和自托管

## 使用流程

### 1. 获取API密钥
```bash
# 注册获取API密钥
# https://firecrawl.dev
```

### 2. 安装SDK (以Python为例)
```bash
pip install firecrawl-py
```

### 3. 基本使用

#### 抓取单个页面
```python
from firecrawl import Firecrawl

firecrawl = Firecrawl(api_key="fc-YOUR_API_KEY")

# 抓取页面
doc = firecrawl.scrape(
    "https://firecrawl.dev",
    formats=["markdown", "html"]
)
print(doc.markdown)
```

#### 爬取整个网站
```python
# 爬取网站
response = firecrawl.crawl(
    "https://firecrawl.dev",
    limit=100,
    scrape_options={"formats": ["markdown"]},
    poll_interval=30
)
print(response)
```

#### 提取结构化数据
```python
from pydantic import BaseModel, Field
from typing import List

class Article(BaseModel):
    title: str
    points: int
    by: str
    commentsURL: str

# 使用LLM提取
doc = firecrawl.scrape(
    "https://news.ycombinator.com",
    formats=[{"type": "json", "schema": TopArticles}],
)
print(doc.json)
```

### 4. REST API直接调用

#### 抓取页面
```bash
curl -X POST https://api.firecrawl.dev/v2/scrape \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer fc-YOUR_API_KEY' \
  -d '{
    "url": "https://firecrawl.dev",
    "formats": ["markdown", "html"]
  }'
```

#### 爬取网站
```bash
curl -X POST https://api.firecrawl.dev/v2/crawl \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer fc-YOUR_API_KEY' \
  -d '{
    "url": "https://docs.firecrawl.dev",
    "limit": 10,
    "scrapeOptions": {
      "formats": ["markdown", "html"]
    }
  }'
```

## 本地开发和部署

### 环境要求
- Node.js
- Rust
- pnpm
- Redis
- PostgreSQL
- Docker (可选)

### 本地运行步骤

1. **安装依赖**
```bash
pnpm install
```

2. **设置数据库**
```bash
cd apps/nuq-postgres
docker build -t nuq-postgres .
docker run --name nuqdb \
  -e POSTGRES_PASSWORD=postgres \
  -p 5433:5432 \
  -v nuq-data:/var/lib/postgresql/data \
  -d nuq-postgres
```

3. **配置环境变量**
```bash
cp .env.example .env
# 编辑 .env 文件
```

4. **启动服务**
```bash
pnpm run dev
```

## 开源 vs 云服务

Firecrawl 在 AGPL-3.0 许可证下开源。

为了提供最佳产品，我们在开源产品旁边提供Firecrawl托管版本。云解决方案允许我们持续创新并为所有用户维护高质量的可持续服务。

Firecrawl Cloud 可在 [firecrawl.dev](https://firecrawl.dev) 获取，提供开源版本中不可用的功能范围。

## 贡献

我们热爱贡献！在提交拉取请求之前，请阅读我们的[贡献指南](CONTRIBUTING.md)。如果您想自托管，请参考[自托管指南](SELF_HOST.md)。

_使用Firecrawl进行抓取、搜索和爬取时，最终用户有责任尊重网站的政策。建议用户在使用Firecrawl之前遵守适用的隐私政策和网站使用条款。默认情况下，Firecrawl在爬取时会尊重网站robots.txt文件中指定的指令。通过使用Firecrawl，您明确同意遵守这些条件。_

## 许可证声明

此项目主要在GNU Affero通用公共许可证v3.0 (AGPL-3.0)下授权，如根目录的LICENSE文件中指定。但是，此项目的某些组件在MIT许可证下授权。有关详细信息，请参阅这些特定目录中的LICENSE文件。

请注意：

- AGPL-3.0许可证适用于项目的除非另有指定的所有部分。
- SDK和一些UI组件在MIT许可证下授权。有关详细信息，请参阅这些特定目录中的LICENSE文件。
- 使用或为此项目做贡献时，请确保您遵守您正在处理的特定组件的相应许可证条款。

有关特定组件许可的更多详细信息，请参阅相应目录中的LICENSE文件或联系项目维护者。

---

<p align="right" style="font-size: 14px; color: #555; margin-top: 20px;">
    <a href="#readme-top" style="text-decoration: none; color: #007bff; font-weight: bold;">
        ↑ 返回顶部 ↑
    </a>
</p>
