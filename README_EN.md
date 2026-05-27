# arXiv Paper Explainer Skill

Language: [中文](README.md) | English

`paper-master` provides one Claude Code Skill: `arxiv-paper-explainer`. It accepts arXiv paper links, extracts the paper ID, fetches metadata, downloads the PDF, generates a structured summary, and can optionally create an illustration.

## Trigger Examples

```text
explain https://arxiv.org/abs/2502.12345
analyze https://arxiv.org/pdf/2310.08560.pdf
summarize https://arxiv.org/abs/2305.12345
总结 https://arxiv.org/abs/2401.00001
```

## Features

- Recognize arXiv `abs` and `pdf` URLs.
- Fetch title, authors, date, and abstract through the arXiv API.
- Download and parse PDF content.
- Generate a structured Markdown paper summary.
- Optionally generate an illustration.
- Degrade gracefully when PDF download or illustration generation fails.

## Repository Layout

```text
.
├── SKILL.md      # Claude Code Skill definition
└── README.md
```

## Output Files

| File | Contents |
|---|---|
| `{arxiv_id}_summary.md` | Summary, contributions, method, conclusions, limitations |
| `{arxiv_id}_image.png` | Optional illustration |

## Installation

Place `SKILL.md` in a Claude Code Skill directory, for example:

```text
skills/arxiv-paper-explainer/SKILL.md
```

## Workflow

```text
Input arXiv URL
  -> Parse paper ID
  -> Fetch metadata from arXiv API
  -> Download and parse PDF
  -> Generate paper summary with a model
  -> Optionally generate an image
  -> Write Markdown output
```

## APIs and Dependencies

| Service | Purpose | Notes |
|---|---|---|
| arXiv API | Metadata | Free, no key required |
| Text model API | Summarization | `SKILL.md` uses MiniMax as the example provider |
| Image generation API | Illustration | `SKILL.md` uses Nano Banana as the example provider |

Before execution, keep external service API keys in environment variables or local config files. Do not commit secrets.

## Error Handling

| Scenario | Behavior |
|---|---|
| Invalid URL | Try to extract the paper ID, otherwise stop with a message |
| arXiv API timeout | Retry |
| PDF download fails | Generate the summary from metadata only |
| Image generation fails | Skip the image and keep the Markdown summary |
| Network failure | Report retry guidance |

## Limitations

- This repository is primarily a Skill definition, not a complete standalone CLI.
- Long-paper quality depends on whether the PDF has a readable text layer.
- Image generation is optional and does not block the Markdown summary.

## License

MIT
