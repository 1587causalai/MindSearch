# MindSearch 快速部署指南 (基于 GPT API)

本指南将帮助你使用 GPT API 快速部署 MindSearch。这是最简单的部署方式之一,只需要有 GPT API key 即可。

## 1. 环境准备

### 1.1 基础环境要求
- Python 3.8 或更高版本
- Git
- pip 包管理器

### 1.2 克隆项目
```bash
git clone https://github.com/InternLM/MindSearch
cd MindSearch
```

### 1.3 安装依赖
```bash
pip install -r requirements.txt
```

## 2. 配置环境变量

### 2.1 创建配置文件
```bash
cp .env.example .env
```

### 2.2 编辑配置文件
使用你喜欢的编辑器打开 .env 文件,填入以下配置:
```env
# OpenAI API 配置
OPENAI_API_KEY=你的_GPT_API_KEY
OPENAI_API_BASE=https://api.openai.com
OPENAI_API_MODEL=gpt-4-turbo-preview

# 搜索引擎配置
SEARCH_ENGINE=DuckDuckGoSearch

# 如果在国内使用，可能需要配置代理
# HTTPS_PROXY=http://127.0.0.1:7890
# HTTP_PROXY=http://127.0.0.1:7890
```

### 2.3 验证环境变量
在启动服务之前，先验证环境变量是否生效：

```bash
# Linux/Mac
echo $OPENAI_API_KEY
echo $OPENAI_API_BASE

# Windows
echo %OPENAI_API_KEY%
echo %OPENAI_API_BASE%
```

### 2.4 可选：配置代理
如果在国内访问 OpenAI API，很可能需要配置代理。可以通过以下方式：

1. 在 .env 文件中配置（推荐）：
```env
HTTPS_PROXY=http://127.0.0.1:7890
HTTP_PROXY=http://127.0.0.1:7890
```

2. 或在命令行中临时设置：
```bash
# Linux/Mac
export HTTPS_PROXY=http://127.0.0.1:7890
export HTTP_PROXY=http://127.0.0.1:7890

# Windows
set HTTPS_PROXY=http://127.0.0.1:7890
set HTTP_PROXY=http://127.0.0.1:7890
```

## 3. 启动服务

### 3.1 先启动后端
```bash
# 确保环境变量已正确设置
export OPENAI_API_BASE="https://api.openai.com/v1/chat/completions"
export OPENAI_API_MODEL="chatgpt-4o-latest"

# 启动后端服务
python -m mindsearch.app --lang en --model_format gpt4 --search_engine DuckDuckGoSearch --asy
```

### 3.2 验证后端
```bash
# 使用 curl 测试后端 API
curl -X POST "http://localhost:8002/solve" \
     -H "Content-Type: application/json" \
     -d '{"query": "Hello!", "stream": false}'
```

### 3.3 启动前端
```bash
python frontend/mindsearch_gradio.py
```

## 4. 验证部署

1. 打开浏览器访问对应的前端地址
2. 在对话框中输入: "谁是马斯克?"
3. 等待系统返回搜索结果和回答

## 常见问题

1. **如果遇到 API 超时**
   - 检查网络连接
   - 确认 API key 是否正确
   - 可能需要使用代理

2. **如果搜索结果不准确**
   - 可以尝试调整 query 的表述方式
   - 考虑更换其他搜索引擎

3. **如果前端无法连接后端**
   - 确认后端服务是否正常运行
   - 检查端口是否被占用
   - 查看后端日志是否有错误信息

4. **如果遇到 OpenAI API 连接问题**
   - 确认 API Key 是否正确
   - 确认 API Base URL 是否正确配置
   - 检查是否需要配置代理
   - 可以先用 curl 测试 API 是否可访问：
   ```bash
   curl https://api.openai.com/v1/models \
     -H "Authorization: Bearer $OPENAI_API_KEY"
   ```

5. **如果遇到 lagent 警告**
   - 这个警告通常不影响使用
   - 如果想去除警告，可以检查 lagent 的版本是否最新

## 下一步

完成快速部署后,你可以:
1. 尝试使用其他搜索引擎
2. 探索更多高级配置
3. 尝试部署 React 前端获得更好的用户体验

---

📝 备注:
- 本文档面向快速部署场景,省略了一些高级配置
- 如需更详细的配置说明,请参考项目完整文档
- 部署过程中遇到问题可以提 Issue 