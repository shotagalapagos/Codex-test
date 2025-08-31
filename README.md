# 簡単アンケート（Q1/Q2） + 管理者画面

Q1の回答に応じてQ2の質問文が変わる、最小構成のサンプルです。`index.html`をブラウザで開くだけで動きます。

見やすさ向上として、アクセントカラー（1色、既定は #4f8cff）を追加しています。

## 使い方（回答者）
- `index.html` を開く（案件URLの場合は `index.html?project=<案件ID>`）
- Q1で「知ったきっかけ」を選ぶ → Q2の文言が自動で変化
- 「送信する」で回答が保存されます（ローカル）

## コードのポイント（初心者向け）
- Q1の選択肢は`options`で管理（最大100）。管理モードから編集可
- Q2は形式を切り替え可能（`q2.type`）。対応: likert/single/multiple/matrix/short/long
- `updateQ2ByQ1()`がQ2タイトル/ヒントを更新し、`renderQ2()`でUIを再描画

## カスタマイズ
- 管理モードを使う: 画面上部の「管理モード」→ GUI編集フォーム もしくは JSON を編集 → 保存
- 直接編集する: `index.html` の `<script>` 内 `defaultConfig`
- 送信の動作を変える: `form.addEventListener('submit', ...)` を書き換え

## 注意
- OneDrive配下のため、Git操作時にファイルロックが起きる場合があります
- バリデーションはブラウザ標準（`required`）に依存しています

## 管理者画面（複数アンケート + 収集案件）
- 開く: `admin.html`
- アンケート管理: `admin-surveys.html`（一覧/新規/複製/削除/インポート/エクスポート/アクティブ切替）
- アンケート編集: `admin-survey-edit.html?id=<アンケートID>`（Q1選択肢、Q2のタイプ別設定、タイトルマップ/ヒント）
- 回答依頼案件管理: `admin-projects.html`（作成/編集/公開・停止/期間設定/対象アンケート紐付け）
- 回答データ管理: `admin-data.html`（案件IDまたはアンケートIDで検索し、JSON/CSVエクスポートや全削除）
- ユーザー管理（ダミー）: `admin-users.html`
- 回答リンク: `index.html?project=<案件ID>` を共有
- ストレージキー
  - アンケート一覧: `surveyIndexV1`
  - アクティブID: `activeSurveyIdV1`
  - 設定: `surveyConfigV1:<id>`
  - 回答（案件）: `surveySubmissionsV1:project:<id>`
  - 回答（案件なし）: `surveySubmissionsV1:survey:<id>`
  - 案件一覧: `surveyProjectsV1`

## 画面構成（フロー）
- トップ: `top.html`（ログイン種別選択）
- 判断: 管理者か？ → YES: 管理者画面（`admin.html`） / NO: ユーザー画面（`user.html`）
- 管理者画面から:
  - アンケート帳票管理 → 一覧（`admin-surveys.html`）→ 新規/編集（`admin-survey-edit.html`）
  - 回答依頼案件管理 → 案件の作成/公開（`admin-projects.html`）
  - 回答データ管理 → 集計/エクスポート（`admin-data.html`）
  - ユーザー管理（ダミー）
- ユーザー画面: 公開中の案件一覧から「回答する」→ `index.html?project=<案件ID>` で回答画面へ
  
### JSON例（形式切替）
  - Likert（段階数変更）
    {
      "options": ["SNS","検索"],
      "q2": {
        "type": "likert",
        "titleMap": {"default": "満足度を教えてください"},
        "hint": "1〜7で評価してください",
        "likert": {"points": 7, "minLabel": "低い", "maxLabel": "高い"}
      }
    }
  - 単一/複数（選択肢追加）
    {
      "options": ["SNS","検索"],
      "q2": {
        "type": "single",
        "titleMap": {"default": "最も当てはまるものを選択"},
        "single": {"options": ["A","B","C"]}
      }
    }
  - マトリックス（行・列）
    {
      "options": ["SNS","検索"],
      "q2": {
        "type": "matrix",
        "titleMap": {"default": "各項目について評価"},
        "matrix": {"rows": ["使いやすさ","デザイン"], "cols": ["不満","普通","満足"]}
      }
    }
  - 短文/長文（preset or pattern）
    {
      "options": ["SNS","検索"],
      "q2": {
        "type": "short",
        "titleMap": {"default": "数字のみで入力"},
        "short": {"preset": "numeric", "maxlength": 10, "placeholder": "半角数字"}
      }
    }
