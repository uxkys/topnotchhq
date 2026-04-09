# CLAUDE.md — Project Instructions for Claude

## Project

Top Notch Inc. 公式サイト（Astro + Cloudflare Workers & Pages）
Local path: /Users/koyasu/document/src/github.com/topnotchhq

## Critical Rules

### Blog Article Creation

When asked to create a blog article, ALWAYS create BOTH English and Japanese versions:

1. English: `src/content/blog/{slug}.md` with `lang: en`
2. Japanese: `src/content/blog/{slug}-ja.md` with `lang: ja`

Both articles must have the same content and structure, properly localized for each language.

### Deployment

After making changes:
1. `npm run build` to verify
2. `git add` the changed files
3. `git commit` with descriptive message
4. `git push` to trigger auto-deploy on Cloudflare

### i18n — Bilingual Consistency Rule

**English and Japanese versions must ALWAYS have identical design.**
The ONLY difference between `/` and `/ja/` is the language of the text content.

Rules:
- When modifying any page design (layout, CSS, components), apply the SAME changes to both the English and Japanese versions
- English pages: `src/pages/` (root)
- Japanese pages: `src/pages/ja/`
- Shared layout: `src/layouts/BaseLayout.astro` (handles both languages via `lang` prop)
- Shared styles: `src/styles/global.css`
- If a design change only requires editing `BaseLayout.astro` or `global.css`, it automatically applies to both languages (no duplication needed)
- If a design change requires editing page-level `<style>` blocks, ensure BOTH the English and Japanese page files receive the exact same CSS changes
- Never introduce language-specific styling differences
