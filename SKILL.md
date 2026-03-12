---
name: markitdown
description: 文档转Markdown转换工具(markitdown)。当用户需要将各种文件格式转换为Markdown(PDF转Markdown/Word转Markdown/Excel转Markdown/PPT转Markdown/图片转Markdown/音频转Markdown/HTML转Markdown/CSV转Markdown/JSON转Markdown/XML转Markdown/ZIP转Markdown/YouTube视频转Markdown/EPub电子书转Markdown)、使用Azure Document Intelligence进行OCR识别、使用LLM生成图片描述、通过CLI命令行或Python API进行文档转换、处理可选依赖插件等相关任务时立即使用。支持pip install 'markitdown[all]'安装，Python >= 3.10。
trigger:
  - 转Markdown
  - PDF转Markdown
  - Word转Markdown
  - Excel转Markdown
  - 文档转换
  - markitdown
  - 文件转MD
  - PPT转Markdown
  - 图片转文字
  - 视频转文字
---

# MarkItDown Skill

> 当前版本: 0.1.5 | Python >= 3.10

## Quick Start

### Installation

```bash
pip install 'markitdown[all]'  # All features (recommended)
# Or install only what you need:
pip install 'markitdown[pdf,docx,pptx,xlsx]'  # Common formats
```

### CLI Usage

```bash
# Convert file to stdout
markitdown path-to-file.pdf > document.md

# Specify output file
markitdown path-to-file.pdf -o document.md

# Pipe content
cat path-to-file.pdf | markitdown

# List installed plugins
markitdown --list-plugins

# Enable plugins
markitdown --use-plugins path-to-file.pdf

# Use Azure Document Intelligence
markitdown path-to-file.pdf -o document.md -d -e "<document_intelligence_endpoint>"

# Version check
markitdown --version
```

### Python API

```python
from markitdown import MarkItDown

# Basic usage
md = MarkItDown()
result = md.convert("test.xlsx")
print(result.text_content)

# Enable plugins
md = MarkItDown(enable_plugins=True)
result = md.convert("document.pdf")

# With Azure Document Intelligence
md = MarkItDown(docintel_endpoint="<endpoint>")
result = md.convert("document.pdf")

# With LLM for image descriptions (OpenAI)
from openai import OpenAI
client = OpenAI()
md = MarkItDown(llm_client=client, llm_model="gpt-4o")
result = md.convert("example.jpg")
print(result.text_content)

# Streaming API (binary mode required)
import io
with open("document.pdf", "rb") as f:
    stream = io.BytesIO(f.read())
    result = md.convert_stream(stream)
    print(result.text_content)
```

## Supported Formats

| Format | Install Command | Notes |
|--------|-----------------|-------|
| PDF | `[pdf]` | Text extraction, table support |
| PowerPoint | `[pptx]` | Slides, notes, images |
| Word | `[docx]` | Document text, tables |
| Excel | `[xlsx]` | Spreadsheet data |
| Old Excel | `[xls]` | Legacy format |
| Images | Built-in | EXIF + OCR |
| Audio | `[audio-transcription]` | EXIF + speech transcription |
| YouTube | `[youtube-transcription]` | Video transcription |
| HTML | Built-in | Web page conversion |
| CSV/JSON/XML | Built-in | Text-based formats |
| ZIP | Built-in | Iterates contents |
| EPub | Built-in | Ebook conversion |
| Outlook | `[outlook]` | Email messages |
| Azure Doc Intel | `[az-doc-intel]` | Cloud OCR |

## Optional Dependencies

```bash
pip install 'markitdown[all]'           # Everything
pip install 'markitdown[pptx]'           # PowerPoint only
pip install 'markitdown[docx]'           # Word only
pip install 'markitdown[xlsx]'           # Excel only
pip install 'markitdown[pdf]'            # PDF only
pip install 'markitdown[xls]'            # Old Excel
pip install 'markitdown[outlook]'        # Outlook emails
pip install 'markitdown[audio-transcription]'  # Audio transcription
pip install 'markitdown[youtube-transcription]' # YouTube transcription
pip install 'markitdown[az-doc-intel]'  # Azure Document Intelligence
```

## Plugins

### Using Plugins

```bash
# List installed plugins
markitdown --list-plugins

# Enable plugins for conversion
markitdown --use-plugins path-to-file.pdf
```

### Developing Plugins

See `packages/markitdown-sample-plugin` in the MarkItDown repository for plugin development guide.

## Azure Document Intelligence

For complex PDF layouts, use Azure Document Intelligence:

```bash
markitdown document.pdf -o output.md -d -e "<endpoint>"
```

Python API:
```python
md = MarkItDown(docintel_endpoint="your-endpoint")
result = md.convert("document.pdf")
```

Setup guide: https://learn.microsoft.com/en-us/azure/ai-services/document-intelligence/how-to-guides/create-document-intelligence-resource

## LLM Image Descriptions

For richer image descriptions, provide an LLM client:

```python
from markitdown import MarkItDown
from openai import OpenAI

client = OpenAI()
md = MarkItDown(llm_client=client, llm_model="gpt-4o")
result = md.convert("image.jpg")
```

## Streaming API

```python
import io
from markitdown import MarkItDown

md = MarkItDown()

# Convert from bytes stream
with open("document.pdf", "rb") as f:
    stream = io.BytesIO(f.read())
    result = md.convert_stream(stream)
    print(result.text_content)
```

Note: `convert_stream()` requires a binary file-like object (not text mode like io.StringIO).

## Troubleshooting

### Common Issues

**"No converter found"**: Install the optional dependency for that format (e.g., `pip install 'markitdown[pdf]'`)

**OCR fails on images**: Ensure Tesseract is installed or use LLM integration for image descriptions

**Azure Document Intelligence errors**: Verify endpoint URL and authentication credentials

**Plugin not loading**: Use `--use-plugins` flag or set `enable_plugins=True` in Python

**Streaming error**: Use `io.BytesIO()` (binary mode), NOT `io.StringIO()` (text mode)

### Installation Check

```bash
markitdown --help  # Verify CLI is installed
pip show markitdown  # Check installed version
markitdown --version  # Show version
```

## Docker Usage

```bash
# Build image
docker build -t markitdown:latest .

# Convert file using Docker
docker run --rm -i markitdown:latest < input.pdf > output.md
docker run --rm -i markitdown:latest input.pdf -o output.md
```

## Use Cases

### LLM Data Preparation
```python
# Convert documents for LLM ingestion
from markitdown import MarkItDown

md = MarkItDown()
files = ["report.pdf", "notes.docx", "data.xlsx"]

for f in files:
    result = md.convert(f)
    print(f"=== {f} ===")
    print(result.text_content[:500])  # Preview
```

### Batch Processing
```python
import os
from markitdown import MarkItDown

md = MarkItDown()
output_dir = "markdown_output"
os.makedirs(output_dir, exist_ok=True)

for filename in os.listdir("."):
    if filename.endswith((".pdf", ".docx", ".pptx")):
        result = md.convert(filename)
        output_path = os.path.join(output_dir, filename.rsplit(".", 1)[0] + ".md")
        with open(output_path, "w", encoding="utf-8") as f:
            f.write(result.text_content)
```
