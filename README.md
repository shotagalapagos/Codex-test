# 簡単アンケート（Q1とQ2のみ）

Q1の回答に応じてQ2の質問文が変わる、最小構成のサンプルです。`index.html`をブラウザで開くだけで動きます。

## 使い方
- `index.html` をダブルクリックで開く
- Q1で「知ったきっかけ」を選ぶ → Q2の文言が自動で変化
- 「送信する」で簡単な確認アラートを表示（学習用の最小挙動）

## コードのポイント（初心者向け）
- Q1の選択肢は`options`で管理（最大100）。管理モードから編集可
- Q2は形式を切り替え可能（`q2.type`）。対応: likert/single/multiple/matrix/short/long
- `updateQ2ByQ1()`がQ2タイトル/ヒントを更新し、`renderQ2()`でUIを再描画

## カスタマイズ
- 管理モードを使う: 画面上部の「管理モード」→JSONを編集→保存
- 直接編集する: `index.html` の `<script>` 内 `defaultConfig`
- 選択肢を増やす: `options`に項目を追加し、同じキーを`q2TextMap`に追加（未指定なら`default`を使用）
- 送信の動作を変える: `form.addEventListener('submit', ...)` を書き換え

## 注意
- OneDrive配下のため、Git操作時にファイルロックが起きる場合があります
- バリデーションはブラウザ標準（`required`）に依存しています

## 管理モードについて
- 保存した設定は、ブラウザのlocalStorage（キー: `surveyConfigV1`）に保管されます
- 「既定に戻す」でlocalStorageをクリアし、`defaultConfig`に戻ります
- JSON例（形式切替）:
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
