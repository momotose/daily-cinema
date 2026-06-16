# DailyCinema Color（ブラウザ完結版）

動画のカラーグレーディングを全てブラウザ内で行う静的Webアプリ。
動画はどこにもアップロードされない（サーバー処理ゼロ）。

## ローカルで使う

```bash
cd ~/Desktop/DailyCinema/color-web
python3 -m http.server 8077
# → http://localhost:8077 を開く
```

## 公開する（GitHub Pages）

1. このフォルダを git リポジトリにして GitHub に push
2. リポジトリ Settings → Pages → Branch: main / root を選択
3. 数分後 `https://<ユーザー名>.github.io/<リポジトリ名>/` で公開される

静的ファイルのみなのでサーバー費用ゼロ。Cloudflare Pages / Netlify でも同様に置くだけ。

## 構成

- `index.html` — アプリ本体（単一ファイル、依存ライブラリなし）
- `luts/*.cube` — 同梱フィルムベースLUT
- `luts/index.json` — LUT一覧マニフェスト（.cube追加時はここに名前を追記）

## 技術メモ

- プレビュー: WebGL2（3D LUT + GLSL grade()）
- 書き出し: canvas.captureStream + MediaRecorder（Chrome=mp4/H.264、非対応ブラウザはwebm）
  - 実時間書き出し（30秒のクリップなら30秒かかる）
  - 解像度はクロップ後の長辺最大1920px
- マイLUT: localStorage 保存。⤓ボタンで標準 .cube としてダウンロード可能（DaVinci/iOSアプリ互換）
- GLSL `grade()` と JS `gradeJS()` は同一数式を厳守（.cube書き出しの色一致のため）
