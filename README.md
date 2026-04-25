# チームみらい 福岡活動戦略

> **このリポジトリは [mirai-shonan/kanagawa-strategy](https://github.com/mirai-shonan/kanagawa-strategy) の Fork です。**
> 共通のUI/ロジックや運用手順は本家を参照してください。

チームみらいの福岡県内での活動戦略をデータに基づいてまとめたページです。

## 📊 公開ページ

👉 https://nefuro.github.io/fukuoka-strategy/

## 概要

令和8年2月衆院選・令和7年7月参院選の福岡県開票データをもとに、
市区町村別の得票率・票数を分析し、街頭活動・ポスティングの戦略をまとめています。

## 📁 内容

- **データ分析**：概況・全自治体一覧・衆参比較・当選議員
- **戦略**：コスパ優先エリア・伸びしろエリア・イベントカレンダー
- **ターゲット別**：学術・若者 / 子育て / 祖父母世代

## 📂 データ出典

- [福岡県選挙管理委員会 衆院比例 R8.2.8執行](https://www.pref.fukuoka.lg.jp/contents/51senkyo.html)
- [福岡県選挙管理委員会 参院比例 R7.7.20執行](https://www.pref.fukuoka.lg.jp/contents/27sangisenkyo.html)
- ポスティングデータ：[action.team-mir.ai](https://action.team-mir.ai/)（2026/04/25時点）

## 🏛️ 当選議員セクションの運用

当選議員セクション（`#elected`）は `data.js` の `ELECTED_MEMBERS` 定数から動的にレンダリングされます。

### データ構造

```js
{ election: "衆院R8.2", name: "古川あおい", bloc: "九州ブロック", status: "新人", type: "比例単独" }
{ election: "参院R7.7", name: "安野たかひろ", bloc: null, prefecture: null, status: "新人", type: "比例" }
```

| フィールド | 説明 |
|---|---|
| `election` | `CONFIG.houseElection` / `senateElection` と一致させる（例: `"衆院R8.2"`） |
| `bloc` | 衆院の比例ブロック（参院比例の場合は `null`） |
| `prefecture` | 参院選挙区の都道府県名（衆院・参院比例の場合は省略） |
| `type` | `"比例単独"` / `"比例復活"` / `"選挙区"` / `"比例"` |
| `district` | 比例復活の場合の小選挙区名（例: `"東京7区"`） |

### 表示ロジック

- **衆院**: `CONFIG.bloc` に一致するブロックの当選者をハイライト
- **参院選挙区**: `CONFIG.prefectureName` に一致する県の当選者をハイライト
- **参院比例**: 全国区のため全県で表示
- 該当者がいない場合は「次回の目標」メッセージを自動表示

### Fork 時の設定

`CONFIG.bloc` と `CONFIG.prefectureName` を変更するだけで自動対応。`ELECTED_MEMBERS` は全ブロック・全選挙共通。

### 新しい選挙が行われた場合

`ELECTED_MEMBERS` に行を追加するだけ（出典: [team-mir.ai](https://team-mir.ai/)）。

## 🔀 kanagawa-strategy との運用の違い

| | kanagawa-strategy | fukuoka-strategy |
|---|---|---|
| ポスティングデータ | HTML内にJSON直書き | [action.team-mir.ai](https://action.team-mir.ai/) から抽出 → 逆ジオコーディング → CSV化（手動・都度更新） |
| 演説データ | Google Apps Script でスプレッドシートからライブ取得 | 未対応（福岡版のスプレッドシート・Apps Script が未構築） |
| 地図 | Datawrapper で作成した画像を埋め込み | action.team-mir.ai へのリンク（Datawrapper は未作成） |
| データ更新 | HTML を直接編集してコミット | `data/posting.csv` と `data.js` を更新してコミット |

### 注意点

- ポスティングデータは action.team-mir.ai の「2026年1月〜」イベントの累計値を使用。衆院選期間（〜2/7）のみのフィルタができないため選挙期間外のデータも含む（サイト上にも注記済み）
- action.team-mir.ai に公開APIがないため、更新のたびにログイン → Leafletマーカー手動抽出 → 国土地理院逆ジオコーダで市区町村変換が必要

## 🔄 upstream との同期

本家に共通ロジックの更新があった場合:

```bash
git remote add upstream https://github.com/mirai-shonan/kanagawa-strategy.git
git fetch upstream
git merge upstream/main
```

## 📝 更新・貢献

- **共通ロジック（UI/JS/CSS）の改善** → 本家 [kanagawa-strategy](https://github.com/mirai-shonan/kanagawa-strategy) に PR を送ってください
- **福岡固有の修正・改善** → このリポジトリの Issues または Pull Requests からお願いします
