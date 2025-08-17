---
title: VS Code + Node.js + Gemini CLI Education Foundation Guide
description: '---'
date: '2025-07-11'
author: Tiky Kobayashi
category: research
subcategory: ai-llm-studies
tags:
  - research
  - maya-programming
  - unity-development
  - python
  - javascript
  - user-interface
  - artificial-intelligence
  - claude-ai
  - tutorial
  - guide
license: MIT
melon_id: 356
version: '2.1'
---

# VS Code + Node.js + Gemini CLI 教育基盤セットアップガイド

**対象者**: プログラミング初学者〜中級学生
**前提条件**: Windows 10/11、基本的なPC操作可能
**目標時間**: 30分でAI支援開発環境完成
**教育効果**: Maya Python・Unity C#開発の現代化、AI協調学習体験

---

## 1. 環境構築ロードマップ

### 🎯 完成イメージ

```
VS Code (統合開発環境)
├── Node.js (JavaScript実行環境)
├── Gemini CLI (AI支援ツール)
├── Maya Python統合 (MEL/Python開発)
├── Unity C#統合 (ゲーム開発)
└── セッション継続機能 (学習成果保持)
```

### 📋 必要ソフトウェア一覧

| ソフトウェア | バージョン | 用途                    | 取得先                                               |
| ------------ | ---------- | ----------------------- | ---------------------------------------------------- |
| VS Code      | 最新版     | 統合開発環境            | [code.visualstudio.com](https://code.visualstudio.com/) |
| Node.js      | v20.x LTS  | JavaScript実行・npm管理 | [nodejs.org](https://nodejs.org/)                       |
| Gemini CLI   | v0.1.9+    | AI支援機能              | npm経由インストール                                  |

### ⏱️ 構築時間の目安（改訂版）

- **VS Code確認**: 5分
- **Node.js環境**: 15-20分（初回インストール + 再起動）
- **Gemini CLI**: 15分（API設定込み）
- **統合設定**: 10分（settings.json + GEMINI.md）
- **動作テスト**: 10分
- **🎯 総所要時間**: **55-70分**（初学者の場合）

---

## 2. VS Code環境確認（ミニマム版）

### ✅ インストール確認

```bash
# コマンドプロンプトで確認
code --version
```

### 📚 詳細設定参照

VS Codeの詳細インストール・設定手順は以下を参照：

- **324-vscode-github-copilot-complete-utilization-guide.md** (Copilot設定)
- **142-reference-for-integrating-vscode-with-maya.md** (Maya統合)

### 🔧 最小限の初期設定

1. **日本語化** (必要に応じて)
2. **基本テーマ選択**
3. **フォルダアクセス確認**

---

## 3. Node.js環境構築

### 📥 LTS版インストール

1. [nodejs.org](https://nodejs.org/) から **LTS版** をダウンロード
2. インストーラーを実行（デフォルト設定推奨）
3. 再起動（必要に応じて）

### ✅ インストール確認

```bash
# バージョン確認
node -v        # v20.x.x と表示されることを確認
npm -v         # v10.x.x と表示されることを確認

# 基本動作テスト
node -e "console.log('Hello Node.js!')"
```

### 🔧 PATH設定確認（通常は自動完了）

Node.jsインストーラーは**通常、PATH設定を自動で行います**。
以下は設定が正しく完了したかの確認手順です：

```bash
# Windows環境でのPATH確認
echo %PATH% | findstr nodejs

# Mac環境でのPATH確認
echo $PATH | grep node

# ✅ 正常な場合: Nodeインストールパスが表示される
# ❌ 問題がある場合: 何も表示されない → 手動設定が必要
```

#### 手動設定が必要な場合（稀なケース）

**Windows**:

```bash
# システム → 詳細設定 → 環境変数 → PATH編集
# または PowerShellで確認
$env:PATH -split ';' | Select-String node
```

**Mac**:

```bash
# ~/.zshrc または ~/.bash_profile に追加
export PATH="/usr/local/bin:$PATH"
source ~/.zshrc  # 設定反映
```

---

## 4. 安全な作業場所（ワークスペース）の準備

**このステップは非常に重要です。** プログラミング学習を安全に進めるため、必ず守ってください。

### ⚠️ なぜ専用フォルダが必要か？

PCのルートディレクトリ（C:ドライブなど）やデスクトップ、ドキュメントフォルダ直下で作業すると、以下のような危険があります。

- **OSの重要ファイルを誤って変更・削除するリスク**
- **作成したファイルが散乱し、管理できなくなる**
- **AIツールが意図せず重要なファイルを読み書きしてしまう事故**

これらのリスクを避けるため、作業は必ず**専用に作成したフォルダ内**で行います。この専用フォルダを「**ワークスペース**」と呼びます。

### 📂 ワークスペースの作成と開き方

#### 1. 学習用の親フォルダを作成する

まず、すべての学習プロジェクトをまとめるための親フォルダを作成します。（例: `C:\Users\YourUser\Documents\MyProjects`）

#### 2. 今回のプロジェクト用フォルダを作成する

次に、親フォルダの中に、今回のチュートリアル専用のフォルダを作成します。
（例: `C:\Users\YourUser\Documents\MyProjects\gemini-tutorial`）

#### 3. VS Codeでプロジェクトフォルダを開く

このプロジェクトフォルダをVS Codeで開くことで、そこが作業範囲（ワークスペース）になります。開き方は複数あります。

**方法A: コマンドプロンプトから（推奨）**

```bash
# 1. コマンドプロンプトでプロジェクトフォルダに移動する
cd C:\Users\◯◯◯\Documents\MyProjects\gemini-tutorial

# 2. VS Codeで現在のフォルダを開く
code .
```

**方法B: VS Codeのメニューから**

1. VS Codeを起動します。
2. `ファイル` > `フォルダーを開く...` を選択します。
3. 作成したプロジェクトフォルダ（`gemini-tutorial`）を選んで開きます。

**方法C: 最近使用した項目から**
一度開いたフォルダは、`ファイル` > `最近使用した項目を開く...` から素早くアクセスできます。

### 💡 ワークスペース活用のヒント

- **プロジェクトごとにフォルダを分ける**: チュートリアル、学校の課題、個人の作品など、目的ごとに必ずフォルダを分けましょう。
- **初心者は細かく分ける**: 慣れないうちは、機能ごとや日付ごとなど、細かくフォルダを分けて作業すると、万が一の事故が起きても影響範囲を最小限に抑えられます。下の階層は基本的に安全に制御できるため、整理整頓を心がけましょう。

**これ以降の作業は、すべてこの `gemini-tutorial` ワークスペース内で行います。**

---

## 5. Gemini CLI セットアップ

### 📥 Gemini CLI インストール

```bash
# グローバルインストール（複数のワークスペースからgeminiを使用したいのでグローバル奨励）
npm install -g @google/gemini-cli

# インストール確認
gemini --version
gemini -v # 意味は同じです
```

### 🔐 Mac環境でのsudo権限について

**Mac環境では管理者権限（sudo）が必要な場合があります：**

```bash
# 権限エラーが発生した場合
sudo npm install -g @google/gemini-cli

# パスワード入力が求められます（ログイン時のパスワード・半角入力を忘れずに）
Password: [パスワードを入力] # 入力中は文字が表示されません
```

**⚠半角入力を忘れずに**

#### 権限が必要になるケース

- **/usr/local/** への書き込み制限
- **Node.js**がシステム領域にインストールされている場合
- **学校管理PC**での制限設定

#### 💡 パスワードがわからないときの回避方法（推奨）

```bash
# 権限が不要でも書き込める領域の作成とパスの設定のうえでインストール
npm config set prefix ~/.npm-global
export PATH=~/.npm-global/bin:$PATH
npm install -g @google/gemini-cli
```

### 🔐 API認証設定

**初回起動時の認証手順：**

```bash
# APIキーが未設定の場合、最初にgeminiコマンドを実行すると認証が始まります
gemini

# 以下のような認証方法の選択画面が表示されます
>1. Continue with a Google account
>2. enter an API key? [account/key]

# "account" を選択し、Enterキーを押します
# ブラウザが自動で開き、Googleログイン画面へ
# 1. Googleアカウントでログイン
# 2. Gemini CLIへのアクセス許可
# 3. 認証完了後、ターミナルに戻る
# 4. さまざまなアクセス許可が表示されますのでOKを押してください
```

#### 📧 推奨アカウント：個人Gmail

**🎯 個人Gmailアカウントの使用を強く推奨します：**

- **個人Gmail**: `yourname@gmail.com` ← **推奨**
- ~~学校アカウント~~: `studentid@osaka-geidai.info` ← 卒業後アクセス不可

#### 💡 個人アカウント推奨理由

```markdown
✅ 長期継続メリット:
- 卒業後もスキル・設定を継続利用
- プロフェッショナルキャリアでの継続性
- 学習履歴・プロジェクト履歴の保持

❌ 学校アカウントのリスク:
- 卒業と同時にアクセス不可
- 蓄積したセッション・設定の消失
- 就職活動・転職時のスキル証明困難
```

### ✅ 基本動作テスト

```bash
# 簡単なテスト
gemini -p "Node.jsについて30文字で説明"

# 正常な応答が返れば設定完了
```

---

## 6. gemini.md設定

### 🎯 gemini.mdの役割

- **プロジェクト固有のAI設定**
- **コンテキスト情報の永続化**
- **チーム共有設定の統一**
- **セッション継続のためのメタデータ**

### 📍 配置場所

```
プロジェクトルート: ./GEMINI.md           # プロジェクト専用
ワークスペース: .vscode/GEMINI.md         # VS Code統合
ユーザーホーム: ~/.gemini/GEMINI.md        # グローバル設定
```

### 📝 GEMINI.md生成の実践的アプローチ

**🎯 手動記述せず、Gemini CLIに生成させる：**

```bash
# GEMINI.mdをAIに生成してもらう
gemini -p "VS Code + Maya Python開発環境用のGEMINI.mdを作成してください。
プロジェクト概要、AI協調ルール、セッション継続設定を含めて。"

# 出力をファイルに保存
gemini -p "GEMINI.md設定ファイルを生成" > GEMINI.md
```

#### 💡 複数AI活用の戦略的メリット

**🔄 分散質問による効率化：**

```markdown
✅ 実用的な使い分け例:
- Gemini CLI: 技術設定・コード生成
- GitHub Copilot: リアルタイムコード補完  
- Claude Web: 長文分析・レビュー
- ChatGPT: アイデア発想・企画

🎯 異なる視点での検証:
- 1つのAIで生成 → 別のAIでレビュー
- 複数のAIで同じ問題へ異なるアプローチ
- 行き詰まった際の別解法発見
```

---

## 7. settings.json設定

### 🎯 settings.jsonの役割

- **VS Code × Gemini CLI統合設定**
- **ショートカット・自動化設定**
- **セキュリティ制約の実装**
- **作業効率化のカスタマイズ**

### 📍 設定レベル

```
ユーザー設定: ~/.vscode/settings.json      # 全プロジェクト共通
ワークスペース: .vscode/settings.json      # ワークスペース専用
プロジェクト: project/.vscode/settings.json # プロジェクト専用
```

### 🔧 settings.json生成の実践的アプローチ

**🎯 AIに最適な設定を生成してもらう：**

```bash
# VS Code設定をAIに生成してもらう
gemini -p "VS Code + Gemini CLI統合用のsettings.jsonを作成してください。
Maya Python開発、セキュリティ制約、ワークフロー最適化を含めて。"

# 特定項目の追加生成
gemini -p "settings.jsonにUnity C#開発用の設定を追加してください"
```

#### 🎨 複数AIでの設定最適化例

```markdown
📋 効率的な設定構築手順:

1. **Gemini CLI**: 基本設定ファイル生成
   → "Maya開発用settings.json作成"

2. **GitHub Copilot**: VS Code内でリアルタイム補完
   → 入力中の設定項目自動補完

3. **Claude Web**: 設定内容の詳細レビュー
   → "この設定の問題点と改善案を教えて"

4. **ChatGPT**: 別アプローチの提案
   → "より効率的な設定方法はありますか？"
```

---

## 8. 重要なセキュリティ・注意事項

### 🔒 必須セキュリティチェック

#### APIキー管理

```markdown
❌ settings.jsonに平文でAPIキー記述禁止
✅ 環境変数またはSecure Storageを使用
✅ .gitignoreでAPIキー含有ファイル除外
```

#### ファイルアクセス制限

```markdown
❌ フルシステムアクセス許可禁止
✅ プロジェクトディレクトリ内に制限
✅ 機密ディレクトリの明示的除外
```

### 🚨 公的書類偽造防止（重要）

```markdown
⚠️ 【厳重警告】以下の依頼は犯罪行為です

❌ 絶対禁止事項:
- 遅延証明書の日付変更・作成
- 診断書・医療証明書の作成
- 成績証明書・在学証明書の偽造
- 収入証明書・住民票等の公的書類作成
- その他、証明書類の偽造・改変

🏛️ 法的責任:
- 公文書偽造罪: 1-10年の懲役
- 私文書偽造罪: 3か月-5年の懲役
- 学則違反: 退学・停学処分の可能性
```

### 📚 学術的誠実性

```markdown
## 適切な使用例
✅ 「Pythonでリストをソートする方法」
✅ 「このエラーメッセージの一般的な原因」  
✅ 「Mayaでカメラアニメーションを作る手順」

## 不適切な使用例
❌ 課題の直接的な解答要求
❌ 他学生のコードそのまま送信
❌ 完成品の一括生成依頼
```

### 💾 ディスク管理

```markdown
## 容量制限・監視
- プロジェクトサイズ上限: 500MB
- 警告閾値: 300MB
- 自動クリーンアップ: 有効化推奨

## 肥大化対策
- node_modules定期削除
- ログファイル自動ローテーション
- キャッシュサイズ制限
```

---

## 9. セッション継続機能

### 💾 基本的なセッション管理

```bash
# 重要なセッションの手動保存
gemini --save-session "maya-animation-project"

# セッション一覧確認
gemini --list-sessions

# 保存済みセッション読み込み
gemini --load-session "maya-animation-project"
```

### 🔄 自動コンテキスト継続

```json
// .vscode/settings.json
{
  "gemini.session": {
    "autoSave": true,
    "contextDepth": 10,        // 直近10回のやり取り保持
    "projectMemory": true,     // プロジェクト固有情報記憶
    "maxStorageSize": "50MB"   // セッションデータ上限
  }
}
```

### 📝 効果的なセッション活用

```markdown
## 保存すべきセッション
✅ 複雑な問題の解決過程
✅ 段階的な学習プロセス  
✅ プロジェクト固有の設定・ルール
✅ エラー解決の成功パターン

## 命名規則推奨
maya-rigging-week3
unity-physics-optimization
csharp-debugging-session
```

---

## 10. 設定適用の優先順位

### ⚙️ 設定ファイル優先度（高→低）

```markdown
1. プロジェクト設定: project/.vscode/settings.json
2. ワークスペース設定: .vscode/settings.json
3. ユーザー設定: ~/.vscode/settings.json
4. システムデフォルト: VS Code標準設定
```

### 📄 gemini.md優先度（高→低）

```markdown
1. プロジェクトルート: ./GEMINI.md
2. ワークスペース: .vscode/GEMINI.md  
3. ユーザーホーム: ~/.gemini/GEMINI.md
```

### 🔄 設定継承ルール

```markdown
- 上位設定が下位設定を上書き
- 配列設定は完全置換（マージなし）
- オブジェクト設定は部分マージ
- 明示的な null 設定で無効化可能
```

---

## 11. コンテンツの整理とメンテナンス

**方針転換**: 近年のAIは非常に大きなコンテキストウィンドウ（一度に読み込める情報量）を持つため、ファイルを細かく分離するメリットは薄れました。むしろ、一つのファイルに必要な情報がまとまっている方が、AIは全体像を把握しやすく効率的です。

そのため、「分離」よりも「**整理**」を重視し、`GEMINI.md`や設定ファイルを常にクリーンで見通しよく保つことを目指します。

### 🧹 定期的なメンテナンス方針

プロジェクトの区切りや、月に一度など、定期的に以下の内容を見直すことを強く推奨します。

- **要約化**: 長くなった議論や解決済みの問題は、結論や要点だけを残して簡潔にまとめます。過程は削除しても構いません。
- **アーカイブ化**: 完全に完了したタスクや、古くなった情報は、`GEMINI.md`から削除するか、別途 `archive.md` のようなファイルに移して参照リンクだけを残すようにします。
- **構造化**: 見出しを適切に使い、関連する情報がまとまるように順序を入れ替えます。古くなった情報はファイルの末尾に移動させると見通しが良くなります。
- **重複の削除**: 同じような内容が複数箇所に書かれていないか確認し、情報を一元化します。

このメンテナンスを習慣づけることで、AIは常に最新で的確なコンテキストを把握でき、パフォーマンスの低下や意図しない動作を防ぐことができます。

---

## 12. 統合テスト・動作確認

### ✅ 基本動作チェックリスト

```bash
# 1. Node.js動作確認
node -e "console.log('Node.js OK')"

# 2. Gemini CLI動作確認  
gemini -p "テスト: 今日の日付を教えて"

# 3. VS Code統合確認
code --version

# 4. プロジェクト作成テスト
mkdir test-project
cd test-project
echo '# Test Project' > GEMINI.md
code .
```

### 🎯 実践的な統合テスト

```markdown
## Maya Python連携テスト（次回詳細解説）
1. Maya起動・Command Port有効化
2. VS Codeからスクリプト送信テスト
3. デバッガ接続確認

## Unity C#連携テスト（次回詳細解説）
1. Unity Hub + VS Code統合確認
2. C# Dev Kit動作テスト
3. デバッガ・IntelliSense確認
```

---

## 13. トラブルシューティング

### 🚨 よくある問題と解決策

#### Node.jsインストール問題

```bash
# PATH設定の確認・修正
echo %PATH% | findstr nodejs

# 管理者権限でインストール再実行
# または手動でPATH追加
```

#### Gemini CLI認証エラー

```bash
# APIキー再設定
setx GEMINI_API_KEY "new-api-key"

# 環境変数確認
echo %GEMINI_API_KEY%

# CLI再インストール
npm uninstall -g @google/gemini-cli
npm install -g @google/gemini-cli
```

#### Windows環境特有の問題

```bash
# 文字化け問題
chcp 65001  # UTF-8設定

# PATH設定が反映されない
# → 環境変数手動追加
# システム → 詳細設定 → 環境変数

# ファイアウォール・プロキシ問題
# → 学校ネットワーク管理者に相談
```

#### VS Code統合問題

```json
// settings.json設定リセット
{
  "gemini.integration": {
    "enabled": false
  }
}
// 段階的に設定を有効化
```

### 📞 サポート体制

```markdown
## 段階的エスカレーション

### レベル1: 自己解決（5分）
- 公式FAQ確認
- 再起動・再実行

### レベル2: 学生サポート（10分）  
- クラスメート相談
- Slack #vscode-support

### レベル3: 教員サポート（授業時間内）
- 授業中: 挙手・直接質問
- 授業外: メール・Office Hours

### レベル4: システム管理者（24時間以内）
- 大規模障害・セキュリティ問題
```

---

**作成日**: 2025-07-11
**最終更新**: 2025-07-31
**対象**: プログラミング教育（初級〜中級）