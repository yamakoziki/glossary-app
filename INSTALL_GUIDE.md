# 多言語用語集アプリ インストール・運用ガイド

このガイドは、開発経験のない方が多言語用語集アプリを使い始めるための手順書です。

---

## 1. このアプリでできること

Excelで管理している用語集を、日本語・フランス語・英語に対応したWebサイトとして公開できます。

### 日常的な作業フロー

```
Excelを編集（LibreOffice）
        ↓
admin.html で読み込み・AI翻訳
        ↓
index.html を生成・ダウンロード
        ↓
docs/ フォルダに配置
        ↓
GitHub にプッシュ
        ↓
GitHub Pages で公開（自動）
```

---

## 2. 必要な環境

以下がすでにインストール済みであることを前提としています。

| ツール | 確認方法 |
|---|---|
| Mac（macOS 12以降） | アップルメニュー → このMacについて |
| Git | ターミナルで `git --version` |
| VS Code | アプリケーションフォルダに VS Code がある |
| Node.js | ターミナルで `node --version` |
| LibreOffice | アプリケーションフォルダに LibreOffice がある |
| Claude Code | ターミナルで `claude --version` |

---

## 3. 初回セットアップ手順

ターミナル（アプリケーション → ユーティリティ → ターミナル）を開いて、以下を順番にコピペして実行してください。

### Step 1: リポジトリをクローン（ファイルをダウンロード）

`ユーザー名` の部分を自分の GitHub ユーザー名に変えて実行します。

```bash
cd ~/Documents
git clone https://github.com/ユーザー名/glossary-app.git
cd glossary-app
```

✅ `glossary-app` フォルダが `~/Documents/` に作成されれば成功です。

### Step 2: 必要なツールをインストール

```bash
npm install
```

✅ `added XX packages` のようなメッセージが表示されれば完了です。

⚠️ エラーが出た場合は「5. よくあるトラブル」の「node_modules のエラー」を参照してください。

### Step 3: 動作確認

```bash
open admin/admin.html
```

✅ ブラウザで管理画面（用語集管理ツール）が開けば、セットアップ完了です。

---

## 4. 日常的な更新作業手順

### Step 1: Excel ファイルを編集する

1. `data/glossary.xlsx` を LibreOffice で開く
2. 用語の追加・修正を行う
3. **xlsx 形式（Excel 2007以降）で上書き保存する**

⚠️ 保存形式の確認：「名前を付けて保存」→ ファイル形式が「ODF スプレッドシート」になっていたら「Excel 2007-365」に変更してください。

✅ ファイル名が `.xlsx` で終わっていれば正しい形式です。

### Step 2: 管理画面で HTML を生成する

```bash
open ~/Documents/glossary-app/admin/admin.html
```

1. **Excel を読み込む** — ファイル選択エリアに `glossary.xlsx` をドラッグ＆ドロップ（または「ファイルを選択」ボタンを押す）
2. **内容を確認する** — 用語数・翻訳状況が表示される
3. **AI 翻訳を実行する（必要な場合）** — Anthropic API キーを入力して「空白のみ翻訳」ボタンを押す
4. **HTML を生成・ダウンロードする** — 「翻訳結果で HTML を生成・ダウンロード」ボタンを押す

✅ `index.html` がダウンロードフォルダ（`~/Downloads/`）に保存されれば成功です。

### Step 3: docs/ フォルダに配置する

```bash
cp ~/Downloads/index.html ~/Documents/glossary-app/docs/index.html
```

✅ `docs/index.html` が更新されたことを確認してください。

### Step 4: 画像を追加する場合

画像ファイルのファイル名ルール：`{用語ID}_{連番}.jpg`

例：用語ID `001` の画像 → `001_1.jpg`、`001_2.jpg`

```bash
# images/ フォルダと docs/images/ フォルダの両方にコピーする
cp 画像ファイルのパス ~/Documents/glossary-app/images/
cp 画像ファイルのパス ~/Documents/glossary-app/docs/images/
```

フォルダをまとめてコピーする場合：

```bash
cp ~/Documents/glossary-app/images/* ~/Documents/glossary-app/docs/images/
```

### Step 5: GitHub に保存する

```bash
cd ~/Documents/glossary-app
git add .
git commit -m "用語集を更新"
git push origin main
```

✅ `Everything up-to-date` または `To https://github.com/...` のメッセージが出れば成功です。プッシュ後、数分でWebサイトに反映されます。

### Step 6: ローカルで確認する

```bash
open ~/Documents/glossary-app/docs/index.html
```

✅ ブラウザでサイトが正しく表示されることを確認してください。

---

## 5. よくあるトラブルと対処法

### Q: admin.html で Excel が読み込めない

**A:** ファイルの保存形式を確認してください。

1. LibreOffice で `data/glossary.xlsx` を開く
2. 「名前を付けて保存」→ ファイル形式を **「Excel 2007-365 (.xlsx)」** に変更して保存
3. ✅ ファイル名が `.xlsx` で終わっていることを確認してから、再度 admin.html で読み込む

⚠️ LibreOffice のデフォルト保存形式（ODF形式：`.ods`）では読み込めません。必ず xlsx 形式で保存してください。

---

### Q: 翻訳ボタンが動作しない

**A:** API キー（Anthropic のサービスを使うための認証コード）を確認してください。

1. API キーが `sk-ant-api03-` で始まる文字列であることを確認する
2. API キーの前後に余分なスペースが入っていないか確認する
3. 入力欄にキーを貼り付けて、もう一度翻訳ボタンを押す

✅ API キーが正しく入力されると、翻訳ボタンが有効（青色）になります。

---

### Q: 画像が表示されない

**A:** ファイル名と保存場所を確認してください。

1. ファイル名が `{用語ID}_{連番}.jpg` の形式になっているか確認する（例：`001_1.jpg`）
2. `docs/images/` フォルダにコピーされているか確認する
3. `git add .` で画像ファイルも含めてコミット・プッシュする

⚠️ `images/` フォルダだけにあっても表示されません。公開用の `docs/images/` にも必ずコピーしてください。

---

### Q: GitHub に Push（アップロード）できない

**A:** Git に名前とメールアドレスを登録してください。

```bash
git config --global user.name "あなたの名前"
git config --global user.email "あなたのメールアドレス"
```

✅ 設定後、再度 `git push origin main` を実行してください。

---

### Q: `node_modules` に関するエラーが出る

**A:** `node_modules`（アプリが使うライブラリの保存フォルダ）が存在しない場合に起きます。以下を実行してください。

```bash
cd ~/Documents/glossary-app
npm install
```

✅ `added XX packages` のようなメッセージが表示されれば完了です。

---

## 6. ファイル構成の説明

```
glossary-app/
├── admin/
│   └── admin.html        # 管理ツール（Excel読込・翻訳・HTML生成）
├── data/
│   └── glossary.xlsx     # 用語集の元データ（ここを編集する）
├── docs/
│   ├── index.html        # 公開用Webサイト（admin.htmlが生成）
│   └── images/           # 公開用画像フォルダ
├── images/               # 画像のバックアップフォルダ
├── node_modules/         # npmパッケージ（自動生成・編集不要）
├── package.json          # Node.js設定ファイル（編集不要）
├── CLAUDE.md             # Claude Code向け設定（編集不要）
└── INSTALL_GUIDE.md      # このガイド
```

**編集するのは基本的にこの2つだけです：**

| ファイル | 用途 |
|---|---|
| `data/glossary.xlsx` | 用語・翻訳・カテゴリの追加・修正 |
| `docs/images/` | 画像ファイルの追加 |

⚠️ `docs/index.html` は admin.html で生成するファイルです。直接編集しないでください。

---

## 7. 画像ファイル名ルール

画像ファイルは以下の命名ルールに従ってください。

### 形式

```
{用語ID}_{連番}.{拡張子}
```

### ルール詳細

| 項目 | ルール | 例 |
|---|---|---|
| 用語ID | 3桁のゼロ埋め | `001`、`002`、`010` |
| 連番 | 1〜5（1用語につき最大5枚） | `1`、`2`、`3` |
| 拡張子 | jpg / jpeg / png / gif | `.jpg`、`.png` |

### 例

用語ID `001` の用語に画像を3枚登録する場合：

```
001_1.jpg
001_2.jpg
001_3.png
```

### 保存場所

⚠️ 以下の **2か所** に同じファイルをコピーしてください。

| フォルダ | 用途 |
|---|---|
| `images/` | 元データのバックアップ |
| `docs/images/` | Webサイトでの表示（こちらがないと表示されない） |

```bash
# 例：001_1.jpg を両方のフォルダにコピーする
cp ~/Downloads/001_1.jpg ~/Documents/glossary-app/images/
cp ~/Downloads/001_1.jpg ~/Documents/glossary-app/docs/images/
```
