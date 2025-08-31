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
- アンケート管理: 一覧/名称編集/新規/複製/削除/インポート/エクスポート、GUI/JSON編集で設定変更
- 収集案件管理: 作成/複製/削除/保存、状態（draft/active/closed）、期間（開始/終了）、対象アンケート紐付け、回答のJSON/CSV書き出し、全削除
- 回答リンク生成: 案件の「リンクコピー」で`index.html?project=<案件ID>`をコピー
- ストレージキー
  - アンケート一覧: `surveyIndexV1`
  - アクティブID: `activeSurveyIdV1`
  - 設定: `surveyConfigV1:<id>`
  - 回答（案件）: `surveySubmissionsV1:project:<id>`
  - 回答（案件なし）: `surveySubmissionsV1:survey:<id>`
  - 案件一覧: `surveyProjectsV1`
  
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
