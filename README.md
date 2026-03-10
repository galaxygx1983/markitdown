# MarkItDown Skill

> Lightweight Python utility for converting files to Markdown

## 支持格式

| 类型 | 格式 |
|------|------|
| 文档 | PDF, Word (.docx), PowerPoint (.pptx), Excel (.xlsx) |
| 图片 | JPEG, PNG, WebP, GIF |
| 音频 | MP3, WAV, FLAC |
| 数据 | CSV, JSON, XML |
| 其他 | HTML, ZIP, YouTube 视频, EPub 电子书 |

## 安装

```bash
# 完整安装（推荐）
pip install 'markitdown[all]'

# 按需安装
pip install 'markitdown[pdf,docx,pptx,xlsx]'
```

## CLI 使用

```bash
# 转换文件
markitdown document.pdf > output.md

# 转换 URL
markitdown https://example.com/page.html

# 批量处理
markitdown *.pdf > all_docs.md
```

## Python API

```python
from markitdown import MarkItDown

md = MarkItDown()
result = md.convert("document.pdf")
print(result.text_content)
```

## 详细文档

查看 [SKILL.md](SKILL.md) 获取完整使用指南。

## 许可证

MIT License