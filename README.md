# arXiv 论文分析 Skill

语言：中文 | [English](README_EN.md)

`paper-master` 提供一个 Claude Code Skill：`arxiv-paper-explainer`。它面向 arXiv 论文链接，自动解析论文 ID、获取元数据、下载 PDF、生成摘要，并可按配置生成配图。

## 触发示例

```text
explain https://arxiv.org/abs/2502.12345
analyze https://arxiv.org/pdf/2310.08560.pdf
summarize https://arxiv.org/abs/2305.12345
总结 https://arxiv.org/abs/2401.00001
```

## 核心能力

- 识别 arXiv `abs` 和 `pdf` 链接。
- 通过 arXiv API 获取标题、作者、日期、摘要等元数据。
- 下载并解析 PDF 内容。
- 生成结构化 Markdown 摘要。
- 可选生成论文配图。
- 在 PDF 下载或配图失败时自动降级。

## 仓库结构

```text
.
├── SKILL.md      # Claude Code Skill 定义
└── README.md
```

## 输出文件

| 文件 | 内容 |
|---|---|
| `{arxiv_id}_summary.md` | 论文摘要、贡献点、方法、结论、局限 |
| `{arxiv_id}_image.png` | 可选配图 |

## 安装

将 `SKILL.md` 放入 Claude Code 可发现的 Skill 目录，例如：

```text
skills/arxiv-paper-explainer/SKILL.md
```

## 工作流程

```text
输入 arXiv URL
  -> 解析论文 ID
  -> arXiv API 获取元数据
  -> 下载并解析 PDF
  -> 模型生成论文摘要
  -> 可选生成配图
  -> 输出 Markdown 文件
```

## API 与依赖

| 服务 | 用途 | 说明 |
|---|---|---|
| arXiv API | 元数据获取 | 免费，无需密钥 |
| 文本模型 API | 论文总结 | `SKILL.md` 中按 MiniMax 示例描述 |
| 图像生成 API | 论文配图 | `SKILL.md` 中按 Nano Banana 示例描述 |

实际执行前，请把外部服务的 API Key 放在环境变量或本地配置中，不要提交到仓库。

## 错误处理

| 场景 | 处理 |
|---|---|
| URL 无效 | 尝试提取论文 ID，失败则提示 |
| arXiv API 超时 | 自动重试 |
| PDF 下载失败 | 仅使用元数据生成摘要 |
| 配图失败 | 跳过配图，保留摘要 |
| 网络失败 | 给出重试建议 |

## 限制

- 当前仓库主要是 Skill 定义，不包含完整的独立 CLI 实现。
- 长论文的解析质量取决于可读取的 PDF 文本层。
- 图像生成是可选步骤，失败不影响 Markdown 摘要。

## 许可证

MIT
