---
title: "PBR実習教材：2013年転換点から学ぶ物理ベースレンダリング"
description: "PBR Workshop Material: Learning Physically Based Rendering from the 2013 Turning Point"
date: "2025-07-02"
author: "TQY Kobayashi"
category: "course-materials"
tags: ["pbr", "workshop", "blender", "maya", "unity", "unreal-engine"]
license: "MIT"
version: "1.0"
melon_id: "421"
---
# PBR実習教材：2013年転換点から学ぶ物理ベースレンダリング

**PBR Workshop Material: Learning Physically Based Rendering from the 2013 Turning Point**

---

**対象：** 芸大デザイン学科4年生（デジタルツール習得済み、リニア・ガンマ未理解レベル）
**目標：** ソフトに依存しない PBR 概念の理解とマテリアル制作技術の習得
**時間：** 100分（1コマ90分+延長10分）
**使用ソフト：** Blender, Maya, Unity, Unreal Engine

---

## 実習の流れ

### 1. 座学：PBRの歴史と理論（20分）

#### 2013年：ゲーム業界の転換点

- **Before 2013：** アーティスト頼みの経験的ワークフロー

  - 各ソフト独自のシェーダー
  - アーティストの勘と経験による調整
  - ソフト間での見た目の不一致
- **After 2013：** 物理ベースの統一ワークフロー

  - Disney Principled BRDF の業界標準化
  - 実測データに基づく材質表現
  - ソフト間での一貫した見た目

#### ゲーム実例比較

- **転換前：** Crysis (2007), Uncharted 2 (2009)
- **転換後：** Crysis 3 (2013), The Last of Us (2013)

#### PBRの3つの柱

1. **物理的正確性：** 実世界の光の振る舞いをシミュレート
2. **エネルギー保存：** 反射する光は入射光を超えない
3. **線形ワークフロー：** 正確な光の計算のための色空間管理

---

### 2. 実物サンプル観察（15分）

#### 準備する材質サンプル（基本4種）

1. **ステンレス球**（金属の基本例）
2. **ガラス球**（透明材質・IOR）
3. **つや消し白アクリル球**（非金属拡散）
4. **18%グレー球**（基準・詳細はガンマ・リニア授業で解説）

#### 観察ポイント

- **触感**：表面の粗さ・滑らかさ
- **反射**：照明の映り込み具合（ステンレス vs 白アクリル）
- **透過**：光の通り方（ガラス球での確認）
- **基準認識**：18%グレー球（詳細は別授業で解説）

#### 質問投げかけ

- 「なぜステンレスは金属っぽく見えるのか？」
- 「なぜアクリルは金属と違って見えるのか？」
- 「なぜガラスは透明なのか？」

---

### 3. 計測値の提示（10分）

#### 実測データテーブル（基本4材質）

| 材質                     | Base Color (sRGB) | Metallic | Roughness | IOR  | 測定根拠                 |
| ------------------------ | ----------------- | -------- | --------- | ---- | ------------------------ |
| **ステンレス鋼**   | #D4D4D4           | 1.0      | 0.15      | -    | Physically Based         |
| **ガラス**         | #FFFFFF           | 0.0      | 0.0       | 1.52 | 標準値                   |
| **アクリル（白）** | #FFFFFF           | 0.0      | 0.5       | 1.49 | Physically Based         |
| **18%グレー**      | #2E2E2E           | 0.0      | 1.0       | -    | 写真標準（詳細は別授業） |

#### データの意味

- **Base Color：** 拡散反射色（非金属）/ 反射色（金属）
- **Metallic：** 金属性（0=非金属、1=金属）
- **Roughness：** 表面粗さ（0=鏡面、1=完全拡散）
- **IOR：** 屈折率（透明材質のみ）

#### 参照資料

- **Physically Based：** https://physicallybased.info/
- **MERL BRDF Database：** 100種類の実測材質データ
- **Disney Research (2012)：** PBR理論の基礎論文

---

### 4. 実習：マテリアル制作（40分）

#### 環境設定（5分）

各ソフトの色空間設定確認：

- **Blender：** Color Management → Filmic
- **Maya：** Color Management → ACES
- **Unity：** Player Settings → Linear
- **Unreal Engine：** Project Settings → Linear

#### シェーダーボール制作（30分）

**基本4マテリアル**（各ソフトで制作）

1. **ステンレス鋼**

   - Base Color: #D4D4D4
   - Metallic: 1.0
   - Roughness: 0.15
   - 用途：金属材質の理解
2. **ガラス**

   - Base Color: #FFFFFF
   - Metallic: 0.0
   - Roughness: 0.0
   - IOR: 1.52
   - 用途：透明材質・屈折の理解
3. **白アクリル（つや消し）**

   - Base Color: #FFFFFF
   - Metallic: 0.0
   - Roughness: 0.5
   - 用途：非金属拡散反射の理解
4. **18%グレー基準**

   - Base Color: #2E2E2E
   - Metallic: 0.0
   - Roughness: 1.0
   - 用途：基準材質（設定確認用）

#### ソフト別パラメータマッピング

| パラメータ | Blender    | Maya       | Unity HDRP  | Unreal Engine |
| ---------- | ---------- | ---------- | ----------- | ------------- |
| Base Color | Base Color | Base Color | Albedo      | Base Color    |
| Metallic   | Metallic   | Metalness  | Metallic    | Metallic      |
| Roughness  | Roughness  | Roughness  | Smoothness* | Roughness     |
| Normal     | Normal     | Normal Map | Normal Map  | Normal        |

*Unity の Smoothness は Roughness の逆数（1 - Roughness）

#### 確認ポイント（5分）

- 同じ数値で同じ見た目になるか？
- ソフト間の微細な差異の原因は？
- 実物サンプルとの一致度は？

---

### 5. シェーディング・ライティング（15分）

#### HDRIライティング設定

**推奨HDRI環境：**

- **Studio Lighting：** 均一な照明でマテリアル確認
- **Outdoor Environment：** 自然光での見た目確認
- **Interior Environment：** 室内照明での質感確認

#### 最終レンダリング比較

1. **4つのソフトで同じシーンをレンダリング**
2. **実物サンプルと CGレンダリング の比較**
3. **パラメータ調整による見た目の変化確認**

#### 評価基準

- **物理的正確性：** 実物との一致度
- **一貫性：** ソフト間での見た目統一
- **芸術性：** 美しい質感表現

---

## 応用編（追加サンプル収集後）

### 追加材質サンプル

**Phase 1：基本材質拡張**

- **アルミ球**（光沢金属）
- **木材球**（自然素材の拡散反射）
- **ゴム球**（高Roughness非金属）
- **光沢プラスチック球**（低Roughness非金属）

**Phase 2：発展材質**

- **透明プラスチック球**（IOR比較）
- **布テクスチャ球**（Sheen, 異方性）
- **錆びた金属球**（劣化表現, Mixed材質）

### 応用実習内容

1. **材質ライブラリ構築**

   - 8-10種類の標準マテリアル作成
   - ソフト間統一プリセット開発
2. **複合材質制作**

   - 車塗装（ベースコート + クリアコート）
   - 布地（Sheen パラメータ活用）
   - セラミック（釉薬表現）
3. **劣化・風化表現**

   - 錆びた金属の段階的変化
   - 汚れ・摩耗の表現技法

---

## 参考資料

### 必読論文・資料

1. **Physically Based Shading at Disney (2012)**

   - https://media.disneyanimation.com/uploads/production/publication_asset/48/asset/s2012_pbs_disney_brdf_notes_v3.pdf
   - PBR理論の基礎となった重要論文
2. **Physically Based Database**

   - https://physicallybased.info/
   - 実測値データベース（CG業界標準）
3. **MERL BRDF Database**

   - https://www.merl.com/research/license/BRDF
   - 100種類の材質実測データ

### 測定手法参考資料

1. **PBR測定実践例**

   - https://www.racoon-artworks.de/blog_PBRfromrulestomeasurements.php
   - 実際の測定機材と手法
2. **LearnOpenGL PBR Series**

   - https://learnopengl.com/PBR/Theory
   - 理論から実装まで包括的解説

### ソフトウェア別ドキュメント

- **Blender：** Principled BSDF 公式ドキュメント
- **Maya：** aiStandardSurface 材質ガイド
- **Unity：** HDRP 材質作成ガイド
- **Unreal Engine：** 物理ベース材質ユーザーガイド

---

## 評価・課題

### 理解度チェックポイント

1. **PBR以前と以後の違いを説明できるか？**
2. **Metallic パラメータの物理的意味を理解しているか？**
3. **同じ材質を異なるソフトで再現できるか？**
4. **実測値の重要性を理解しているか？**

### 提出課題

1. **4ソフトでの統一マテリアル制作**

   - 指定された実測値での材質再現
   - レンダリング結果の比較レポート
2. **オリジナル材質の提案**

   - 実在材質の実測値調査
   - PBRパラメータへの変換
   - 制作過程の文書化

### 評価基準

- **技術的正確性（40%）：** 実測値の正確な適用
- **一貫性（30%）：** ソフト間での統一性
- **創造性（20%）：** 新しい材質表現への挑戦
- **文書化（10%）：** 制作過程の記録・共有

---

## まとめ

この実習を通じて学生は以下を習得します：

1. **PBRの歴史的背景と意義の理解**
2. **物理的根拠に基づく材質制作技術**
3. **ソフトウェア間での統一的なワークフロー**
4. **実測データの活用方法**
5. **科学的アプローチによる品質向上**

2013年の技術転換点を理解することで、単なる技術習得にとどまらず、なぜPBRが業界標準となったのかという本質的な理解を深めることができます。実物観察から始まり、科学的データに基づく制作へと段階的に進むことで、確実な技術定着を図ります。

---

**ライセンス / License**: この作品は Creative Commons Attribution 4.0 International License の下でライセンスされています。

**リポジトリ / Repository**: [MELON System](https://github.com/tQy2015/melon) - Education Section