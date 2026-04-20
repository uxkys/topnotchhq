# Top Notch Inc. — Official Website

## Overview

株式会社トップノッチ（Top Notch Inc.）の公式Webサイト。
国際ビジネス及びブランド戦略に特化したアドバイザリー会社のコーポレートサイト。

- **本番URL**: https://topnotchhq.com
- **ドメイン管理**: Cloudflare
- **ホスティング**: Cloudflare Workers & Pages（GitHub連携で自動デプロイ）
- **フレームワーク**: Astro（静的サイト生成）
- **リポジトリ**: https://github.com/uxkys/topnotchhq

## Architecture

### Hosting & Deployment

- Cloudflare Workers & Pages を使用
- GitHub `main` ブランチへの push で自動ビルド・デプロイ
- ビルドコマンド: `npm run build`
- 出力ディレクトリ: `dist/`
- カスタムドメイン `topnotchhq.com` を Cloudflare DNS で設定済み
- プレビューURL: `topnotchhq.wrkkys.workers.dev`

### Internationalization (i18n)

- **英語（デフォルト）**: `/` 以下
- **日本語**: `/ja/` 以下
- ナビバーに EN/JA 切り替えボタンあり
- 全てのページ・ブログ記事は英語版と日本語版の両方を持つ

### Project Structure

```
src/
├── content/
│   └── blog/              # ブログ記事（Markdown）
│       ├── welcome.md      # 英語記事 (lang: en)
│       └── welcome-ja.md   # 日本語記事 (lang: ja)
├── content.config.ts       # Content Collections 定義
├── layouts/
│   └── BaseLayout.astro    # 共通レイアウト（ヘッダー/フッター/i18n）
├── pages/
│   ├── index.astro         # 英語トップページ
│   ├── blog/
│   │   ├── index.astro     # 英語ブログ一覧
│   │   └── [id].astro      # 英語ブログ記事ページ
│   └── ja/
│       ├── index.astro     # 日本語トップページ
│       └── blog/
│           ├── index.astro # 日本語ブログ一覧
│           └── [id].astro  # 日本語ブログ記事ページ
├── styles/
│   └── global.css          # グローバルCSS（カラー変数等）
public/
├── images/
│   ├── TopNotchInc_icon.avif        # 会社ロゴ
│   └── topnotch_babasaki_photo001.avif  # 代表者写真
└── favicon.svg
```

### Pages & Sections

トップページは以下のセクションで構成:

1. **Hero** — キャッチコピー + CTA
2. **About** — 会社概要 + ロゴ
3. **Services** — 4つのサービス（市場調査、ブランド戦略、パートナー開拓、事業展開）
4. **Founder** — 馬場崎 修のプロフィール + 写真 + 経歴
5. **Contact** — メール連絡先（hello@topnotchhq.com）

### Blog System

- Astro Content Collections を使用
- 記事は `src/content/blog/` に Markdown ファイルで管理
- 各記事の frontmatter に `lang: en` または `lang: ja` を指定
- 一覧ページで `lang` フィールドによりフィルタリング
- 記事ファイル名がURLスラッグになる（例: `my-article.md` → `/blog/my-article/`）

### Design

- カラー: ダークネイビー (`#0f1b2d`) + ブルーアクセント (`#2563eb`)
- フォント: Inter（Google Fonts）
- レスポンシブ対応（モバイル/タブレット/デスクトップ）

### Admin Panel (Sveltia CMS)

- URL: https://topnotchhq.com/workspace/
- 非エンジニアでもブログ記事をWYSIWYGで作成・編集できる
- GitHub OAuth でログイン（リポジトリへの書き込み権限が必要）
- 設定: `public/workspace/config.yml`
- 認証プロキシ: Sveltia公式ホスト版（auth.sveltia.app）を利用

## Claude への運用指示

### ブログ記事の作成ルール

**記事作成を依頼された場合、必ず英語版と日本語版の両方を作成すること。**

手順:
1. `src/content/blog/` に英語版 Markdown ファイルを作成（`lang: en`）
2. 同じディレクトリに日本語版 Markdown ファイルを作成（`lang: ja`、ファイル名末尾に `-ja` を付与）
3. 両方の記事は同じ内容・同じ構成で、それぞれの言語にローカライズする
4. `git add` → `git commit` → `git push` で自動デプロイ

ファイル命名規則:
- 英語: `my-article-slug.md`
- 日本語: `my-article-slug-ja.md`

Frontmatter テンプレート:
```yaml
---
title: "Article Title"
description: "Short description for SEO and blog list"
date: 2026-04-09
tags: ["Tag1", "Tag2"]
lang: en  # or ja
---
```

### サイト更新の手順

1. ローカルでファイルを編集
2. `npm run build` でビルド確認（任意）
3. `git add` → `git commit` → `git push`
4. Cloudflare が自動でビルド・デプロイ（通常1〜2分）

## Commands

| Command           | Action                                      |
| :---------------- | :------------------------------------------ |
| `npm install`     | Install dependencies                        |
| `npm run dev`     | Start local dev server at `localhost:4321`   |
| `npm run build`   | Build production site to `./dist/`           |
| `npm run preview` | Preview build locally before deploying       |
