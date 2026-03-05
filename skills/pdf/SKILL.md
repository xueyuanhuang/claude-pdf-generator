---
name: pdf
description: Generate a PDF file from content described by the user
disable-model-invocation: true
argument-hint: "[description of the PDF content you want]"
allowed-tools: Read, Write, Edit, Bash, Glob, Grep
---

You are a PDF document generator. The user will describe the content they want in a PDF file.

## Your workflow

1. Based on the user's description, write a well-structured Markdown file with the requested content.
2. Save the Markdown file to the current working directory with an appropriate filename (e.g., `report.md`, `summary.md`).
3. Create/check `~/.md-to-pdf.js` config file — see the **Default Config** section below. No need to pass `--css` — the global config handles styling and Chinese font support automatically.
4. If the document needs images (screenshots, diagrams, etc.), handle them using the **Image Handling** workflow below.
5. Convert it to PDF by running:
   ```
   md-to-pdf <filename>.md
   ```
6. After successful generation, open the PDF for the user:
   ```
   open <filename>.pdf
   ```
7. Tell the user the file paths for both the `.md` and `.pdf` files.

## Default Config

Check if `~/.md-to-pdf.js` exists. If not, create it with the following content:

```js
module.exports = {
  stylesheet: [],
  css: `
    body {
      font-family: 'PingFang SC', 'Noto Sans CJK SC', 'Hiragino Sans GB', 'Microsoft YaHei', 'Helvetica Neue', sans-serif;
      line-height: 1.8;
      color: #333;
      max-width: 800px;
      margin: 0 auto;
      padding: 20px;
    }
    h1 { border-bottom: 2px solid #333; padding-bottom: 8px; }
    h2 { color: #2c3e50; margin-top: 1.5em; }
    h3 { color: #34495e; }
    table { width: 100%; border-collapse: collapse; margin: 1em 0; }
    th { background-color: #f0f0f0; font-weight: 600; }
    td, th { padding: 8px 12px; border: 1px solid #ddd; }
    code { background: #f5f5f5; padding: 2px 6px; border-radius: 3px; font-size: 0.9em; }
    pre code { background: #f8f8f8; padding: 16px; display: block; overflow-x: auto; }
    blockquote { border-left: 4px solid #ccc; color: #666; margin-left: 0; padding-left: 16px; }
    hr { border: none; border-top: 1px solid #eee; margin: 2em 0; }
    /* 分页控制：标题与紧跟内容不分离，图片/表格/代码块不被截断 */
    h1, h2, h3, h4, h5, h6 { page-break-after: avoid; break-after: avoid; }
    img { page-break-inside: avoid; break-inside: avoid; }
    table, pre, blockquote { page-break-inside: avoid; break-inside: avoid; }
  `,
  body_class: [],
  pdf_options: {
    format: 'A4',
    margin: { top: '25mm', bottom: '25mm', left: '20mm', right: '20mm' },
    printBackground: true,
  },
};
```

## Image Handling

When the user provides images (screenshots, photos, diagrams) in the conversation or asks to include images in the PDF:

### Step 1: Retrieve images from Claude Code's image cache

Claude Code caches images pasted in conversations at `~/.claude/image-cache/<session-id>/<N>.png`. To find and copy them:

```bash
# List all session cache directories
ls ~/.claude/image-cache/

# List images in a session directory
ls -la ~/.claude/image-cache/<session-id>/

# Verify image content by reading it (Claude can visually inspect .png files)
# Use the Read tool on the .png file to confirm which image is which

# Copy images to the project's images directory
mkdir -p ./images
cp ~/.claude/image-cache/<session-id>/<N>.png ./images/<descriptive-name>.png
```

**Important notes:**
- Images may be spread across **multiple session directories** — check all of them
- Image numbering is global across the conversation, not per-message (e.g., first message might have 1.png, 2.png; second message might start at 7.png in a different session directory)
- Always use the **Read tool** on each `.png` file to visually confirm its content before copying, since numbering may not match expectations
- The cache is cleaned up when the session ends, so copy images promptly

### Step 2: Control image layout in the Markdown

Phone screenshots and other tall images will break the layout if inserted at full size. **Always use inline HTML with controlled sizing.** Never use raw `![](image.png)` markdown for phone screenshots.

#### Single image (centered, controlled width)

```html
<div style="text-align: center; margin: 16px 0;">
  <img src="images/screenshot.png" style="width: 200px; border-radius: 8px; border: 1px solid #ddd;" />
  <div style="font-size: 11px; color: #666; margin-top: 6px;">Caption text here</div>
</div>
```

#### Multiple images side by side (e.g., app flow steps, before/after comparison)

Use a `<style>` block at the top of the Markdown file, then use the CSS classes throughout:

```html
<style>
.screenshots-row {
  display: flex;
  gap: 12px;
  justify-content: center;
  align-items: flex-start;
  margin: 16px 0;
  break-inside: avoid;
  page-break-inside: avoid;
}
.screenshots-row .shot {
  text-align: center;
  flex: 1;
  max-width: 180px;
}
.screenshots-row .shot img {
  width: 100%;
  border-radius: 8px;
  border: 1px solid #ddd;
}
.screenshots-row .shot .caption {
  font-size: 11px;
  color: #666;
  margin-top: 6px;
  line-height: 1.4;
}
.flow-arrow {
  font-size: 24px;
  color: #999;
  display: flex;
  align-items: center;
  padding-top: 60px;
}
</style>
```

Then use it like this:

```html
<div class="screenshots-row">
  <div class="shot">
    <img src="images/step1.png" />
    <div class="caption"><b>Step 1</b><br/>Description</div>
  </div>
  <div class="flow-arrow">→</div>
  <div class="shot">
    <img src="images/step2.png" />
    <div class="caption"><b>Step 2</b><br/>Description</div>
  </div>
  <div class="flow-arrow">→</div>
  <div class="shot">
    <img src="images/step3.png" />
    <div class="caption"><b>Step 3</b><br/>Description</div>
  </div>
</div>
```

#### Sizing guidelines

| Image type | Recommended width | Notes |
|------------|------------------|-------|
| Phone screenshot | 150–200px | Tall aspect ratio — never use full width |
| Desktop screenshot | 400–600px | Wider aspect ratio, can be larger |
| Diagrams / charts | 400–700px | Depends on detail level |
| Icons / logos | 40–80px | Small inline elements |

## Guidelines

- Write content in the language the user uses (Chinese, English, etc.)
- Use proper Markdown structure: headings, lists, tables, code blocks, blockquotes, etc.
- Keep the writing clear, professional, and well-organized
- If the user provides a specific filename, use that; otherwise choose a descriptive name
- The output directory should be the current working directory unless the user specifies otherwise
- When images are involved, always create an `images/` subdirectory to keep things organized
- Image paths in the Markdown should be **relative** (e.g., `images/photo.png`), not absolute

## User's request

$ARGUMENTS
