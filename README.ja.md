# vc-verifier

日本の新型コロナワクチン接種証明書アプリのQRコードのJWS署名を検証するコマンドラインツールです。

このプロジェクトは、日本政府公式の[新型コロナワクチン接種証明書アプリ](https://www.digital.go.jp/policies/posts/vaccinecert)によって生成されるQRコードを検証するために作成されました。これらのQRコードは、JSON Web Signature (JWS) を使用してデータをエンコードする国際的な[SMART Health Card](https://smarthealth.cards/en/)規格に準拠しています。

## 仕組み

スクリプトは以下の手順を実行します:
1.  `shc:/`プレフィックスで始まる生のQRコード文字列を入力として受け取ります。
2.  数値文字列を標準のJWS形式にデコードします。
3.  デジタル庁が提供する公式の発行者URLから公開鍵（JSON Web Key Set: JWKS）を取得します。
4.  取得した公開鍵を使用してJWS署名を検証します。
5.  zlib (DEFLATE) を使用してJWSペイロードを展開します。
6.  検証済みのJSONペイロード（ワクチン接種の詳細を含む）をコンソールに出力します。

## 前提条件

*   Node.js
*   Yarn

## インストール

```bash
yarn install
```

## 使い方

`"shc:/"`プレフィックスを含むQRコード文字列全体を、`verify`スクリプトの引数として渡してください。

```bash
yarn verify <your_qrcode_string>
```

成功すると、スクリプトはデコードおよび検証済みのJSONペイロードを出力します。

## 技術仕様

*   **規格:** [SMART Health Card Framework](https://spec.smarthealth.cards/)
*   **公開鍵エンドポイント (JWKS):** `https://vc.vrs.digital.go.jp/issuer/.well-known/jwks.json`
*   **主要な依存ライブラリ:**
    *   [node-jose](https://github.com/cisco/node-jose) - JWS署名の検証用
    *   [pako](https://github.com/nodeca/pako) - zlib DEFLATEの展開用

## ライセンス

MIT License
