# RAG Challenge 冠军方案

**进一步了解本项目：**
- 俄语：https://habr.com/ru/articles/893356/
- 英语：https://abdullin.com/ilya/how-to-build-best-rag/

本仓库包含 RAG Challenge 比赛两个奖项类别的获奖方案。该系统结合以下技术，在回答公司年度报告相关问题上取得了领先结果：

- 使用 Docling 进行自定义 PDF 解析
- 基于父文档检索的向量搜索
- 使用 LLM 重排序以提升上下文相关性
- 结合思维链推理的结构化输出提示
- 面向多公司对比的查询路由

## 免责声明

这是竞赛代码，虽不够精致，但能够运行。在开始前请注意：

- IBM Watson 集成无法使用（它是竞赛专用的）
- 代码可能有不够完善之处及特殊的变通方案
- 没有测试，错误处理也很有限，请知悉
- 你需要自行准备 OpenAI/Gemini 的 API 密钥
- GPU 能显著加速 PDF 解析（作者使用的是 4090）

如果你在寻找可直接用于生产环境的代码，本项目并不适合；但若想探索不同的 RAG 技术及其实现方式，欢迎查看。

## 快速开始

克隆并配置项目：
```bash
git clone https://github.com/IlyaRice/RAG-Challenge-2.git
cd RAG-Challenge-2
python -m venv venv
venv\Scripts\Activate.ps1  # Windows (PowerShell)
pip install -e . -r requirements.txt
```

将 `env` 重命名为 `.env`，并填写你的 API 密钥。

## 测试数据集

本仓库包含两个数据集：

1. 小型测试集（位于 `data/test_set/`），包含 5 份年度报告及问题
2. 完整的 ERC2 比赛数据集（位于 `data/erc2_set/`），包含全部比赛问题和报告

每个数据集目录都包含独立的 README，其中说明了具体的配置方法和可用文件。你可以使用任一数据集来：

- 学习示例问题、报告和系统输出
- 基于提供的 PDF 从头运行流水线
- 使用预处理数据，直接跳至特定流水线阶段

详细的数据集内容和配置说明请参阅对应 README：
- `data/test_set/README.md`：小型测试数据集
- `data/erc2_set/README.md`：完整比赛数据集

## 使用方法

在 `src/pipeline.py` 中取消注释你要运行的方法，然后执行以下命令，即可运行流水线的任意部分：
```bash
python .\src\pipeline.py
```

你也可以通过 `main.py` 运行任意流水线阶段，但需要在数据所在目录中执行：
```bash
cd .\data\test_set\
python ..\..\main.py process-questions --config max_nst_o3m
```

### CLI 命令

查看可用命令的帮助信息：
```bash
python main.py --help
```

可用命令：
- `download-models`：下载所需的 docling 模型
- `parse-pdfs`：解析 PDF 报告，支持并行处理选项
- `serialize-tables`：处理已解析报告中的表格
- `process-reports`：对已解析报告运行完整流水线
- `process-questions`：使用指定配置处理问题

每条命令都有各自的选项。例如：
```bash
python main.py parse-pdfs --help
# 显示 --parallel/--sequential、--chunk-size、--max-workers 等选项

python main.py process-reports --config ser_tab
# 使用表格序列化配置处理报告
```

## 部分配置

- `max_nst_o3m`：使用 OpenAI o3-mini 模型的最佳表现配置
- `ibm_llama70b`：使用 IBM Llama 70B 模型的替代配置
- `gemini_thinking`：利用 Gemini 巨大的上下文窗口进行完整上下文问答；它实际上并非 RAG

更多配置及其详情请查看 `pipeline.py`。

## 许可证

MIT