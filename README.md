# Top Notch Inc. — Official Website

## Overview

株式会社トップノッチ（Top Notch Inc.）の公式Webサイト。
国際ビジネス及びブランド戦略に特化したアドバイザリー会社のコーポレートサイト。

- **本番URL**: https://topnotchhq.com
- **ドメイン管理**: Cloudflare
- **ホスティング**: Cloudflare Workers（Workers Builds によるGitHub連携自動デプロイ）
- **フレームワーク**: Astro（静的サイト生成）
- **リポジトリ**: https://github.com/uxkys/topnotchhq

## Architecture

### Hosting & Deployment

- **Cloudflare Workers Builds** を使用（2026年5月に Pages から移行済み）
- GitHub `main` ブランチへの push で自動ビルド・デプロイ
- ビルドコマンド: `npm run build` → 静的ファイルを `dist/` に出力
- デプロイコマンド: `npx wrangler deploy` → リポジトリルートの `wrangler.jsonc` を読む
- カスタムドメイン `topnotchhq.com` を Cloudflare DNS で設定済み
- プレビューURL: `topnotchhq.wrkkys.workers.dev`

#### wrangler.jsonc について

`wrangler.jsonc` は**静的アセット専用 Worker**として設定されている（`assets.directory: "./dist"`、`main` エントリポイントなし）。

このサイトは `output: "static"` の完全な静的サイトなので、SSRアダプタ（`@astrojs/cloudflare`）は**不要かつ追加してはいけない**。

`wrangler.jsonc` が無いと、wrangler がビルド環境（読み取り専用）にアダプタを自動インストールしようとして**デプロイ段階だけが失敗する**。このときビルドログ上は Astro のビルドが成功して見えるため気づきにくい。実際にこの状態で本番が3か月間更新されないまま止まっていたことがある。

変更が本番に出ないときは、ビルドログの成功表示ではなく **Cloudflare ダッシュボードの Builds タブのステータス**を確認する。

push 前にローカルで確認する場合:

```
npx wrangler deploy --dry-run
```

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
docs/
├── business/            # Webサイトには掲載しない事業整理メモ
└── source-materials/    # ブログ記事の元原稿（docx等）の保管場所
wrangler.jsonc            # Cloudflare Workers デプロイ設定（静的アセット専用）
```

※ `docs/` は Astro の配信対象外（配信されるのは `public/` のみ）。元原稿を `public/` に置くと本番サイトからダウンロード可能になってしまうので置かないこと。

### Pages & Sections

トップページは以下のセクションで構成:

1. **Hero** — キャッチコピー + CTA
2. **About** — 会社概要 + ロゴ
3. **Services** — 4つのサービス（市場調査、ブランド戦略、パートナー開拓、事業展開）
4. **Founder** — 馬場崎 修のプロフィール + 写真 + 経歴
5. **Contact** — メール連絡先（o.babasaki@topnotchhq.com）

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
- 認証プロキシ: Cloudflare Workers 上の `sveltia-cms-auth.wrkkys.workers.dev` を利用

### Business Notes

- `docs/business/` はTop Notch関連プロジェクトや新規事業アイデアの整理場所
- Webサイトには表示されない
- このリポジトリは公開前提のため、営業リスト・価格戦略・顧客候補・契約情報などの機密情報は置かない
- 機密性がある内容は別のprivateリポジトリまたは非公開ドキュメントで管理する

### Source Materials

- `docs/source-materials/` はブログ記事の元になった原稿（docx等）の保管場所
- 記録として残すためのもので、Webサイトからはアクセスできない
- Business Notes と同じく、公開リポジトリに置けない機密情報は保存しない

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
