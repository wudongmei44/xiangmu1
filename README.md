# 📚 RAG智能问答系统

基于本地知识库的检索增强生成（RAG）智能问答系统，利用Ollama本地大模型、LangChain框架和Streamlit开发工具，实现对本地文档的智能问答。

## 环境要求

- **操作系统**: Windows 10/11
- **Python**: 3.8 - 3.11
- **Ollama**: 最新版本（用于运行本地大模型）

## 安装步骤

### 1. 安装 Ollama

1. 访问 [https://ollama.com/download](https://ollama.com/download) 下载 Windows 版本
2. 安装完成后，打开命令行工具，下载所需模型：

```bash
# 下载 deepseek-r1:7b 模型（推荐）
ollama pull deepseek-r1:7b

# 或者下载 qwen2:7b 模型
ollama pull qwen2:7b

# 下载嵌入模型（用于文本向量化）
ollama pull nomic-embed-text
```

3. 验证 Ollama 服务是否正常运行：
```bash
ollama list
```

### 2. 创建 Python 虚拟环境

```bash
# 进入项目目录
cd RAG-QA-System

# 创建虚拟环境
python -m venv venv

# 激活虚拟环境
# Windows:
venv\Scripts\activate
# Linux/Mac:
source venv/bin/activate
```

### 3. 安装依赖包

```bash
pip install -r requirements.txt
```

## 使用说明

### 方式一：Web 界面（推荐）

```bash
# 确保 Ollama 服务已启动
# 确保虚拟环境已激活

streamlit run app.py
```

Web 界面功能：
1. **📤 文档上传**：在侧边栏上传 PDF、DOCX 或 TXT 文件
2. **🔨 知识库构建**：点击"构建知识库"按钮，系统自动解析文档并建立向量索引
3. **💬 问答交互**：在输入框中输入问题，系统基于知识库给出回答
4. **📊 知识库状态**：显示当前知识库中的文档数量和文本块数量
5. **📜 对话历史**：自动保存多轮对话记录

### 方式二：命令行模式

```bash
# 先构建知识库
python build_knowledge_base.py
```

### 方式三：独立可执行文件

```bash
# 使用 PyInstaller 打包
pyinstaller --onefile --add-data "src;src" --add-data "sample_docs;sample_docs" app.py

# 运行生成的 dist/app.exe
```

## 项目结构

```
RAG-QA-System/
├── src/                      # 源代码模块
│   ├── __init__.py
│   ├── document_loader.py    # 文档加载模块
│   ├── text_splitter.py      # 文本分割模块
│   ├── vector_store.py       # 向量数据库模块
│   └── rag_chain.py          # RAG问答链模块
├── sample_docs/              # 示例文档
│   ├── 01_NLP简介.txt
│   ├── 02_词嵌入技术.txt
│   ├── 03_Transformer模型.txt
│   ├── 04_注意力机制.txt
│   ├── 05_BERT模型.txt
│   └── 06_文本分类技术.txt
├── chroma_db/                # Chroma 向量数据库存储目录
├── app.py                    # Streamlit Web 应用
├── build_knowledge_base.py   # 命令行版知识库构建与测试
├── test_ollama.py            # Ollama 连接测试脚本
├── requirements.txt          # Python 依赖包列表
├── .gitignore                # Git 忽略文件
└── README.md                 # 项目说明文档
```

## 关键技术点

### RAG 工作流程

1. **文档加载**：支持 PDF、DOCX、TXT 格式文档的批量读取
2. **文本分割**：使用 `RecursiveCharacterTextSplitter`，chunk_size=1000, chunk_overlap=200
3. **向量化存储**：使用 `nomic-embed-text` 嵌入模型将文本转为向量，存入 Chroma 向量数据库
4. **相似性检索**：用户提问时，检索最相关的 3 个文本块
5. **答案生成**：将检索结果作为上下文，由大模型生成最终答案

### 使用的模型

- **语言模型**: deepseek-r1:7b（通过 Ollama 本地运行）
- **嵌入模型**: nomic-embed-text（通过 Ollama 本地运行）

### 系统提示词

模型回答严格遵循以下规则：
- 基于提供的参考文档回答问题
- 若文档中没有相关信息，明确回答"文档中未找到相关答案"
- 不编造或臆想文档中没有的信息

## 功能特点

- ✅ 支持 PDF、DOCX、TXT 格式文档上传
- ✅ 批量文档处理和向量化存储
- ✅ 智能检索增强生成（RAG）
- ✅ 多轮对话记忆功能
- ✅ 知识库状态实时显示
- ✅ 可打包为独立 exe 文件
- ✅ 完全本地运行，数据安全

## 项目效果截图

### 界面截图1：系统主界面
![主界面](screenshots/main_interface.png)

### 界面截图2：问答示例
![问答示例](screenshots/qa_example.png)

### 界面截图3：知识库管理
![知识库管理](screenshots/kb_management.png)

## 已知问题与改进方向

### 已知问题
1. 首次加载模型需要一定时间
2. 处理大文档时内存占用较高
3. 嵌入模型对中文支持有待优化

### 改进方向
1. 支持更多文档格式（HTML、Markdown、JSON等）
2. 添加文档预览功能
3. 支持多语言文档处理
4. 优化检索策略（如混合检索）
5. 添加用户认证和多用户支持
6. 支持知识库的增量更新
7. 使用更强大的嵌入模型提升检索质量

## 许可证

MIT License