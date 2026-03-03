---
name: arxiv-paper-explainer
description: 自动分析 arXiv 论文，提取元数据、生成摘要和配图。
author: Claude
license: MIT
version: 1.0.0
---

# arxiv-paper-explainer

智能 arXiv 论文分析工具。

## 静默执行协议

- 无确认提示
- 自动错误恢复
- 结果自动保存

## 触发词

`explain`、`analyze`、`summarize`、`paper` + arXiv URL

**示例：**
- `explain https://arxiv.org/abs/2502.12345`
- `analyze https://arxiv.org/pdf/2310.08560.pdf`
- `summarize https://arxiv.org/abs/2305.12345`
- `总结 https://arxiv.org/abs/2401.00001`

## 工作流程

```
输入 arXiv URL
    ↓
1. URL 解析 → 提取论文 ID
    ↓
2. arXiv API 获取元数据
    ↓
3. 下载并解析 PDF
    ↓
4. MiniMax API 提取总结内容
    ↓
5. Nano Banana API 生成配图
    ↓
6. 格式化输出 Markdown
```

## API 配置

| 服务 | 端点 | 用途 |
|------|------|------|
| arXiv API | `http://export.arxiv.org/api/query` | 获取论文元数据 |
| MiniMax API | `https://api.minimax.chat/v1/text/chatcompletion_v2` | 内容提取与总结 |
| Nano Banana API | `https://nanobananana.com/api/v1/images/generate` | 配图生成 |

## 输入规格

| 字段 | 类型 | 说明 |
|------|------|------|
| `url` | string | arXiv URL (abs/pdf) |
| `command` | string | explain/analyze/summarize/paper |
| `language` | string | 输出语言，默认 `zh` |
| `save_path` | string | 保存路径 |

## 输出文件

| 文件 | 内容 |
|------|------|
| `{id}_summary.md` | 论文摘要 |
| `{id}_image.png` | 配图 |

## 输出模板

```markdown
# {论文标题}

**作者:** {作者} | **日期:** {日期} | **arXiv:** {ID}

---

## 摘要

{摘要}

## 配图

![配图]({id}_image.png)

## 核心贡献

- {贡献点1}
- {贡献点2}
- {贡献点3}

## 方法要点

{方法简述}

## 主要结论

1. {结论1}
2. {结论2}

## 局限与展望

{简要说明}
```

## 错误处理

| 错误 | 处理 |
|------|------|
| URL 无效 | 提取 ID，重试 |
| API 超时 | 重试 3 次 |
| PDF 下载失败 | 仅用元数据 |
| Nano Banana 失败 | 跳过配图 |
| 网络错误 | 提示重试 |

## 写作风格

**简洁概括风格：**
- 用最少的字传达最多的信息
- 去除冗余表达
- 关键信息前置
- 列表精炼

## 待补充

```yaml
# MiniMax API 配置
minimax:
  api_key: "YOUR_MINIMAX_API_KEY"
  endpoint: "https://api.minimax.chat/v1/text/chatcompletion_v2"
  model: "abab6.5s-chat"

# Nano Banana API 配置
nano_banana:
  api_key: "YOUR_NANO_BANANA_API_KEY"
  endpoint: "https://nanobananana.com/api/v1/images/generate"
  style: "academic"  # 配图风格
```

## 示例

```
用户: explain https://arxiv.org/abs/2305.12345

系统:
[1/6] 解析 URL → ID: 2305.12345
[2/6] 获取元数据
[3/6] 下载 PDF
[4/6] MiniMax 提取内容
[5/6] Nano Banana 生成配图
[6/6] 格式化输出
---
✓ 已保存: 2305.12345_summary.md
✓ 已保存: 2305.12345_image.png
```
