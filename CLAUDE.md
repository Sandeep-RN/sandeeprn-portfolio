# Claude Code Instructions — sandeeprn-portfolio

## Deployment Rule

After **every** set of file changes — no exceptions — run:

1. `npm run build` (verify zero errors before proceeding)
2. `git add <changed files>`
3. `git commit -m "..."` 
4. `git push origin master`

Cloudflare Pages auto-deploys on push. Do not stop at file edits and consider the task done.

## Blog Post Pre-publish Checklist

Run these checks every time a new blog post (`src/content/blog/*.mdx`) is added or tags are changed.

### 1. Tag colors

All tags in a post's frontmatter must have a corresponding entry in the `tagColors` dictionary in `src/pages/blog/index.astro`.

**Check:** compare the post's `tags: [...]` array against the keys in `tagColors`.  
**Fix:** add any missing tags to the dictionary with a distinct color before committing.

Current tag colors are in `src/pages/blog/index.astro` lines 8-17 (approx).

### 2. Required frontmatter fields

Every blog post must have:
```
title:       string
description: string
tags:        string[]
date:        "YYYY-MM-DD"
```

### 3. Sensitive content

Before committing any content file (blog or project):
- No real customer names — use Customer A, Customer B, etc.
- No internal team/project names (GOLD, CatPlan, CCR, Ninja) — use Team A, Team B, etc.
- No salary figures or target salary ranges
- No internal hostnames, IPs, or compartment names

### 4. ASCII diagram alignment

If a blog or project post contains ASCII box-drawing diagrams, verify every line inside the code block has the same character length using PowerShell:

```powershell
$lines = [System.IO.File]::ReadAllLines("path\to\file.mdx")
foreach ($l in $lines) {
  if ($l.Contains([char]0x250C) -or $l.Contains([char]0x2502) -or $l.Contains([char]0x2514)) {
    Write-Host "$($l.Length): $l"
  }
}
```

All box-border lines must print the same length number. If they differ, rebuild the diagram with calculated padding — do not hand-align.
