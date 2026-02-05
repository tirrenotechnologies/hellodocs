# hellodocs

Embedded project documentation.

A single cross-platform AWK script that converts Markdown to raw HTML — no dependencies, no build step. Drop the output into your project, Docker image, or repo.

```
┌───────────┐                          ┌────────────┐                  ┌─────┬─────────┐
│ README.md │                          │ /docs      │                  │ nav │ hello   │
│ ───────── │                          │ index.html │                  │ ... │─d─o─c─s─│
│ # Usage   │ ──▶  $ awk hellodocs ──▶ │ 1.html     │ ──▶ browser ──▶  │ ... │ ·· ···· │
│ # API     │                          │ 2.html     │                  │ ··· │ ·· · ·· │
│ # Security│                          │ 3.html     │                  │ ··· │ · · ··· │
└───────────┘                          └────────────┘                  └─────┴─────────┘
```

**One command to HTML docs**
```bash
awk -v title='Hello, Docs!' -v source='https://github.com/user/repo' -f hellodocs.awk README.md
```

**Comparison**

| | hellodocs | MkDocs | Sphinx | Docusaurus |
|---|---|---|---|---|
| Input | Markdown | Markdown | RST | MDX |
| Dependencies | None | Python | Python | Node.js |
| Setup skills | None | YAML | RST | React |
| Size | 1 file | 50+ files | 100+ files | 1000+ files |
| Self-contained | Yes | No | No | No |
| Offline | Yes | Partial | Partial | No |

## Features

HTML docs that will live within your application forever.

* **Zero dependencies** - just AWK, built into every *NIX system
* **Single file**
* **Cross-platform** - Linux, macOS, BSD, Windows (WSL)
* **Offline-first**
* **No CSS, JS, IMG**
* **Frameset** navigation
* **Contenteditable** code blocks for easy code copying
* **Auto-nav** from `##` and `###` headers
* **MD tables, lists, code blocks** support

## Live demo

| Git | hellodocs |
|-----|-----------|
| [tirreno ADMIN.md](https://github.com/tirrenotechnologies/ADMIN.md) | [tirreno.com/admin](https://www.tirreno.com/admin/) |
| [tirreno DEVELOPMENT.md](https://github.com/tirrenotechnologies/DEVELOPMENT.md) | [tirreno.com/devs](https://www.tirreno.com/devs/) |

## System requirements

AWK is a scripting language designed for text processing and data extraction, developed in 1977 and embedded in most Linux distributions. It natively runs in GitHub Actions and Docker without any additional installation steps.

hellodocs is known to work on many POSIX-compliant systems, including but not limited to:
* GNU/Linux
* macOS
* *BSD
* Android (through Termux)
* Windows (through WSL, Cygwin, or MSYS2)

Browsers:
* Chrome 20+
* Firefox 15+
* Safari 6+
* Edge (all versions)
* Internet Explorer 10+
* Opera 12+

## Installation

You can embed hellodocs directly into your software by downloading or cloning it, or follow the [Integration](#integration) steps to include it into your workflow.

### Direct download

```bash
curl -O https://raw.githubusercontent.com/tirrenotechnologies/hellodocs/main/hellodocs.awk
```

Or using wget:
```bash
wget https://raw.githubusercontent.com/tirrenotechnologies/hellodocs/main/hellodocs.awk
```

### From Git

```bash
git clone https://github.com/tirrenotechnologies/hellodocs.git
cd hellodocs
```

Or [download ZIP](https://github.com/tirrenotechnologies/hellodocs/archive/refs/heads/main.zip) and extract.

## Usage

Run this command to generate your HTML docs.

```bash
awk -v title='Hello, Docs!' -v source='https://github.com/user/repo' -f hellodocs.awk README.md
```

### Markdown syntax

| Element | Syntax | Description |
|---------|--------|-------------|
| Heading 1 | # Title | Document title, used for intro section |
| Heading 2 | ## Section | Main sections, creates new page |
| Heading 3 | ### Subsection | Subsections within page |
| Heading 4 | #### Sub-subsection | Sub-subsections within page |
| Bold | two asterisks around text | Bold text |
| Italic | single asterisk around text | Italic text |
| Inline code | backticks around code | Inline code with contenteditable |
| Code block | triple backticks on own line | Fenced code block |
| Link | bracket text paren url | Hyperlink, external opens in new tab |
| Unordered list | asterisk or dash + space | Bullet list |
| Ordered list | number + period + space | Numbered list |
| Nested list | 2-4 spaces then dash | Indented sub-item |
| Table | pipe-separated columns | Table with header row |
| Horizontal rule | three or more dashes | Skipped, not rendered |
| Blockquote | greater-than + space | Rendered as cite element |
| Table of contents | ## Table of contents | Skipped (nav auto-generated from headers) |

### README.md example

Navigation is auto-generated from `##` and `###` headers:

```markdown
# Project Name
Short description.

## Getting started
Introduction text.

### Installation
Installation instructions.

### Configuration
Configuration details.

## Usage
Usage examples.

## License
MIT
```

### Output

| File | Description |
|------|-------------|
| `index.html` | Main frameset file |
| `hellodocs-title.html` | Title bar |
| `hellodocs-nav.html` | Navigation sidebar |
| `hellodocs-N.html` | Content sections (1, 2, 3, ...) |

### Folder structure

```
my-project/
├── src/
├── docs/
│   ├── hellodocs.awk
│   ├── README.md
│   ├── index.html                  (generated)
│   │   ├── hellodocs-title.html    (generated)
│   │   ├── hellodocs-nav.html      (generated)
│   │   ├── hellodocs-1.html        (generated)
│   │   └── ...
│   └── ...
└── ...
```

### Customization

**Command-line variables:**

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `title` | No | hellodocs | Documentation title in header, footer, and title bar |
| `source` | No | (none) | URL to source repository, adds "Source code" link in footer |

**Advanced: theming (edit hellodocs.awk to change):**

| Constant | Default | Description |
|----------|---------|-------------|
| `NAV_BG` | lavender | Navigation background color |
| `CONTENT_BG` | gainsboro | Content background color |
| `CODE_BG` | blanchedalmond | Code block background color |
| `LINK` | dodgerblue | Link color |
| `VLINK` | dimgray | Visited link color |
| `ALINK` | tomato | Active link color |
| `TEXT` | black | Text color |

Colors must be web-safe named colors.

## Integration

hellodocs can be integrated into your build process to automatically regenerate documentation when your README.md changes. Add one of the following snippets to your project.

```
DEVELOPMENT.md repo → GitHub Action → /docs/index.html
```

### GitHub Actions

Automatically generate documentation on every push to main/master.

**Step 1: Create workflow directory**
```bash
mkdir -p .github/workflows
```

**Step 2: Create workflow file**

Create `.github/workflows/hellodocs.yml` with the following content:

```yaml
name: Generate Documentation
on:
  push:
    branches: [main, master]
  workflow_dispatch:  # Manual trigger
permissions:
  contents: write  # Required to commit generated docs
jobs:
  build-docs:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Pull latest changes
        run: git pull origin ${{ github.ref_name }}

      - name: Create docs directory
        run: mkdir -p docs

      - name: Download source documentation
        run: |
          curl -fsSL --max-time 30 -o docs/README.md https://raw.githubusercontent.com/YOUR_ORG/YOUR_REPO/refs/heads/master/README.md

      - name: Download hellodocs
        run: |
          curl -fsSL --max-time 30 -o docs/hellodocs.awk https://raw.githubusercontent.com/tirrenotechnologies/hellodocs/refs/heads/master/hellodocs.awk

      - name: Verify hellodocs (basic sanity check)
        run: |
          head -1 docs/hellodocs.awk | grep -q '^#' || exit 1
          grep -q 'function inline_fmt' docs/hellodocs.awk || exit 1
          grep -q 'function html_head' docs/hellodocs.awk || exit 1

      - name: Generate HTML documentation
        run: |
          cd docs
          awk -v title='Hello, Docs!' -v source='https://github.com/YOUR_ORG/YOUR_REPO' -f hellodocs.awk README.md

      - name: Cleanup source files
        run: |
          rm -f docs/hellodocs.awk
          rm -f docs/README.md

      - name: Commit and push generated docs
        run: |
          git config user.name "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"
          git add docs/
          if git diff --staged --quiet; then
            echo "No changes to commit"
          else
            git commit -m "hellodocs: regenerate documentation"
            git push
          fi
```

**Step 3: Customize the workflow**

Change these values to match your project:

| Line | Value | Description |
|------|-------|-------------|
| `mkdir -p docs` | `docs` | Output folder name (change to your preferred folder) |
| `YOUR_ORG/YOUR_REPO` | URL path | Source repository containing README.md |
| `title='Hello, Docs!'` | Title | Displayed in header, footer, and title bar |
| `source='https://...'` | URL | Link to source code in footer |

**Step 4: Trigger the workflow**

Push to main/master branch, or manually trigger via: Actions → Generate Documentation → Run workflow

**Output structure:**
```
docs/
├── hellodocs.awk
├── README.md
├── index.html                  (generated)
├── hellodocs-title.html        (generated)
├── hellodocs-nav.html          (generated)
├── hellodocs-1.html            (generated)
└── ...
```

### Makefile

Allows running `make docs` to regenerate documentation locally.

```makefile
docs:
  cd docs && awk -v title='Hello, Docs!' -v source='https://github.com/user/repo' -f hellodocs.awk README.md
```

**npm (package.json)** allows running `npm run docs` in Node.js projects.
```json
"scripts": {
  "docs": "cd docs && awk -v title=\"Hello, Docs!\" -v source=\"https://github.com/user/repo\" -f hellodocs.awk README.md"
}
```

**Dockerfile** generates documentation during container build, embedding docs into your Docker image.
```dockerfile
FROM php:8.2-apache
COPY . /var/www/html/
RUN cd /var/www/html/docs && \
    awk -v title='Hello, Docs!' -v source='https://github.com/user/repo' -f hellodocs.awk README.md
```

## Security

After generating documentation, remove the source files:
```bash
rm hellodocs.awk README.md
```

## Troubleshooting

| Problem | Solution |
|---------|----------|
| AWK not found | Run `awk --version` to check if AWK is installed. AWK is pre-installed on most Unix systems. On Windows use WSL, Cygwin, or MSYS2. |
| Markdown not rendering as expected | Check syntax of your markdown, and if it's passing validation simplify your markup. |
| Permission denied | Run `chmod 644 *.html` to set read permissions. Verify with `ls -la *.html`. |

## Contribution

Contributions are welcome. The goal of hellodocs is to provide documentation that any browser can render five years ago, tomorrow, and decades from now.

Please respect the project constraints: single file (everything in `hellodocs.awk`), pure POSIX-compatible AWK with no specific extensions, no external dependencies, no JavaScript, no CSS, no inline styles, web-safe named colors only (lavender, gainsboro, etc.), frameset-based navigation.

## Found a mistake?

If you have found a mistake in the documentation, no matter how large or small, please let us know by [creating a new issue](https://github.com/tirrenotechnologies/hellodocs/issues/new) in the hellodocs repository.

## License

MIT License. Free to use, modify, and distribute.

't'
