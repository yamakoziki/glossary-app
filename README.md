# 多言語用語集アプリ（glossary-app）

任意の分野の専門用語を日本語・フランス語・英語の3言語で閲覧できるWebアプリです。

---

## 特徴

- Excelファイルで用語を一元管理
- AI翻訳機能（Anthropic Claude API）
- スマートフォン対応（モバイルファースト）
- 画像・写真の添付に対応（最大5枚/用語）
- オフライン閲覧対応（データをHTMLに埋め込み）
- 3言語対応（日本語・フランス語・英語）

---

## 画面構成

| 画面 | ファイル | 説明 |
|---|---|---|
| 管理画面 | `admin/admin.html` | 用語の管理・AI翻訳・HTML生成 |
| 公開画面 | `docs/index.html` | 用語集の閲覧（GitHub Pages で公開） |

---

## 使い方

詳しいセットアップ手順・日常運用・トラブル対処は [INSTALL_GUIDE.md](./INSTALL_GUIDE.md) を参照してください。

---

## ファイル構成

```
glossary-app/
├── README.md
├── INSTALL_GUIDE.md
├── data/
│   └── glossary.xlsx     # 用語集の元データ
├── images/               # 画像のバックアップ
├── admin/
│   └── admin.html        # 管理ツール
└── docs/
    ├── index.html        # 公開用Webサイト
    └── images/           # 公開用画像
```

---

## 動作環境

- Mac（macOS 12以降）
- Node.js
- モダンブラウザ（Chrome・Safari・Firefox）
