# amp-protocol
A standard protocol for managing large artifacts and context in LLM sessions

## プロジェクト概要
Artifact Migration Protocol (AMP)は、トークン制限のあるAIシステムでも長時間にわたる複雑な会話を維持し、生成された成果物を効率的に保存・再利用するために設計されたプロトコルです。

主な特徴：
- コンテキスト制限の克服
- アーティファクト保全
- セッション継続性の確保
- クロスモデル互換性

## 使用方法
AMPは複数のLLMで使用できます。基本的な使用例：

```json
{
  "command": "migrate",
  "version": "1.0",
  "settings": {
    "splitMode": "auto",
    "safetyMargin": 0.8
  },
  "artifacts": [],
  "context": {
    "sessionId": "amp-20250322-12345",
    "timestamp": "2025-03-22T17:55:42Z",
    "segmentCount": 1,
    "currentSegment": 1
  }
}
```
## Credits
Last Updated: 2025.03.22

Author: TQY Kobayashi (竹の台地域委員会CXO, HAL大阪 専任教官, 大阪芸術大学デザイン学科 客員教授 & 八箱CDO [TQY KOBAYASHI DesignTech Workshop](https://tqy.yahaco.com))  
Contributors: Claude, Gemini, Deepseek, ChatGPT, Copilot
