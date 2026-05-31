# iOSショートカット設定手順 — Apple Pay自動ログ

## 概要

Apple Payで払うたびにWalletの通知が来る
→ ショートカットが自動起動
→ Life Visualize に自動記録

---

## ステップ1: ショートカットアプリを開く

iPhone の「ショートカット」アプリ → 「オートメーション」タブ → 「+」

---

## ステップ2: トリガーを設定

「個人用オートメーションを作成」
→ 「通知」を選択
→ アプリ：「Wallet」を選択
→ 「次へ」

---

## ステップ3: アクションを追加

以下のアクションを順番に追加：

### アクション1: 通知内容を取得
- 「通知コンテンツを取得」を追加
- 「タイトル」を選択

### アクション2: テキストから金額を抽出
- 「テキストを照合」を追加
- 正規表現: `[¥￥]([0-9,]+)`
- 「最初の照合結果のグループ1」を変数 `金額` に保存

### アクション3: URLにPOST
- 「URLの内容を取得」を追加
- URL: `http://localhost:8000/api/lv/transactions`
- メソッド: POST
- ヘッダー: `Content-Type: application/json`
- ボディ（JSON）:
```json
{
  "amount": 金額,
  "name": "Apple Pay",
  "category": "その他",
  "source": "apple_pay"
}
```

---

## ステップ4: 確認なしで実行

「実行の前に確認」→ オフ にする

---

## 注意点

- localhost:8000 はMacとiPhoneが同じWi-Fiにいる時のみ動作
- MacのIPアドレスを使う場合: `http://192.168.1.XX:8000/api/lv/transactions`
  （システム環境設定→ネットワークでIPを確認）
- 外出先でも使いたい場合: 別途Cloudflare TunnelかTailscaleで対応予定

---

## テスト

ショートカットを手動実行して、Life Visualize の画面に取引が追加されればOK。
