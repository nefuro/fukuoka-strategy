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

#### 1. 当選者データの確認

[team-mir.ai](https://team-mir.ai/) の選挙結果ページを確認:
- 衆院選: `https://team-mir.ai/election/shugiin-YYYY/result`
- 参院選: `https://team-mir.ai/election/sangiin-YYYY/result`

各当選者の **名前・ブロック（衆院）or 選挙区（参院）・当選経路・新人/元職** を確認します。

#### 2. `data.js` の `ELECTED_MEMBERS` に行を追加

```js
// 例: 次の参院選で福岡県選挙区から当選者が出た場合
{ election: "参院R10.7", name: "候補者名", bloc: null, prefecture: "福岡", status: "新人", type: "選挙区" },

// 例: 次の参院選で比例から当選者が出た場合
{ election: "参院R10.7", name: "候補者名", bloc: null, prefecture: null, status: "新人", type: "比例" },

// 例: 次の衆院選で小選挙区当選者が出た場合
{ election: "衆院R12.X", name: "候補者名", bloc: "九州ブロック", status: "前職", type: "選挙区", district: "福岡1区" },
```

#### 3. `CONFIG` の選挙回次を更新

新しい選挙のデータを概況タブにも反映する場合は `CONFIG` の以下も更新:

```js
houseElection: "R12.X",   // 新しい衆院選の場合
senateElection: "R10.7",  // 新しい参院選の場合
teamVotesHouse: ...,      // 新しい得票数
teamRateHouse: ...,       // 新しい得票率
```

#### 4. コミット＆push

```bash
git add data.js
git commit -m "feat: ELECTED_MEMBERS に参院R10.7の当選者を追加"
git push
```

**注意:** `ELECTED_MEMBERS` は全ブロック・全選挙共通データです。本家 kanagawa-strategy と同じデータを使うので、本家に PR を出すか、本家の更新を `git merge upstream/main` で取り込む形が理想的です。

## 🔀 kanagawa-strategy との運用の違い

| | kanagawa-strategy | fukuoka-strategy |
|---|---|---|
| ポスティングデータ | HTML内にJSON直書き | [action.team-mir.ai](https://action.team-mir.ai/) から抽出 → 逆ジオコーディング → CSV化（手動・都度更新） |
| 演説データ | Google Apps Script でスプレッドシートからライブ取得 | 未対応（福岡版のスプレッドシート・Apps Script が未構築） |
| 地図 | Datawrapper で作成した画像を埋め込み | action.team-mir.ai へのリンク（Datawrapper は未作成） |
| データ更新 | HTML を直接編集してコミット | `data/posting.csv` と `data.js` を更新してコミット |

### ポスティングセクション（#posting）の掲載方針

- **合計配布枚数**: 手動集計の正確な値（約57,021枚）を掲載
- **散布図**: Computer Use で取得した参考データを使用（注記付き）。市区町村別の相対的な傾向把握が目的
- **配布枚数ランキング・全エリア一覧**: データ精度の問題により**非表示**
- action.team-mir.ai から自動取得したデータには以下の誤差がある:
  - 矩形フィルタにより佐賀・大分など隣接県のデータが混入（最大20%程度過大）
  - 逆ジオコーディングの精度限界
  - 衆院選期間のみのフィルタ不可（累計値使用）

### 注意点

- action.team-mir.ai に公開APIがないため、更新のたびにログイン → Leafletマーカー手動抽出 → 国土地理院逆ジオコーダで市区町村変換が必要

## 📍 ポスティングデータの取得手順（Claude Code + Computer Use）

action.team-mir.ai はログイン必須・公開APIなしのため、**Claude Desktop の Computer Use** でブラウザ操作してデータを抽出し、**Claude Code** で加工・反映するワークフローで運用しています。

### 前提

- action.team-mir.ai は Next.js Server Components 構成で、クライアント側 API 呼び出しは発生しない
- ポスティングデータは Leaflet マーカーとして DOM 上に展開される（lat / lng / 枚数）
- マーカーに市区町村名は含まれないため、逆ジオコーディングが必要

### Step 1: Claude Desktop（Computer Use）でデータ抽出

以下のプロンプトを Claude Desktop に渡してブラウザ操作を依頼:

```
1. ブラウザで https://action.team-mir.ai/sign-in を開きログイン
2. https://action.team-mir.ai/map/posting に移動
3. DevTools Console で以下を実行し、全マーカーの lat/lng/枚数 を JSON で取得:

const orig = L.Marker.prototype._setPos;
L.Marker.prototype._setPos = function(p) {
  if (this._map) window.__theMap = this._map;
  return orig.call(this, p);
};
window.dispatchEvent(new Event('resize'));
// 3秒待ってから実行:
const m = window.__theMap;
const records = [];
for (const k in m._layers) {
  const l = m._layers[k];
  if (l._latlng && l._icon) {
    const t = l._icon.textContent;
    if (t && /枚/.test(t)) {
      records.push({
        lat: l._latlng.lat,
        lng: l._latlng.lng,
        count: parseInt(t.replace('枚','').trim(), 10)
      });
    }
  }
}

4. 福岡県の範囲（lat 33.0-34.0, lng 129.9-131.2）でフィルタ
5. 結果を JSON ファイルとして保存
```

### Step 2: Claude Code で逆ジオコーディング → CSV 化

抽出した JSON を Claude Code に渡して以下を依頼:

```
1. 各レコードの lat/lng を国土地理院逆ジオコーダで市区町村に変換
   API: https://mreversegeocoder.gsi.go.jp/reverse-geocoder/LonLatToAddress?lat={lat}&lon={lng}
   （無料・認証不要）
2. muniCd（自治体コード）で市区町村別に集計
3. data/posting.csv と data.js の FALLBACK_POSTING を更新
```

### Step 3: 確認 → コミット

```bash
python3 -m http.server 8000  # ローカルプレビュー
# http://localhost:8000/#posting で確認
git add data/posting.csv data.js
git commit -m "feat: ポスティングデータを更新（YYYY/MM/DD時点）"
git push
```

### 他県で Fork した場合

Step 1 の緯度経度フィルタを対象県に変更するだけで同じ手順が使えます。

## 🔄 upstream との同期

本家に共通ロジックの更新があった場合:

```bash
git remote add upstream https://github.com/mirai-shonan/kanagawa-strategy.git
git fetch upstream
git merge upstream/main
```

## 📝 更新・貢献

- **福岡固有の修正・改善** → このリポジトリの Issues または Pull Requests からお願いします
- **共通ロジック（UI/JS/CSS）の改善** → 本家 [kanagawa-strategy](https://github.com/mirai-shonan/kanagawa-strategy) への PR はリポジトリ管理者（[@nefuro](https://github.com/nefuro)）が [nefuro/kanagawa-strategy](https://github.com/nefuro/kanagawa-strategy) 経由で行います。本家に上げたい変更がある場合は Issue で相談してください
