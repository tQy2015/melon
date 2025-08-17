---
title: GitHub Education Application Manual for Students
description: この無料版では、以下の制限があります：
date: '2025-08-17'
author: Tiky Kobayashi
category: misc
subcategory: random-thoughts
tags:
  - misc
  - artificial-intelligence
  - claude-ai
  - educational-resources
license: MIT
melon_id: 133
version: '1.0'
---

# GitHub Education 申請マニュアル（学生向け）

## はじめに：無料版と教育ライセンスについて

**重要なお知らせ：** 2024年12月以降、GitHub Copilotは無料版（GitHub Copilot Free）も提供されています。この無料版を利用するには、単にGitHubアカウントにログインしてVS CodeなIDEでGitHub Copilot拡張機能を設定するだけで、申請手続きや審査は必要ありません。

この無料版では、以下の制限があります：
- 月間2,000回のコード補完（平日使いに推定で80回/日）
- 月間50回のチャットリクエスト
- 基本的なAIモデルのみ利用可能（GPT-4o、Claude 3.5 Sonnet、Gemini Pro）

無料版は手軽に始められるメリットがありますが、学生の皆さんには、以下の理由から教育ライセンス（GitHub Student Developer Pack）の取得を強くお勧めします：

- **コード補完は無制限**: Copilotの強力なコード補完機能は、回数制限なく利用できます。
- **高度な機能には制限あり**: Copilot Chatなどの高度な機能（プレミアムリクエスト）には、**月間300回**という利用制限が設定されています。これは多くの場合、1日に10回程度の利用に相当します。
- **追加特典**: AWS/Azureクレジット、JetBrains IDEなど、100以上の開発ツールを無料で利用可能。

**【重要】Copilotの利用制限（クォータ）について**
この学生向けプランは非常に強力ですが、Chat機能などを多用すると、月の途中で利用制限に達し、機能が制限される可能性があります。

制限の詳細と、クォータを節約しながら最大限に活用するための戦略（代替ツールGemini CLIの導入など）については、以下のガイドで詳しく解説しています。申請前に必ずご一読ください。

**[GitHub Copilot Education版のクォータ問題と代替案](./205-github-copilot-education-quota-and-alternatives.md)**

このマニュアルでは、GitHub Student Developer Packの申請方法を解説します。手続きは必要ですが、得られるメリットは非常に大きく、本格的な開発を行う学生にとって価値あるリソースとなります。

## 必要なもの

1. 大学/専門学校発行のメールアドレス（～.ac.jpなど）
2. エデュケーション版の申請にはACドメインの学校メールアドレスで登録が必要です（卒業後は無効になるため卒業までに卒業後も使用できるメールアドレスへ変更しておきましょう）
3. 学生証などの在学証明書（デジタル画像）
4. GitHubアカウント（持っていない場合は新規作成）

## 1. GitHubアカウントの作成（既にお持ちの場合はスキップ）

1. [GitHub公式サイト](https://github.com)にアクセス
2. 「Sign up」ボタンをクリック
3. ユーザー名、メールアドレス、パスワードを入力
4. アカウント検証を完了
5. 無料プランを選択
6. 設定内容に回答し、アカウント作成を完了

## 2. GitHub Educationへの申請手順

1. [GitHub Education](https://education.github.com)にアクセス
2. 「Get benefits for students」ボタンをクリック
3. 先ほど作成したGitHubアカウントでログイン

### 学生情報の入力

4. 「Get student benefits」ボタンをクリック
5. 学校名を入力（日本語可）
6. メールアドレスは大学/専門学校発行のメールアドレス（～.ac.jpなど）を使用
   - 個人のGmailなどは承認されにくいので注意
7. 学校の情報（ウェブサイトURL、学生数、住所など）を入力
   - 例：
     - 学校サイト：https://www.hal.ac.jp/osaka
     - 教員メール例：kobayashi.takuya@hal.ac.jp
     - 学生メール例：ohs*****@ohs.hal.ac.jp
     - 学校種別：Higher-education: university, college
     - 学生数：1,000 - 5,000
     - 住所：3-3-1 Umeda, Kita-ku
     - 市区町村：Osaka
     - 国：Japan
     - 都道府県：Hyogo-ken

<img src="./assets/GHD_1.png" alt="GitHub Education申請画面の例" width="450">

8. 「How do you plan to use GitHub?」には学業での利用予定を英語で簡潔に記入
   - 例: "I will use GitHub for coding projects in my graphic design course and collaborate with classmates."

### 在学証明のアップロード

9. 「Upload proof of academic status」で学生証や在学証明書の画像をアップロード
   - **申請の通過には在学証明書の英訳文章添付が必須です**
   - 学生証の写真を"GitHub-Education-251.json"といっしょにChat-GPTへ投入し「翻訳をして」と依頼すれば翻訳版が生成されます
   - **個人情報保護のため、学籍番号以外の個人情報（生年月日など）は隠すことを推奨**
   - 学校名、学生の氏名、有効期限が確認できる必要があります

<img src="./assets/GHD_2.png" alt="GitHub Education在学証明アップロード画面の例" width="450">

10. 利用規約に同意し「Submit your information」をクリック

申請が完了すると、以下のような完了画面が表示されます：

<img src="./assets/GHD_3.png" alt="GitHub Education申請完了画面の例" width="450">

## 3. 審査と承認

- 審査には通常1〜7日程度かかります
- 承認されると登録したメールアドレスに通知が届きます
- 否認された場合は理由が記載されるので、修正して再申請してください

## 4. GitHub Copilotの有効化

承認後、以下の手順でCopilotを有効化します：

1. [GitHub Student Developer Pack](https://education.github.com/pack)にアクセス
2. 「Sign up for Student Developer Pack」をクリック

## 5. VS CodeとのCopilot連携

1. VS Codeを起動
2. 拡張機能タブ（左側のブロックアイコン）をクリック
3. 検索ボックスに「GitHub Copilot」と入力
4. GitHub Copilot拡張機能をインストール
5. VS Code右下の通知から、またはCtrl+Shift+P（macOSはCmd+Shift+P）でコマンドパレットを開き「GitHub Copilot: Sign In」を選択
6. ブラウザが開くので、GitHubアカウントでログイン
7. 認証が完了すると、VS CodeでCopilotが使用可能になります

## トラブルシューティング

1. **申請が否認された場合**：
   - 学校のメールアドレスを使用しているか確認
   - 在学証明書の撮影時に翻訳を印刷した書類が同時に撮影できているか確認

2. **Copilotが動作しない場合**：
   - VS Codeを再起動
   - GitHub Copilot拡張機能が最新か確認
   - GitHubへの再ログインを試す

## 注意事項

- Student Developer Packは通常1年間有効です（更新可能）
- 卒業後はEducation Packの更新ができなくなるため注意

## 参考リンク

- [GitHub Education 公式サイト](https://education.github.com)
- [GitHub Copilot 公式ドキュメント](https://docs.github.com/en/copilot)
- [VS Code GitHub Copilot 拡張機能](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot)

## 補足：GitHub Copilot Free vs 教育ライセンス比較

2024年12月以降、GitHub Copilotは一般ユーザーも無料プラン（GitHub Copilot Free）で利用できるようになりました。以下は教育ライセンスとの比較です：

| 機能/特徴 | GitHub Copilot Free | GitHub Student Developer Pack |
|---------|-------------------|---------------------------|
| **コード補完** | 月間2,000回まで | 無制限 (Copilot Pro相当) |
| **チャットリクエスト** | 月間50回まで | 無制限 (Copilot Pro相当) |
| **利用可能なAIモデル** | 基本モデルのみ<br>(GPT-4o, Claude 3.5 Sonnet, Gemini Pro) | 全モデル<br>(GPT-4o, Claude 3.5 Sonnet, o1, Gemini Pro等) |
| **必要条件** | GitHubアカウントのみ | 学生証明書と学校メールアドレス |
| **申請手続き** | 不要 | 必要 (審査あり) |
| **対応IDE** | VS Code, Visual Studio等 | VS Code, Visual Studio等 |
| **追加特典** | なし | 100以上の開発ツール・サービス<br>(AWS/Azure クレジット、JetBrains IDE等) |
| **Copilot Edits** | 制限あり (チャット回数に含まれる) | 無制限 |

教育ライセンスは申請手続きが必要ですが、Copilotの制限がなく、多数の追加特典を受けられるため、学生であれば取得する価値が高いでしょう。一方、GitHub Copilot Freeは審査なしで誰でもすぐに利用できるため、教育機関に所属していない方や、軽度の利用であれば十分な選択肢となります。