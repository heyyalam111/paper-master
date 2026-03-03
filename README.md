# arxiv-paper-explainer

自动分析 arXiv 论文，生成摘要和配图。

## 功能

- 解析 arXiv URL
- 获取论文元数据
- 下载并解析 PDF
- AI 提取关键内容
- 生成配图
- 输出 Markdown 摘要

## 使用

```bash
explain https://arxiv.org/abs/2502.12345
analyze https://arxiv.org/pdf/2310.08560.pdf
总结 https://arxiv.org/abs/2305.12345
```

## 输出

| 文件 | 说明 |
|------|------|
| `{id}_summary.md` | 论文摘要 |
| `{id}_image.png` | 配图 |

## API 配置

```yaml
arXiv: 免费
MiniMax: 需 API Key
Nano Banana: 需 API Key（待补充）
```

## 错误处理

| 问题 | 处理 |
|------|------|
| PDF 下载失败 | 仅用元数据 |
| 配图失败 | 跳过 |
| 网络错误 | 重试 |

## 安装

复制到 skills 目录即可。
