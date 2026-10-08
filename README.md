# Scholar RAG

基于检索增强生成（RAG）技术的学术论文问答系统。

![Demo](doc/demo.gif)

## 功能特点

- 📚 支持学术论文的上传和解析（PDF格式）
- 🔍 基于向量检索的智能问答
- 🤖 集成 LangGraph 的 Agent 对话能力
- 🎨 现代化的 Web 界面
- 💾 支持多会话管理
- 🖼️ 可选的视觉语言模型（VLM）支持
- 📊 内置检索和生成评估工具

## 技术架构

- **后端**: FastAPI + LangChain + LangGraph
- **前端**: React + Vite + TailwindCSS
- **向量数据库**: Milvus
- **关系数据库**: PostgreSQL
- **LLM**: 支持 OpenAI API 兼容接口（默认 Ollama）
- **文档解析**: Docling + PyMuPDF

![Architecture](doc/architecture.png)

## 快速开始

### 使用 Docker Compose（推荐）

1. 克隆项目并配置环境变量：
```bash
cd backend
cp .env.example .env
# 编辑 .env 文件配置你的 LLM API
```

2. 启动所有服务：
```bash
make docker-up
```

3. 访问应用：
- 前端界面: http://localhost:5173
- 后端 API: http://localhost:8000
- API 文档: http://localhost:8000/docs

### 本地开发

#### 前置要求

- Python 3.10+
- Node.js 18+
- PostgreSQL
- Milvus（或使用 Milvus Lite）

#### 安装依赖

```bash
make install
```

#### 配置环境变量

在 `backend/.env` 中配置：

```env
# LLM 配置
LLM_BASE_URL=http://localhost:11434/v1
LLM_MODEL=qwen3:32b
LLM_API_KEY=ollama

# 数据库配置
POSTGRES_URI=postgresql://postgres:postgres@localhost:5432/scholar_rag
MILVUS_URI=./data/milvus.db

# 可选：VLM 配置
VLM_ENABLED=false
VLM_BASE_URL=http://localhost:11434/v1
VLM_MODEL=qwen-vl
```

#### 启动开发服务器

启动后端和前端（同时运行）：
```bash
make dev
```

或分别启动：
```bash
# 仅后端
make backend

# 仅前端
make frontend
```

## 使用说明

1. **上传文档**: 点击上传按钮，选择 PDF 格式的学术论文
2. **等待处理**: 系统会自动解析并向量化文档内容
3. **开始对话**: 在输入框中提出关于论文的问题
4. **查看引用**: 回答会附带相关的文档引用信息

## 开发命令

```bash
make help          # 查看所有可用命令
make install       # 安装依赖
make start         # 构建并启动生产环境
make dev           # 启动开发环境
make build         # 构建前端
make test          # 运行测试
make lint          # 代码检查
make clean         # 清理构建产物
make docker-up     # 启动 Docker 服务
make docker-down   # 停止 Docker 服务
```

## 项目结构

```
scholar-rag/
├── backend/           # FastAPI 后端
│   ├── agent/        # LangGraph Agent 定义
│   ├── app/          # FastAPI 应用
│   ├── rag/          # RAG 核心逻辑
│   ├── eval/         # 评估工具
│   └── test/         # 测试文件
├── frontend/         # React 前端
├── doc/              # 文档和资源
└── docker-compose.yml
```

## 配置说明

主要配置项位于 `backend/.env`：

| 配置项 | 说明 | 默认值 |
|--------|------|--------|
| `LLM_BASE_URL` | LLM API 地址 | `http://localhost:11434/v1` |
| `LLM_MODEL` | LLM 模型名称 | `qwen3:32b` |
| `EMBEDDING_MODEL` | 向量模型 | `BAAI/bge-small-en-v1.5` |
| `RERANKER_MODEL` | 重排序模型 | `BAAI/bge-reranker-v2-m3` |
| `TOP_K` | 检索返回数量 | `5` |
| `ENABLE_CACHE` | 启用缓存 | `true` |
| `VLM_ENABLED` | 启用视觉模型 | `false` |

## 评估

运行检索评估：
```bash
cd backend
python -m eval.eval_retrieval
```

运行生成评估：
```bash
cd backend
python -m eval.eval_generation
```

## License

本项目基于 MIT 协议开源。详见 [LICENSE](LICENSE) 文件。

## 贡献

欢迎提交 Issue 和 Pull Request！

---

如有问题，请在 GitHub Issues 中反馈。
