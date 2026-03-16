# mineru-skill

A [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skill for parsing documents with [MinerU](https://github.com/opendatalab/MinerU) API.

Convert PDF, DOC, DOCX, PPT, PPTX, and images into clean Markdown/JSON — with OCR, formula recognition, table extraction, and batch processing.

## Features

- **Cloud API** — No GPU needed, uses `mineru.net` hosted service
- **Local API** — Self-hosted with `mineru-api` for full control
- **Smart models** — `hybrid` (default), `pipeline`, `vlm`, `MinerU-HTML`
- **Rich extraction** — OCR (109 languages), LaTeX formulas, cross-page tables
- **Batch processing** — Parse up to 200 files per request
- **Extra formats** — Export to DOCX, HTML, or LaTeX alongside Markdown
- **CLI script** — `mineru-parse.sh` for quick command-line usage

## Install

```bash
# Clone into Claude Code skills directory
cd ~/.claude/skills
git clone https://github.com/LeoLin990405/mineru-skill.git mineru
```

## Setup

1. Get an API token at [mineru.net/apiManage/token](https://mineru.net/apiManage/token)
2. Save it locally:

```bash
mkdir -p ~/.config/mineru
echo "YOUR_TOKEN" > ~/.config/mineru/token
chmod 600 ~/.config/mineru/token
```

## Usage

### In Claude Code

Just ask Claude to parse a document:

```
Parse this PDF to markdown: https://arxiv.org/pdf/2301.00001.pdf
```

```
Extract tables from /path/to/report.pdf using vlm model
```

```
Batch parse all PDFs in this folder to markdown
```

Claude will use the skill's API knowledge to handle the request.

### CLI Script

```bash
# Parse from URL
~/.claude/skills/mineru/scripts/mineru-parse.sh https://example.com/doc.pdf

# Parse local file with options
~/.claude/skills/mineru/scripts/mineru-parse.sh paper.pdf \
  --model vlm --ocr --output ./parsed --extract

# Extra output formats
~/.claude/skills/mineru/scripts/mineru-parse.sh report.pdf \
  --format docx --format latex --output ./out

# Specific pages
~/.claude/skills/mineru/scripts/mineru-parse.sh book.pdf \
  --pages "1-20" --output ./chapters
```

### Script Options

| Option | Description |
|--------|-------------|
| `--model <m>` | `hybrid` (default), `pipeline`, `vlm`, `MinerU-HTML` |
| `--ocr` | Enable OCR mode |
| `--no-formula` | Disable formula recognition |
| `--no-table` | Disable table recognition |
| `--output <dir>` | Download results to directory |
| `--extract` | Auto-extract zip and display markdown |
| `--pages <range>` | Page ranges, e.g. `"1-5,8"` |
| `--format <fmt>` | Extra format: `docx`, `html`, `latex` (repeatable) |
| `--callback <url>` | Webhook URL for async notification |
| `--data-id <id>` | Custom tracking identifier |
| `--quiet` | Suppress progress output |

### Environment Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `MINERU_TOKEN_FILE` | `~/.config/mineru/token` | Token file path |
| `MINERU_API_BASE` | `https://mineru.net/api/v4` | API base URL |
| `MINERU_POLL_INTERVAL` | `5` | Poll interval (seconds) |
| `MINERU_MAX_POLL` | `360` | Max poll attempts |

## API Reference

See [SKILL.md](SKILL.md) for complete API documentation including:

- All endpoints (single/batch extraction, file upload)
- Request/response formats
- Model comparison
- Error codes
- Python and curl examples
- Local deployment guide

## Limits (Cloud API)

| Item | Limit |
|------|-------|
| File size | 200MB |
| Pages per file | 600 |
| Daily priority pages | 2,000/account |
| Batch upload | 200 files/request |
| Token validity | 90 days |

## Models

| Model | Best for | Speed |
|-------|----------|-------|
| `hybrid` | General use (default since v2.7.0) | Medium |
| `pipeline` | CPU-only environments | Fast |
| `vlm` | Complex layouts, scanned docs | Slower |
| `MinerU-HTML` | Preserving HTML structure | Medium |

## Requirements

- `curl` and `jq` for the CLI script
- A MinerU API token (free tier available)
- No local GPU required for cloud API

## License

MIT
