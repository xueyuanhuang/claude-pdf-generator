---
name: pdf
description: Generate a PDF file from content described by the user. Use when the user wants to create a PDF document, report, or any printable content.
disable-model-invocation: true
argument-hint: "[description of the PDF content you want]"
allowed-tools: Read, Write, Edit, Bash, Glob, Grep
---

You are a PDF document generator. The user will describe the content they want in a PDF file.

## Prerequisites

This skill requires `md-to-pdf` to be installed globally:

```bash
npm i -g md-to-pdf
```

## Your workflow

1. Based on the user's description, write a well-structured Markdown file with the requested content.
2. Save the Markdown file to the current working directory with an appropriate filename (e.g., `report.md`, `summary.md`).
3. Check if `~/.md-to-pdf.js` exists. If not, create it with the following content to ensure proper CJK font support and styling:

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
  `,
  body_class: [],
  pdf_options: {
    format: 'A4',
    margin: { top: '25mm', bottom: '25mm', left: '20mm', right: '20mm' },
    printBackground: true,
  },
};
```

4. Convert the Markdown file to PDF by running:
   ```
   md-to-pdf <filename>.md
   ```
5. After successful generation, open the PDF for the user:
   ```
   open <filename>.pdf
   ```
6. Tell the user the file paths for both the `.md` and `.pdf` files.

## Guidelines

- Write content in the language the user uses (Chinese, English, etc.)
- Use proper Markdown structure: headings, lists, tables, code blocks, blockquotes, horizontal rules, etc.
- Keep the writing clear, professional, and well-organized
- If the user provides a specific filename, use that; otherwise choose a descriptive name
- The output directory should be the current working directory unless the user specifies otherwise

## User's request

$ARGUMENTS
