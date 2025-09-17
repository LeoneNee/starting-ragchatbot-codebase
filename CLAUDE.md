# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目概述

这是一个基于检索增强生成（RAG）的课程材料问答系统，使用 FastAPI 构建后端，ChromaDB 作为向量存储，Anthropic Claude 作为 AI 生成器。

## 核心架构

### 主要组件
- **RAGSystem** (`backend/rag_system.py`): 核心协调器，管理文档处理、向量存储和AI生成
- **FastAPI 应用** (`backend/app.py`): 提供 Web API 和前端静态文件服务
- **DocumentProcessor**: 处理课程文档并分割成向量存储的块
- **VectorStore**: 基于 ChromaDB 的向量存储和语义搜索
- **AIGenerator**: 使用 Anthropic Claude 生成响应
- **SessionManager**: 管理对话历史记录

### 目录结构
```
├── backend/           # Python 后端代码
│   ├── app.py        # FastAPI 主应用
│   ├── rag_system.py # RAG 系统核心
│   ├── config.py     # 配置管理
│   └── ...
├── frontend/         # Web 前端文件
│   ├── index.html    # 主页面
│   ├── script.js     # 前端逻辑
│   └── style.css     # 样式
├── docs/            # 课程材料文档
└── run.sh           # 启动脚本
```

## 开发命令

### 环境设置
```bash
# 安装依赖
uv sync

# 设置环境变量（创建 .env 文件）
cp .env.example .env
# 编辑 .env 添加 ANTHROPIC_API_KEY
```

### 运行应用
```bash
# 使用提供的启动脚本（推荐）
chmod +x run.sh
./run.sh

# 或手动启动
cd backend
uv run uvicorn app:app --reload --port 8000
```

### 应用访问
- Web 界面: http://localhost:8000
- API 文档: http://localhost:8000/docs

## 配置说明

系统配置在 `backend/config.py` 中管理：
- `CHUNK_SIZE`: 800 - 文档分块大小
- `MAX_RESULTS`: 5 - 搜索返回的最大结果数
- `MAX_HISTORY`: 2 - 保存的对话历史记录数
- `CHROMA_PATH`: "./chroma_db" - 向量数据库存储位置

## 数据处理

系统支持的课程材料格式：
- PDF (.pdf)
- Word 文档 (.docx)
- 纯文本 (.txt)

课程材料存放在 `docs/` 目录中，系统启动时会自动加载和索引这些文档。

## API 端点

- `POST /api/query`: 处理用户查询，返回 AI 响应和来源
- `GET /api/courses`: 获取课程统计信息

## 注意事项

- 系统使用 uv 包管理器，不要使用 pip
- 需要有效的 Anthropic API 密钥
- Windows 用户建议使用 Git Bash 运行命令