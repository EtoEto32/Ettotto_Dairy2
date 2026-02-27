# CTF_Blog
CTF Writeup 用のブログ兼メモ置き場（Astro 製）

### 参考文献
https://qiita.com/Michinosuke/items/09c30fde4ca7168f96ef#%E6%89%80%E8%A6%81%E6%99%82%E9%96%93

---

## プロジェクト構成

このリポジトリは、Astro 製ブログプロジェクト `my-blog` をメインとして構成されています。

```text
ettotto_diary2/
├── my-blog/                 # Astro ブログ本体
│   ├── public/              # 静的ファイル（画像など）
│   ├── src/
│   │   ├── components/      # 共通コンポーネント（ヘッダー、フッター等）
│   │   ├── content/
│   │   │   └── blog/        # ブログ記事（Markdown / MDX）
│   │   ├── layouts/         # 記事ページ用レイアウト
│   │   └── pages/           # ルーティングされるページ (.astro / .md)
│   ├── astro.config.mjs     # Astro の設定（サイトURL、プラグイン等）
│   ├── package.json         # 依存パッケージと npm スクリプト
│   ├── package-lock.json    # 依存関係のロックファイル
│   ├── tsconfig.json        # TypeScript / Astro 用の設定
│   └── README.md            # Astro テンプレート由来の README
└── README.md                # （このファイル）リポジトリ全体の概要
```

## 技術スタック / 構成

- **フレームワーク**: Astro 5 系（ブログスターター）
- **言語**: TypeScript / Astro (`.astro` ファイル)
- **パッケージマネージャ**: npm
- **主な機能**
  - CTF Writeup / 技術メモを Markdown / MDX で管理
  - RSS フィード (`@astrojs/rss`)
  - サイトマップ (`@astrojs/sitemap`)
  - シンプルなブログレイアウト（`BlogPost.astro` ベース）

## 開発環境の使い方（ローカル）

開発は `my-blog` ディレクトリをルートとして行います。

```bash
# 1. プロジェクトルートへ移動
cd D:/UserApp/git/ettotto_diary2

# 2. ブログプロジェクトへ移動
cd my-blog

# 3. 依存パッケージのインストール（初回 or 依存更新時のみ）
npm install

# 4. 開発サーバ起動
npm run dev
# → http://localhost:4321 でブログを確認
```

本番ビルドやプレビューは以下のコマンドを利用してください。

```bash
# 本番ビルド
npm run build

# ビルド結果のローカルプレビュー
npm run preview
```

## デプロイ / CI（Cloudflare Pages）

- **ホスティング先**: Cloudflare Pages（プロジェクト名: `Ettotto_Dairy2` 想定）
- **CI/CD**: GitHub Actions で `main` ブランチへの push をトリガーに、自動デプロイ
  - ワークフロー定義: `.github/workflows/deploy-cloudflare.yml`
  - 手順（GitHub Actions 内）:
    - `cd my-blog`
    - `npm ci`
    - `npm run build` → `my-blog/dist` を生成
    - `wrangler pages deploy my-blog/dist --branch main --project-name Ettotto_Dairy2`
- 必要な GitHub Secrets:
  - `CLOUDFLARE_API_TOKEN`: Cloudflare API Token（Pages へのデプロイ権限付き）
  - `CLOUDFLARE_ACCOUNT_ID`: Cloudflare Account ID

ブランチ運用の想定:

- `dev` ブランチで開発 → PR → `main` にマージ
- `main` にマージ / push されたタイミングで CI が走り、Cloudflare Pages に反映される。

## 今後のカスタマイズ方針メモ

- `my-blog/src/consts.ts`
  - サイトタイトル・説明文など、ブログ全体のメタ情報を管理
- `my-blog/src/pages/index.astro`
  - トップページの文言や構成を編集
- `my-blog/src/content/blog/`
  - Markdown / MDX ファイルを追加して記事を増やす
- `my-blog/src/layouts/BlogPost.astro`
  - 記事ページのレイアウト・デザインを調整
## pagesプロジェクト作成  
CloudflarePagesのUIが分かりづらい。
https://qiita.com/q-1-p/items/37209f221a74752078dc
```
wrangler pages project create <projectname>
```

