# EspañolQuiz 開発チーム

スペイン語単語帳アプリ「EspañolQuiz」を作るチームの体制メモです。
Claude Code上では、以下の役割を [Agent](https://docs.claude.com/) のサブエージェントとして `.claude/agents/` に定義しています。担当分野のタスクを頼むと、その役割で応答します。

## 役割

| 役割 | サブエージェント | 主な担当 |
|---|---|---|
| フロントエンド実装 | `frontend-engineer` | `index.html` / `style.css` / `App.js` のUI・機能実装 |
| スペイン語コンテンツ監修 | `content-curator` | 単語・訳語・例文・カテゴリ・DELEレベルの正確性 |
| QA / 動作確認 | `qa-tester` | クイズロジック・CRUD・スプシ連携・成績・PWA動作の検証 |
| PWA / インフラ | `pwa-specialist` | `manifest.json` / `sw.js` / オフラインキャッシュ / スプシ取得の堅牢性 |
| スプレッドシート担当 | `sheets-manager` | DELE/マイ単語データを保持するGoogleスプレッドシートの内容整備・拡充データの作成 |

## アプリ概要
- 名称: EspañolQuiz（スペイン語単語マスター）
- 形態: ビルドツールなしのVanilla JS PWA（`index.html` / `style.css` / `app.js`）
- データソース: マイ単語（ローカル管理） / DELE（レベル別、Googleスプレッドシートの`DELE`タブで管理）
- 主な機能: 4択・スペル・例文の3クイズモード、es⇔ja両方向、Googleスプレッドシート連携、成績・streak管理、オフライン対応

## DELEデータセットの現状（2026-07-15時点）
- 合計1508語（A1: 785語 / A2: 723語）。**B1以上のレベルは未整備**
- スペイン語見出し語の表記ゆれによる重複が57件確認されている
- 拡充作業は `sheets-manager`（シート整備・重複チェック・貼り付け用データ作成）と `content-curator`（語彙・訳語・例文の正確性）が連携して進める

## 開発フロー（目安）
1. 機能要件が固まったら `content-curator` で単語・例文などコンテンツ面を先に確定
2. データをスプレッドシートに反映する場合は `sheets-manager` がフォーマット整備・重複チェック・貼り付け用データを用意
3. `frontend-engineer` がUI・ロジックを実装
4. PWA/キャッシュ/スプシ取得ロジックに関わる変更は `pwa-specialist` が担当
5. 実装完了後、`qa-tester` が動作確認してから完了報告
