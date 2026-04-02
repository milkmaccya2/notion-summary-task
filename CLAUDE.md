# Notion 要約タスク

## 概要

Notion Inbox DBの「要約」カラムが空のレコードを検出し、コンテンツを読み取って300字以内の日本語で要約を生成・書き込むタスク。

## Notion DB 情報

- DB ID: `2b93611beab880fc8a4cfdb981797149`
- ビューURL: `view://31d3611b-eab8-8027-87cd-000c917e2746`（週報用ビュー）

## プロパティ

| プロパティ | 型 | 説明 |
|-----------|------|------|
| 名前 | title | ページタイトル |
| 日付 | created_time | 作成日 |
| 要約 | text | 要約テキスト（このカラムに書き込む） |
| URL | url | 元のURL |
| タグ | multi_select | カテゴリタグ |

## 要約ルール

- 日本語で300字以内
- ページのコンテンツ（本文）を読み取って要約する
- 要約カラムが空のレコードのみ対象
- 既に要約が入っているレコードはスキップ

## Notion MCP 手順

1. `notion-fetch` で DB ID を指定し、データソース情報とスキーマを取得する
2. `notion-search` で DB 内のページを検索する（※セマンティック検索のため漏れが出る可能性あり）
3. 各ページを `notion-fetch` で取得し、`要約` プロパティが空かどうか確認する
4. 空のページに対して `notion-update-page`（`update_properties` コマンド）で要約を書き込む

## Notion MCP 注意事項

- `notion-query-database-view` は **Business プラン以上 + Notion AI** が必要。Free/Plus プランでは使えない
- ビューのクエリには `view://` 形式を使うこと（`https://notion.so/...?v=xxx` 形式はエラーになる）
- `notion-search` はセマンティック検索で漏れが出る可能性があるが、ビュークエリが使えない場合の代替手段として利用可
- `notion-update-page` の `properties` では、URL プロパティは `userDefined:URL` のようにプレフィックスが必要
- データソース URL は `collection://2b93611b-eab8-805f-99b5-000b39248424`
