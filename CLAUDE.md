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

Diagrams MUST use plain ASCII characters only — no Unicode box-drawing chars (┌─┐│└┘▼ etc.). Unicode chars in the U+2500+ range render at inconsistent pixel widths because Google Fonts subsets them out, causing browser font fallback mid-line that breaks alignment even when character counts are equal.

Use: `+` for corners and junctions, `-` for horizontal lines, `|` for vertical lines, `v` for down-arrows.

If a diagram exists with Unicode chars, replace them using this PowerShell snippet:
```powershell
$map = @{
  [char]0x250C='+';[char]0x2510='+';[char]0x2514='+';[char]0x2518='+'
  [char]0x251C='+';[char]0x2524='+';[char]0x252C='+';[char]0x2534='+'
  [char]0x2500='-';[char]0x2502='|';[char]0x25BC='v'
}
$c = [System.IO.File]::ReadAllText($file)
foreach ($kv in $map.GetEnumerator()) { $c = $c.Replace([string]$kv.Key,[string]$kv.Value) }
[System.IO.File]::WriteAllText($file, $c)
```

After any diagram change, verify all box-border lines have equal length:
```powershell
$lines = [System.IO.File]::ReadAllLines("path\to\file.mdx")
foreach ($l in $lines) {
  if ($l -match '[\+\-\|]' -and $l.Length -gt 10) { Write-Host "$($l.Length): $l" }
}
```
