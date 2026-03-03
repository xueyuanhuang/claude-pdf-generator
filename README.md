# claude-pdf-generator

A Claude Code plugin that generates beautifully formatted PDF files from natural language descriptions, with full CJK (Chinese, Japanese, Korean) support.

## Features

- Generate PDF from natural language — just describe what you want
- Full CJK font support (Chinese, Japanese, Korean)
- Clean, professional A4 layout
- Supports tables, code blocks, lists, blockquotes, and more
- Works in any directory

## How It Works

This plugin has two parts:

| Component | Role |
|-----------|------|
| **Plugin (SKILL.md)** | Instructs Claude how to generate content and convert it to PDF |
| **md-to-pdf** | The underlying tool that actually renders Markdown into PDF files |

The plugin itself is a set of instructions — the actual PDF rendering is done by `md-to-pdf` (which uses headless Chromium under the hood). Both are required.

## Getting Started

### Step 1: Install the dependency

`md-to-pdf` is the tool that converts Markdown to PDF. Install it globally (one-time setup):

```bash
npm i -g md-to-pdf
```

> Requires Node.js 14+. On first run, it will download Chromium (~300MB).

### Step 2: Install the plugin

Choose one of the following methods:

**Option A: Plugin install (recommended)**

```
/plugin marketplace add xueyuanhuang/claude-pdf-generator
/plugin install claude-pdf-generator@claude-pdf-generator
```

**Option B: Manual install**

```bash
mkdir -p ~/.claude/commands
curl -o ~/.claude/commands/pdf.md \
  https://raw.githubusercontent.com/xueyuanhuang/claude-pdf-generator/main/skills/pdf/SKILL.md
```

### Step 3: Use it

In Claude Code, type `/pdf` followed by a description of what you want:

```
/pdf 帮我写一份 Q1 季度工作总结
```

```
/pdf Write a project status report for Q1 2026
```

```
/pdf Create a technical design document for a REST API
```

```
/pdf 写一份关于用户增长的数据分析报告，包含表格和图表描述
```

Claude will automatically:
1. Generate a well-structured Markdown file
2. Convert it to a styled PDF (A4, with proper CJK fonts)
3. Open the PDF for you to preview

Both the `.md` source and `.pdf` output are saved in your current directory.

## Customization

The plugin automatically creates a global style config at `~/.md-to-pdf.js` on first use. You can edit this file to customize:

- **Fonts** — default is `PingFang SC` (macOS). Change to `Noto Sans CJK SC` for Linux, or `Microsoft YaHei` for Windows
- **Colors** — heading colors, text color, link color
- **Margins** — page margins (default: 25mm top/bottom, 20mm left/right)
- **Line height** — default is 1.8
- **Table styles** — borders, header background, padding
- **Code block styles** — background color, font size

## FAQ

**Q: I get "md-to-pdf: command not found"**
A: Run `npm i -g md-to-pdf` to install the dependency.

**Q: Chinese characters show as blank or garbled**
A: Edit `~/.md-to-pdf.js` and change the font-family to a CJK font available on your system:
- macOS: `PingFang SC`, `Songti SC`, `Hiragino Sans GB`
- Linux: `Noto Sans CJK SC`
- Windows: `Microsoft YaHei`, `SimSun`

**Q: Can I change the page size?**
A: Edit `~/.md-to-pdf.js`, change `format: 'A4'` to `'Letter'`, `'A3'`, etc.

## License

MIT
