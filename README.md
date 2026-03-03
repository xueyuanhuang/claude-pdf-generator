# claude-pdf-generator

A Claude Code plugin that generates beautifully formatted PDF files from natural language descriptions, with full CJK (Chinese, Japanese, Korean) support.

## Features

- Generate PDF from natural language — just describe what you want
- Full CJK font support (Chinese, Japanese, Korean)
- Clean, professional A4 layout
- Supports tables, code blocks, lists, blockquotes, and more
- Works in any directory

## Prerequisites

Install `md-to-pdf` globally:

```bash
npm i -g md-to-pdf
```

## Installation

### As a Plugin (recommended)

```bash
# Add the marketplace
/plugin marketplace add xueyuanhuang/claude-pdf-generator

# Install the plugin
/plugin install claude-pdf-generator@claude-pdf-generator
```

### Manual Installation

Copy the skill file to your Claude Code commands directory:

```bash
mkdir -p ~/.claude/commands
cp skills/pdf/SKILL.md ~/.claude/commands/pdf.md
```

## Usage

After installation, use the `/pdf` command in Claude Code:

```
/pdf Write a project status report for Q1 2026
```

```
/pdf 帮我写一份关于用户增长的数据分析报告
```

```
/pdf Create a technical design document for a REST API
```

Claude will generate a Markdown file, convert it to a styled PDF, and open it for you.

## Customization

The plugin automatically creates a global style config at `~/.md-to-pdf.js` on first use. You can edit this file to customize:

- Fonts
- Colors
- Margins
- Line height
- Table styles
- Code block styles

## License

MIT
