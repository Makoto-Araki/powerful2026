# powerful2026 Shopifyテーマのカスタマイズ記録

## 目的
- Shopifyテーマのカスタマイズ記録を取り、顧客からのカスタマイズ案件に備える。エンジニア向けの説明のため注意。

## 開発履歴
| 日付       | バージョン | 説明                                                               |
| ---------- | --------- | ------------------------------------------------------------------ |
| 2026/09/21 | 0.2.0     | 業務フローをClaude Code依頼型に変更                                  |
| 2026/09/21 | 0.1.0     | README作成                                                         |

## 開発環境
- Windows11
- WSL(Ubuntu)
- Shopify Cli
- VSCode
- Git
- Claude Code

## 開発の進め方
テーマの調査・修正は、自然言語で Claude Code に依頼して進める。Shopify CLI の操作はユーザーが行い、Claude Code は実行しない。コミット・プッシュ・PR作成は、ユーザーが依頼したときだけ Claude Code が行う。

| 手順 | 担当 | 内容 |
| ---- | ---- | ---- |
| 1 | ユーザー | 作業前の準備(下記「開発手順1」) |
| 2 | ユーザー | Claude Code にテーマ調査・テーマ修正を依頼する |
| 3 | Claude Code | `main` をプルし、開発ブランチを作成して、調査・コード修正を行う。修正後は変更内容を報告して止まる |
| 4 | ユーザー | プレビューで確認する(下記「開発手順2」)。修正が必要なら、Claude Code に伝えて 3 に戻る |
| 5 | ユーザー → Claude Code | プレビュー確認後に、コミット・プッシュ・PR作成を依頼する。開発履歴の追記案を確認してもらい、同じコミットに含める |
| 6 | ユーザー | GitHub 上で PR をマージし、リモートの開発ブランチを削除する。必要に応じて Shopify にプッシュする(下記「開発手順3」) |
| 7 | ユーザー → Claude Code | ローカルの開発ブランチの削除を依頼する |
| 8 | ユーザー | Shopify からログアウトする |

- 調査のみの依頼(コードを変更しない)では、開発ブランチ・コミット・PR は作らず、調査結果の報告で終える。
- ストア側の情報(テーマエディタの設定、アプリ、エラー内容、再現 URL など)が必要な場合は、依頼時にユーザーが Claude Code に伝える。
- Issue は使わない。
- 1つの依頼につき、1つのブランチ、1つの PR とする。前の PR がマージされてから、次の依頼を行う。

## 開発手順
- Shopify上でテーマ複製を行い、編集用(edit)と保存用(backup)を作成しておくこと
- WSL上で作業を行う
- Shopify CLI のコマンドは、ユーザーが実行する

### 開発手順1 作業前の準備
```bash
## ログイン
$ shopify auth login

## テーマの一覧 ※テーマIDを確認
$ shopify theme list --store powerful2026.myshopify.com

## 過去のプレビュー用テーマの削除
$ shopify theme delete --theme テーマID --store powerful2026.myshopify.com

## 編集用テーマのプル
$ shopify theme pull --theme テーマID --store powerful2026.myshopify.com
```

- `theme pull` で差分が出た場合は、Claude Code への依頼時に伝える。今回の変更に含めるかは、依頼時に決める。

### 開発手順2 プレビュー確認
```bash
## 編集用テーマをプレビュー
$ shopify theme dev --theme テーマID --store powerful2026.myshopify.com
```

### 開発手順3 マージ後
```bash
## 必要に応じて、修正済のテーマをShopifyにプッシュして顧客に修正済のテーマを確認してもらう
$ shopify theme push --theme テーマID --store powerful2026.myshopify.com

## ログアウト
$ shopify auth logout
```

### Git 操作 (Claude Code が実施)
Claude Code は次の操作を行う。コミット・プッシュ・PR作成は、ユーザーの依頼があったときだけ行う。

- 作業開始時: `main` に切り替えて `git pull origin main` を行い、開発ブランチ `feature/英語の短い名前` を作成する
- 依頼時: ファイルを指定してステージングし、コミット・プッシュ・PR作成を行う
- 依頼時: `main` をプルした後、ローカルの開発ブランチを `git branch -d` で削除する

リモートの開発ブランチの削除は、PR マージ後にユーザーが GitHub 上で行う。
