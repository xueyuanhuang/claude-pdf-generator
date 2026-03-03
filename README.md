# claude-pdf-generator

A Claude Code plugin that generates beautifully formatted PDF files from natural language descriptions, with full CJK (Chinese, Japanese, Korean) support and **image embedding** capabilities.

## Features

- Generate PDF from natural language — just describe what you want
- Full CJK font support (Chinese, Japanese, Korean)
- **Embed images** — paste screenshots in the conversation, and they'll be included in the PDF
- Smart image layout — phone screenshots displayed side-by-side at proper sizes
- Clean, professional A4 layout
- Supports tables, code blocks, lists, blockquotes, and more

## How It Works

| Component | Role |
|-----------|------|
| **Plugin (SKILL.md)** | Instructs Claude how to generate content, handle images, and convert to PDF |
| **md-to-pdf** | The underlying tool that renders Markdown + HTML into PDF files |

The plugin itself is a set of instructions — the actual PDF rendering is done by `md-to-pdf` (which uses headless Chromium under the hood). Both are required.

## Getting Started

### Step 1: Install the dependency

`md-to-pdf` is the tool that converts Markdown to PDF. Install it globally (one-time setup):

```bash
npm i -g md-to-pdf
```

> Requires Node.js 14+. On first run, it will download Chromium (~300MB).

### Step 2: Install the plugin

**Option A: Slash command install (recommended)**

```
/install-github-skill xueyuanhuang/claude-pdf-generator
```

**Option B: Manual install**

```bash
mkdir -p ~/.claude/commands
curl -o ~/.claude/commands/pdf.md \
  https://raw.githubusercontent.com/xueyuanhuang/claude-pdf-generator/main/skills/pdf/SKILL.md
```

### Step 3: Use it

In Claude Code, type `/pdf` followed by a description:

```
/pdf 帮我写一份 Q1 季度工作总结
```

```
/pdf Write a project status report for Q1 2026
```

```
/pdf Create a technical design document for a REST API
```

## Image Support

You can paste screenshots or images directly into the Claude Code conversation, and the plugin will embed them in the PDF with proper layout.

### How it works

1. When you paste images in Claude Code, they are cached locally at `~/.claude/image-cache/`
2. The plugin finds these cached images, copies them to your project's `images/` directory
3. Images are embedded in the PDF with controlled sizing and layout

### Example: App screenshot comparison

Paste your screenshots into the conversation, then:

```
/pdf 把这些截图整理成一份竞品分析 PDF
```

The plugin will:
- Retrieve screenshots from the image cache
- Display phone screenshots at 180px width (not full-page)
- Arrange multiple screenshots side-by-side with captions
- Add flow arrows (→) between sequential steps

### Image layout features

| Layout | Use case |
|--------|----------|
| **Side-by-side with arrows** | App flow steps (e.g., 4-step process) |
| **Side-by-side comparison** | Before/after, competitor vs. your app |
| **Single centered** | Highlighting one specific screen |
| **Controlled sizing** | Phone screenshots at 150-200px, desktop at 400-600px |

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

**Q: Images are too large / break the layout**
A: The plugin automatically sizes phone screenshots to 180px width. If you need to adjust, edit the `<style>` block in the generated `.md` file and change `max-width` values.

**Q: Images from the conversation aren't found**
A: Images are cached at `~/.claude/image-cache/` only during the active session. If the session ended, the cache may have been cleaned up. Paste the images again in a new message.

## License

MIT
