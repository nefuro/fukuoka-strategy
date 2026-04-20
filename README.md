# チームみらい 福岡活動戦略

> **このリポジトリは [mirai-shonan/kanagawa-strategy](https://github.com/mirai-shonan/kanagawa-strategy) の Fork です。**
> 共通のUI/ロジック（`app.js`, `style.css`）や運用手順・開発ガイドは本家を参照してください。

チームみらいの福岡県内での活動戦略をデータに基づいてまとめたページです。

## 公開ページ

https://nefuro.github.io/fukuoka-strategy/

## 概要

令和8年2月衆院選・令和7年7月参院選の福岡県開票データをもとに、
市区町村別の得票率・票数を分析し、街頭活動・ポスティングの戦略をまとめています。

## 内容

- **データ分析**：概況・全自治体一覧・衆参比較・当選議員
- **戦略**：コスパ優先エリア・伸びしろエリア・イベントカレンダー
- **ターゲット別**：学術・若者 / 子育て / 祖父母世代

## ファイル構成

```
index.html         # メインHTML（コンテンツ・タブ構造）
style.css          # CSS（デザイン・レイアウト）
app.js             # JSロジック（テーブル描画・グラフ・タブ切替等）
data.js            # CONFIG + フォールバックデータ + CSV読み込み
data-template.js   # 他県用データテンプレート（Fork時に使用）
data/              # 選挙データのCSV（自動生成）
├── house.csv      # 衆院比例
├── senate.csv     # 参院比例
├── candidate.csv  # 候補者別
└── posting.csv    # ポスティング（手動作成）
raw/               # 選管Excel/PDF（git管理）
├── R8.2_shugi_hirei.pdf
├── R7.7_sangi_hirei_district.xlsx
└── R7.7_sangi_senkyoku.xlsx
tools/
└── convert_excel.py  # 選管Excel/PDF → CSV変換スクリプト
README.md
```

## 🔗 タブとURL（Hash routing）

タブを切り替えると URL のハッシュが変わります。これにより:
- 特定タブのリンクを共有できる（例: `https://.../#growth`）
- ブラウザの戻る/進むでタブ遷移できる
- リロードしても同じタブが開いたままになる

| URL | タブ |
|---|---|
| `#overview` | 概況 |
| `#all` | 全自治体 |
| `#compare` | 衆参比較 |
| `#elected` | 当選議員 |
| `#cost` | コスパ |
| `#growth` | 伸びしろ |
| `#events` | イベント |
| `#posting` | ポスティング |
| `#speech` | 演説 |
| `#academic` | 学術・若者 |
| `#family` | 子育て |
| `#senior` | 祖父母世代 |

## セットアップ

### 必要パッケージ

```bash
pip install openpyxl pdfplumber
```

- `openpyxl` — Excelパース用
- `pdfplumber` — PDFパース用（衆院比例がPDFのみ公開の場合）

### ローカルプレビュー

CSV読み込みのため、`file://` で直接開くのではなく簡易サーバーが必要です:

```bash
python3 -m http.server 8000
```

ブラウザで http://localhost:8000/ を開きます。

---

## 🤖 運用手順・開発ガイド

**運用手順（Fork方式での横展開、選挙データ最新化、共通ロジック修正、戦略コンテンツ修正、データ整合性検証、トラブルシューティング等）は、すべて本家リポジトリの README を参照してください。**

👉 **[mirai-shonan/kanagawa-strategy — README（運用手順）](https://github.com/mirai-shonan/kanagawa-strategy#-運用手順claudeai向け実行ガイド)**

> **注意:** 共通ロジック（`app.js`, `style.css`）のバグ修正・機能追加は、まず本家 kanagawa-strategy で修正し、各 Fork に反映する運用を推奨します。

---

## 福岡固有の変更点

本家 kanagawa-strategy からの主な差分:

- `data.js` の `CONFIG` を福岡県のデータに差し替え
- `raw/` を福岡県選管のExcel/PDFに差し替え
- `data/` を福岡県のCSVに差し替え
- `index.html` の戦略コンテンツ（イベント・ターゲット別等）を福岡県向けに書き換え
- カラーテーマを変更

---

## データ出典

- [福岡県選挙管理委員会 衆院比例 R8.2.8執行](https://www.pref.fukuoka.lg.jp/contents/51senkyo.html)
- [福岡県選挙管理委員会 参院比例 R7.7.20執行](https://www.pref.fukuoka.lg.jp/contents/27sangisenkyo.html)

## upstream との同期

本家に共通ロジックの更新があった場合:

```bash
git remote add upstream https://github.com/mirai-shonan/kanagawa-strategy.git
git fetch upstream
git merge upstream/main
```

コンフリクトが発生した場合、`data.js`・`index.html`・`raw/`・`data/` は福岡固有なので **こちら側（ours）を優先** してください。`app.js`・`style.css` は本家の変更を取り込むのが基本です。

## 更新・貢献

- **共通ロジック（UI/JS/CSS）の改善** → 本家 [kanagawa-strategy](https://github.com/mirai-shonan/kanagawa-strategy) に PR を送ってください
- **福岡固有の修正・改善** → このリポジトリの Issues または Pull Requests からお願いします
