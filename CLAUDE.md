# Claude Code Instructions — sandeeprn-portfolio

Personal portfolio for Sandeep R N (sandeeprn.in). Astro 5, Tailwind CSS v4, MDX content collections, hosted on Cloudflare Pages. Pushing to `master` auto-deploys.

## Positioning (read before editing any copy)

- Current title: **DevOps Engineer · Site Reliability Engineer** (Senior Cloud Engineer, SRE at SymphonyAI, Apr 2026 – present; Cloud Engineer Mar 2023 – Mar 2026, same employer, promotion).
- Target: DevOps / SRE roles, open to remote. Copy should lead with the DevOps toolchain, then SRE impact.
- **Never overstate.** Interviewers probe every claim. Facts to respect:
  - The company does NOT run Kubernetes or Prometheus in production. Do not claim it. They appear only in the personal EKS project.
  - The EKS project (`eks-terraform.mdx`) is built on a course repo (a fork, `ultimate-devops-project-aws`). Describe it as a personal project, never as production work.
  - Salary, target salary, customer names, and internal team names must never appear (see Sensitive content).
- The resume PDF is `public/Sandeep_RN_Resume.pdf`. When the user supplies a new resume, replace that file and sync site copy and numbers to it.

## Deployment Rule

After **every** set of file changes, with no exceptions:

1. `npm run build` (must finish with zero errors)
2. `git add <specific files>`
3. `git commit -F <message file>` — PowerShell breaks on quotes in `-m` strings and heredocs, so write the message to a temp file
4. `git pull --rebase origin master`, then `git push origin master`

Do not stop at file edits and consider the task done. End commit messages with the attribution line the session specifies.

Only `master` exists. The old AZ-400 "in progress" variant is preserved as the tag `archive/az400-inprogress` (restore with `git checkout -b az400-inprogress archive/az400-inprogress`).

## Content rules

Collections are defined in `src/content.config.ts`. The loader only reads `*.mdx`; a `.md` file is silently ignored.

- **Projects** (`src/content/projects/*.mdx`): `title`, `description`, `tags[]`, `status` (`done` | `in-progress`), `date` (string), optional `github`. Only add `github` if the repo actually contains that project's code.
- **Blog** (`src/content/blog/*.mdx`): `title`, `description`, `tags[]`, `date` ("YYYY-MM-DD" string). Use `date`, not `pubDate`.
- Homepage project cards are hardcoded in `src/pages/index.astro` (not generated from the collection). When a project's title, description, or status changes, update the card there too.

### Blog pre-publish checklist

1. **Tag colors:** every tag must exist in `tagColors` in `src/pages/blog/index.astro`. Tags are Title Case (`Kubernetes`, not `kubernetes`).
2. **Frontmatter:** all four required fields, valid `date`.
3. **Sensitive content:** no real customer names (use Customer A, B), no internal team names (use Team A, B), no salary figures, no internal hostnames, IPs, or compartment names.
4. **Diagrams:** ASCII only (see below).

## ASCII diagrams

Diagrams must use plain ASCII only: `+` corners and junctions, `-` horizontal, `|` vertical, `v` down arrow, `>` right arrow. No Unicode box-drawing characters, and no `→` or `—` inside a diagram. Those render at inconsistent widths because the web font is subset and the browser falls back mid-line, which breaks alignment even when character counts match.

After any diagram change, check that all lines within each box have equal length:

```powershell
$lines = [System.IO.File]::ReadAllLines("path\to\file.mdx")
foreach ($l in $lines) { if ($l -match '^\s*[\+\|]' -and $l.Length -gt 10) { "$($l.Length): $l" } }
```

## Layout notes (`src/pages/index.astro`)

Section order: Hero, About, Stats strip, Skills, Projects, Latest Posts, Contact. Spacing was tuned and measured at 1440x900: 48px above and below the stats cards, the same section padding as everywhere else, no forced hero min-height. Do not reintroduce `min-h-[90vh]` on the hero; it leaves large empty bands at tall or zoomed-out viewports.

For visual changes, run the dev server (`npm run dev`), measure with `getBoundingClientRect` or look at it in a browser, and stop the server afterward. A passing build does not prove the layout is right.

## Related

- GitHub profile README: repo `Sandeep-RN/Sandeep-RN`, local copy at `C:\Users\SandeepNagaraj\Projects\README_github.md`. Update it with `gh api` PUT (base64 content plus the current sha). Keep it in sync with the portfolio (title, projects, cert progress).
- Cert progress shown in two places that must match: `src/pages/certifications.astro` and the About sidebar in `src/pages/index.astro`.
- Windows + PowerShell environment: no `tail`, no `&&`, no heredocs for git messages.

## Agent skills

### Issue tracker

Issues are tracked in GitHub Issues for Sandeep-RN/sandeeprn-portfolio (via `gh`). See `docs/agents/issue-tracker.md`.

### Triage labels

Default five-label vocabulary (needs-triage, needs-info, ready-for-agent, ready-for-human, wontfix). See `docs/agents/triage-labels.md`.

### Domain docs

Single-context: one `GLOSSARY.md` + `docs/adr/` at the repo root. See `docs/agents/domain.md`.
