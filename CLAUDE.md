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

### i18n

- English pages: `src/pages/` (root)
- Japanese pages: `src/pages/ja/`
- Layout handles language switching via `lang` prop
