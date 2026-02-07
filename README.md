# bilican.xyz

Personal bilingual blog (EN/TR) built with Hugo.

## Local development

```bash
hugo server -D
```

Open http://localhost:1313. The `-D` flag includes draft posts.

## Adding posts

### English post

```bash
hugo new content/en/posts/my-post-title.md
```

### Turkish post

```bash
hugo new content/tr/posts/yazi-basligi.md
```

This creates a file with today's date and `draft = true`. Edit the generated file and write your content below the front matter:

```markdown
---
title: "Your Post Title"
date: 2026-02-07
description: "A short summary for SEO/RSS."
tags: ["tag1", "tag2"]
draft: true
---

Your content here. Regular markdown — **bold**, *italic*,
[links](https://example.com), code blocks, etc.
```

### Publishing

Remove `draft: true` (or set to `false`) when ready to publish. Push to `main` and GitHub Actions deploys automatically.

### Good to know

- **Reading time** — calculated automatically from word count
- **Language tag** — determined by folder (`content/en/` or `content/tr/`)
- **URL** — derived from filename: `my-first-post.md` becomes `/posts/my-first-post/`
