# dots-oauth（公開リポジトリ）

> **このリポジトリは誰でも見られる public です。**
> 置いてよいのは「サインイン後に dots アプリへ戻すだけの静的ページ」だけです。

## 役割

Slack などの OAuth は https の戻り先 URL が必要です。ここは GitHub Pages で
`https://qurio-inc.github.io/dots-oauth/<サービス>/` を公開し、受け取ったクエリ
（`?code=…&state=…`）を `dots://<サービス>-callback` にそのまま渡すだけのページを置きます。
コードの交換（秘密鍵を使う処理）は Mac の dots サーバーだけで行います。

## 置いてよいもの（これ以外は禁止）

| パス | 中身 |
|---|---|
| `README.md` | このルール |
| `<サービス>/index.html` | 戻すだけの静的 HTML（下のひな形どおり、外部読み込みなし） |
| `.github/workflows/guard.yml` | 下の自動チェック |

## 絶対に置かないもの

- トークン・API キー・Client Secret・パスワード・`.env`・証明書や鍵（`.p8` `.pem`）
- Client ID、ワークスペース名、メールアドレス、IP アドレス、Tailscale のホスト名
- アクセス解析・外部スクリプト・外部フォント（ページは JS 数行のみ）
- dots 本体のコードや設定（本体は private の `Qurio-Inc/dots_ttpm`）

## 変更するとき

1. 変更は上の表のファイルだけ。新しいサービスを足すときも `index.html` のひな形をコピーしてパスとスキーム名だけ変える。
2. push すると `guard` が自動で走り、許可外のファイルや秘密っぽい文字列があれば失敗します。失敗したら**消して force push ではなく、すぐ相談**（一度 push した秘密は漏れた前提で鍵を作り直す）。
3. 秘密を間違えて push した場合: その鍵・トークンをすぐ無効化して再発行 → 履歴から削除 → 影響範囲を確認。

## ひな形

```html
<!doctype html>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>dots</title>
<p>dots にもどっています…</p>
<script>location.replace('dots://SERVICE-callback' + location.search);</script>
```
