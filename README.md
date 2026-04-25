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
