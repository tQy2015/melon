---
title: "AMP: Artifact Migration Protocol (ja)"
description: "LLMとのセッションにおける大規模アーティファクトとコンテキストを管理するための標準プロトコル"
date: "2025-03-23"
author: "TQY Kobayashi"
category: "protocol-specification"
tags: ["amp", "llm", "protocol", "context-management", "japanese"]
license: "MIT"
---
# AMP: Artifact Migration Protocol

**Artifact Migration Protocol (AMP)** はLLMとのセッションにおける大規模アーティファクトとコンテキストを管理するための標準プロトコルです。このプロトコルは、トークン制限のあるAIシステムでも長時間にわたる複雑な会話を維持し、生成された成果物を効率的に保存・再利用するために設計されています。

**最終更新日:** 2025年3月23日  
**Author:** TQY Kobayashi (竹の台地域委員会CXO, HAL大阪 専任教官, 大阪芸術大学デザイン学科 客員教授 & 八箱CDO https://tqy.yahaco.com)  
**Contributors:** Claude, Gemini, Deepseek, ChatGPT, Copilot

## 1. 目的と背景

AMP（Artifact Migration Protocol）は以下の課題を解決するために開発されました：

- **コンテキスト制限の克服**: 長時間のセッションでのトークン制限を管理
- **アーティファクト保全**: 生成された成果物（コード、文章など）の一貫性を維持
- **セッション継続性**: 会話の途切れを最小限に抑え、シームレスな対話を実現
- **クロスモデル互換性**: 様々なLLMで一貫した方法でのデータ維持を可能に

## 2. 仕様

### 2.1 基本構造

AMPはJSON形式で記述され、以下の主要コンポーネントから構成されます：

```json
{
  "command": "migrate",
  "version": "1.0",
  "settings": {
    "splitMode": "auto",
    "safetyMargin": 0.8,
    "thresholds": {
      "text": 2000,
      "numbers": 1500,
      "code": 1000,
      "mixed": 1800
    },
    "markers": {
      "prefix": "AMP-Session",
      "continuation": "AMP-Continue",
      "complete": "AMP-Complete"
    }
  },
  "artifacts": [
    {
      "id": "conversation-1",
      "type": "text/markdown",
      "title": "最初の会話",
      "content": "ここに最初の会話内容を記述します。",
      "checksum": "e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855"
    },
    {
      "id": "code-snippet-1",
      "type": "text/code",
      "title": "サンプルコード",
      "content": "console.log('Hello, World!');",
      "checksum": "7f83b1657ff1fc53b92dc18148a1d65dfc2d4b1fa3d677284addd200126d9069"
    }
  ],
  "context": {
    "sessionId": "amp-20250322-12345",
    "timestamp": "2025-03-22T17:55:42Z",
    "segmentCount": 3,
    "currentSegment": 1
  }
}
```

### 2.2 パラメータ説明

#### command
- `migrate`: 新しいセッションの開始または継続
- `continue`: セッションの続行
- `complete`: セッションの完了

#### settings
- `splitMode`: 分割方法
  - `auto`: システムが最適な分割を自動決定
  - `fixed`: 固定サイズでの分割
  - `adaptive`: データの複雑さに応じて動的に分割

- `safetyMargin`: トークン上限に対する安全マージン（0.0〜1.0）

- `thresholds`: データタイプ別の分割しきい値
  - `text`: 通常のテキスト（単語数）
  - `numbers`: 数値データ（アイテム数）
  - `code`: プログラムコード（行数）
  - `mixed`: 混合コンテンツ（推定トークン数）

- `markers`: セグメント識別用マーカー
  - `prefix`: セッション開始マーカー
  - `continuation`: 継続セグメントマーカー
  - `complete`: 完了マーカー

#### artifacts
アーティファクトの配列。各アーティファクトは以下を含む：
- `id`: 一意の識別子
- `type`: MIMEタイプ（例：text/markdown, application/json）
- `title`: 人間が読める形式のタイトル
- `content`: アーティファクトの実際の内容
- `checksum`: 整合性検証用のハッシュ値

#### context
- `sessionId`: セッションの一意識別子
- `timestamp`: ISO形式の日時
- `segmentCount`: 全セグメント数
- `currentSegment`: 現在のセグメント番号

## 3. 使用方法

### 3.1 セッション開始

新しいセッションを開始する場合：

```json
{
  "command": "migrate",
  "version": "1.0",
  "settings": {
    /* 設定情報 */
  },
  "artifacts": [
    /* 初期アーティファクト */
  ],
  "context": {
    "sessionId": "新規セッションID",
    "timestamp": "開始時刻",
    "segmentCount": 1,
    "currentSegment": 1
  }
}
```

### 3.2 セッション継続

分割されたセッションを継続する場合：

```json
{
  "command": "continue",
  "version": "1.0",
  "context": {
    "sessionId": "amp-20250322-12345",
    "timestamp": "2025-03-22T18:02:15Z",
    "segmentCount": 3,
    "currentSegment": 2
  },
  "previousSegment": {
    "checksum": "e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855"
  },
  "recoveryStrategy": {
    "onConnectionError": "retry",
    "maxRetries": 3,
    "retryIntervalMs": 1000,
    "fallbackMode": "localCache"
  }
}
```

### 3.3 セッション完了

セッションを完了する場合：

```json
{
  "command": "complete",
  "version": "1.0",
  "context": {
    "sessionId": "amp-20250322-12345",
    "timestamp": "2025-03-22T18:15:37Z",
    "segmentCount": 3,
    "currentSegment": 3
  },
  "summary": {
    "artifactsCount": 5,
    "totalSize": 28450,
    "totalTokens": 8129,
    "processingTimeMs": 12583
  }
}
```

## 4. 実装ガイドライン

### 4.1 分割戦略

複数の主要LLMでの実験により、データタイプやモデルによって最適な分割戦略が異なることが判明しています。詳細な実験結果は付録「複数AIモデル間のトークン枯渇特性比較」を参照してください。

主要モデルにおける推奨分割値：

| モデル | テキスト | 数値データ | コード | 混合 |
|--------|---------|-----------|-------|------|
| Claude | 200語   | 225アイテム | 600行  | 約600トークン |
| Deepseek | 250語 | 300アイテム | 850行 | 約900トークン |

### 4.2 トークン推定の考慮事項

- モデルにより1トークンあたりの文字数換算が異なる場合がある
- 数値や特殊文字はトークン消費量が異なる場合がある
- パターン化された内容は予想以上にトークンを消費する場合がある

### 4.3 セッション管理のベストプラクティス

- 各セグメントの開始と終了を明確にマークする
- チェックサムを使用して整合性を確認
- セグメント内で重要なコンテキスト情報を繰り返す
- 最終セグメントでは完全なアーティファクトリストを提供

### 4.4 チェックサム生成と検証

チェックサムはアーティファクトの整合性を保証するために重要です。推奨されるチェックサム生成方法はSHA-256ハッシュです。

```javascript
// JavaScriptでのチェックサム生成例
async function generateChecksum(content) {
  const encoder = new TextEncoder();
  const data = encoder.encode(content);
  const hashBuffer = await crypto.subtle.digest('SHA-256', data);
  const hashArray = Array.from(new Uint8Array(hashBuffer));
  const hashHex = hashArray.map(b => b.toString(16).padStart(2, '0')).join('');
  return hashHex;
}

// チェックサム検証例
async function verifyChecksum(content, expectedChecksum) {
  const actualChecksum = await generateChecksum(content);
  return actualChecksum === expectedChecksum;
}
```

#### Python実装例

```python
import hashlib

def generate_checksum(content):
    """文字列コンテンツからSHA-256チェックサムを生成"""
    if isinstance(content, str):
        content = content.encode('utf-8')
    return hashlib.sha256(content).hexdigest()

def verify_checksum(content, expected_checksum):
    """チェックサムが一致するか検証"""
    actual_checksum = generate_checksum(content)
    return actual_checksum == expected_checksum
```

### 4.5 エラー回復戦略

AMPプロトコルでは、通信エラーやセグメント欠損に対する回復戦略が重要です。以下に推奨される回復戦略を示します：

#### 4.5.1 通信エラー時の対応

1. **再試行戦略**:
   - 指数バックオフによる再試行（初回1秒、2回目2秒、3回目4秒...）
   - 最大再試行回数の設定（デフォルト: 3回）

2. **ローカルキャッシュの活用**:
   - 最後の成功レスポンスをローカルに保存
   - 通信障害時にキャッシュからコンテキストを復元

3. **部分的セッション再開**:
   - 最後に検証されたチェックサムポイントからセッションを再開
   - 失われたデータの推定と再構築

#### 4.5.2 セグメント欠損検出と対応

1. **欠損検出方法**:
   - セグメント番号の連続性チェック
   - チェックサムの検証によるデータ整合性確認

2. **欠損時の対応フロー**:
```
欠損検出 → 前セグメントから復元試行 → 
部分的に復元できた場合: 差分更新要求 → 
復元できない場合: 欠損セグメント再要求 → 
それでも失敗: 最終既知状態から再開
```

3. **セグメント間の重複情報の活用**:
   - 先頭/末尾の重複情報を利用して境界を推定
   - 重要なコンテキスト情報は複数セグメントに重複して保存

## 5. クロスモデル互換性

AMPは主要LLMで使用可能ですが、モデルごとに最適なパラメータ調整が必要な場合があります。現在テスト済みのモデル：

- Claude (Anthropic)
- Gemini (Google)
- Deepseek
- ChatGPT (OpenAI)

### 5.1 クロスモデル実装例

複数のAIモデルでAMPプロトコルが実装可能であることが確認されています。各モデルでの実装方法と特性は以下の通りです。

#### モデル別実装方法の違い

| モデル | 実装方法 | セッション保持 | 特記事項 |
|--------|---------|--------------|---------|
| **Claude** | ユーザープロファイル内へ人為的にAMPのJSONを記述可能 | プロファイル記述で永続化 | セッション間で状態維持が可能 |
| **ChatGPT** | 最初にAMPのJSONを明示するとユーザープロファイルへ自動格納 | 自動保存される | 初回提示後は参照・更新が可能 |
| **Gemini** | 新セッションごとにAMP JSONによる初期化が必要 | セッション限定 | 毎回初期化コマンドが必要 |
| **Deepseek** | 新セッションごとにAMP JSONによる初期化が必要 | セッション限定 | 毎回初期化コマンドが必要 |
| **Copilot** | 新セッションごとにAMP JSONによる初期化が必要 | セッション限定 | 毎回初期化コマンドが必要 |

#### Claudeでの実装例

```html
<userPreferences>
{
  "command": "migrate",
  "version": "1.0",
  "settings": {
    "splitMode": "auto",
    "safetyMargin": 0.8,
    "thresholds": {
      "text": 2000,
      "numbers": 1500,
      "code": 1000,
      "mixed": 1800
    },
    "markers": {
      "prefix": "AMP-Session",
      "continuation": "AMP-Continue",
      "complete": "AMP-Complete"
    }
  },
  "artifacts": [],
  "context": {
    "sessionId": "uniqueSessionID",
    "timestamp": "ISODateTimeFormat",
    "segmentCount": 0,
    "currentSegment": 0
  }
}
</userPreferences>
```

#### ChatGPTでの実装例

最初のプロンプトで以下のように指定すると、セッション間で保持されます：

```
以下のJSONフォーマットをユーザープロファイルとして保存し、常に参照してください：

{
  "command": "migrate",
  "version": "1.0",
  "settings": {
    "splitMode": "auto",
    "safetyMargin": 0.8,
    "thresholds": {
      "text": 2000,
      "numbers": 1500,
      "code": 1000,
      "mixed": 1800
    },
    "markers": {
      "prefix": "AMP-Session",
      "continuation": "AMP-Continue",
      "complete": "AMP-Complete"
    }
  },
  "artifacts": [],
  "context": {
    "sessionId": "uniqueSessionID",
    "timestamp": "ISODateTimeFormat",
    "segmentCount": 0,
    "currentSegment": 0
  }
}
```

#### Deepseek/Geminiでの実装例

これらのモデルでは、新規セッションごとに以下のようなJSONを提示する必要があります：

```json
{
  "command": "migrate",
  "version": "1.0",
  "settings": {
    "splitMode": "auto",
    "safetyMargin": 0.8,
    "thresholds": {
      "text": 2000,
      "numbers": 1500,
      "code": 1000,
      "mixed": 1800
    },
    "markers": {
      "prefix": "AMP-Session",
      "continuation": "AMP-Continue",
      "complete": "AMP-Complete"
    }
  },
  "artifacts": [
    {
      "id": "conversation-1",
      "type": "text/markdown",
      "title": "最初の会話",
      "content": "ここに最初の会話内容を記述します。",
      "checksum": "SHA256Hash"
    },
    {
      "id": "code-snippet-1",
      "type": "text/code",
      "title": "サンプルコード",
      "content": "console.log('Hello, World!');",
      "checksum": "SHA256Hash"
    }
  ],
  "context": {
    "sessionId": "uniqueSessionID",
    "timestamp": "ISODateTimeFormat",
    "segmentCount": 3,
    "currentSegment": 1
  }
}
```

#### Copilotでの実装例

Copilotでもカスタマイズされたパラメータでの実装が可能です：

```json
{
  "command": "migrate",
  "version": "1.0",
  "settings": {
    "splitMode": "manual",  // データの分割方法を手動に設定
    "safetyMargin": 0.9,    // 安全マージンを90%に設定
    "thresholds": {
      "text": 2500,         // テキストのトークン制限を2500に設定
      "numbers": 2000,      // 数値のトークン制限を2000に設定
      "code": 1500,         // コードのトークン制限を1500に設定
      "mixed": 2200         // 混合データのトークン制限を2200に設定
    },
    "markers": {
      "prefix": "Custom-Session",      // セッションの開始マーカーをカスタマイズ
      "continuation": "Custom-Continue", // セッションの継続マーカーをカスタマイズ
      "complete": "Custom-Complete"    // セッションの終了マーカーをカスタマイズ
    },
    "recoveryOptions": {               // エラー回復オプションを追加
      "retryCount": 3,                 // 再試行回数
      "retryInterval": 2000,           // 再試行間隔（ミリ秒）
      "cacheStrategy": "sessionStorage" // キャッシュ戦略
    }
  },
  "artifacts": [
    {
      "id": "conversation-1",
      "type": "text/markdown",
      "title": "最初の会話",
      "content": "ここに最初の会話内容を記述します。",
      "checksum": "7d9394b780182a038387dee71b31b19d4131fab8940c5a1a813f0d24d566e4be"
    }
  ],
  "context": {
    "sessionId": "copilot-20250322-67890",
    "timestamp": "2025-03-22T18:10:15Z",
    "segmentCount": 1,
    "currentSegment": 1
  }
}
```

各モデルでの一致した構造は、AMPがモデルに依存しない汎用性を持つことを示しています。実装方法の違いはありますが、基本的なプロトコル構造は共通して使用可能です。

## 6. 将来の拡張

- **差分更新**: 変更部分のみを送信する効率的な更新方式
- **圧縮方式**: 大きなアーティファクトのための効率的なエンコーディング
- **暗号化対応**: 機密データの安全な転送
- **分散セッション**: 複数のモデル間でのコンテキスト共有

## 7. 貢献とフィードバック

AMPは継続的に進化するオープンプロトコルです。実装例、パフォーマンス測定、改善提案などのフィードバックをお待ちしています。

## 付録

詳細な実験結果、データタイプ別の最適化ガイド、モデル別のパラメータ設定については、以下のリソースを参照してください：

- [複数AIモデル間のトークン枯渇特性比較](https://github.com/tQy2015/amp-protocol/blob/main/docs/research/token-exhaustion-comparison.md)

これらの技術文書では、各モデルでのトークン枯渇パターンと最適な分割戦略を詳細に解説しています。

---

*この仕様書はAMPの初期バージョンを定義するものであり、コミュニティフィードバックに基づいて更新される可能性があります。*