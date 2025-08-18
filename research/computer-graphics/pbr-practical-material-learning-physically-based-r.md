---
title: >-
  PBR Practical Material Learning: Physically Based Rendering from the 32-Year Lag
description: PBRは理論から普及に32年もかかった
date: '2025-08-18'
author: Tiky Kobayashi
category: research
subcategory: computer-graphics
tags:
  - research
  - maya-programming
  - unity-development
  - artificial-intelligence
license: MIT
melon_id: 320
version: '1.0'
---

# PBR実習教材：32年ラグから学ぶ物理ベースレンダリング革命

**対象：** 芸大デザイン学科4年生（デジタルツール習得済み、リニア・ガンマ未理解レベル）  
**目標：** ソフトに依存しない PBR 概念の理解とマテリアル制作技術の習得  
**時間：** 110分（1コマ90分+延長20分）  
**使用ソフト：** Blender, Maya, Unity, Unreal Engine

---

## 実習の流れ

### 1. 座学：PBRの歴史と32年ラグの真因（30分）

#### 🎯 Cook-Torrance理論(1981) → 実用化(2013) = 32年ラグの謎

**なぜ優れた理論があっても実用化に32年もかかったのか？**

#### 1981年：理論的基盤の確立
- **Cook-Torrance論文発表**：「A Reflectance Model for Computer Graphics」
- **マイクロファセット理論**：表面を微小な面の集合として物理的にモデル化
- **参考論文**：https://dl.acm.org/doi/10.1145/357290.357293
- **問題**：当時のハードウェアでは計算不可能

#### 1981-2005年：技術的制約期間
**3つの制約が揃わなかった：**

1. **VRAM制約**
   - 1995年 PlayStation: 1MB VRAM
   - 2000年 PlayStation 2: 4MB VRAM  
   - **PBR必要最低ライン**: 1GB以上（複数テクスチャ同時参照のため）

2. **解像度制約**
   - 1995-2005年: 480p標準画質
   - **PBR効果が視認できる最低ライン**: 720p以上

3. **ポリゴン性能制約**
   - 1995年 PlayStation: 0.36M polygons/sec
   - 2005年 Xbox 360: 10M polygons/sec（実測値）
   - **PBR適正ライン**: 20M+ polygons/sec（詳細ジオメトリ + 法線マップ）

#### 2005-2012年：ハードウェア準備完了、しかし普及せず

**なぜまだ普及しなかったのか？**
- Xbox 360 (512MB統合メモリ): 技術的には可能
- PS3 (256MB VRAM): まだ制約あり
- **決定的欠如**: 実装ノウハウとワークフローの未整備

#### 🔥 2012-2013年：SIGGRAPH Course革命 - 真の転換点

**2012年 SIGGRAPH**: "Practical Physically Based Shading in Film and Game Production"
- **Disney BRDF発表** (Brent Burley): 業界標準となるPBRモデル
- **Far Cry 3実装事例** (Stephen McAuley): 実際のゲームでのPBR成功例
- **Pixar制作例** (Brian Smits): WALL-E、Upでの実用ワークフロー
- **参考URL**: https://blog.selfshadow.com/publications/s2012-shading-course/

**2013年 SIGGRAPH**: "Physically Based Shading in Theory and Practice"
- **UE4 Real Shading発表** (Brian Karis): ゲームエンジンのPBR標準化
- **Call of Duty: Black Ops II**: AAAタイトルでの実装
- **The Order: 1886**: 次世代機向けPBRパイプライン
- **参考URL**: https://blog.selfshadow.com/publications/s2013-shading-course/

#### 2013年：ついに3つの条件が揃う

1. **ハードウェア成熟**: PS4/Xbox One (8GB統合メモリ)
2. **知識の民主化**: SIGGRAPH Courseで具体的実装ノウハウ公開
3. **エンジン標準化**: UE4・Unity 5がPBRをデフォルト採用

#### なぜ2013年だったのか？
- **理論の実証**: Disney・Pixarが実際のプロダクションで成功
- **知識共有革命**: SIGGRAPHで業界全体にノウハウが拡散
- **市場圧力**: 次世代機競争による技術革新の加速
- **開発効率**: PBRによる制作ワークフロー改善の実証

#### ゲーム実例比較
- **転換前**: Crysis (2007), Uncharted 2 (2009) - 経験的ワークフロー
- **転換後**: Crysis 3 (2013), The Last of Us (2013) - 物理ベースワークフロー

#### PBRの3つの柱（再確認）
1. **物理的正確性**: 実世界の光の振る舞いをシミュレート
2. **エネルギー保存**: 反射する光は入射光を超えない
3. **線形ワークフロー**: 正確な光の計算のための色空間管理

---

### 2. 実物サンプル観察（15分）

#### 準備する材質サンプル（基本4種）
1. **ステンレス球**（金属の基本例）
2. **ガラス球**（透明材質・IOR）
3. **つや消し白アクリル球**（非金属拡散）
4. **18%グレー球**（基準・詳細はガンマ・リニア授業で解説）

#### 観察ポイント
- **触感**: 表面の粗さ・滑らかさ
- **反射**: 照明の映り込み具合（ステンレス vs 白アクリル）
- **透過**: 光の通り方（ガラス球での確認）
- **基準認識**: 18%グレー球（詳細は別授業で解説）

#### 質問投げかけ
- 「なぜステンレスは金属っぽく見えるのか？」
- 「なぜアクリルは金属と違って見えるのか？」
- 「なぜガラスは透明なのか？」
- 「2013年以前はこれらをどう表現していたのか？」

---

### 3. 計測値の提示（10分）

#### 実測データテーブル（基本4材質）

| 材質 | Base Color (sRGB) | Metallic | Roughness | IOR | 測定根拠 |
|------|------------------|----------|-----------|-----|----------|
| **ステンレス鋼** | #D4D4D4 | 1.0 | 0.15 | - | Physically Based |
| **ガラス** | #FFFFFF | 0.0 | 0.0 | 1.52 | 標準値 |
| **アクリル（白）** | #FFFFFF | 0.0 | 0.5 | 1.49 | Physically Based |
| **18%グレー** | #2E2E2E | 0.0 | 1.0 | - | 写真標準（詳細は別授業） |

#### データの意味
- **Base Color**: 拡散反射色（非金属）/ 反射色（金属）
- **Metallic**: 金属性（0=非金属、1=金属）
- **Roughness**: 表面粗さ（0=鏡面、1=完全拡散）
- **IOR**: 屈折率（透明材質のみ）

#### 32年ラグの教訓
**なぜ実測値が重要なのか？**
- 2013年以前: アーティストの勘と経験に依存
- 2013年以後: 科学的データに基づく一貫した品質
- Disney BRDFの功績: 実測値を使いやすいパラメータに変換

---

### 4. 実習：マテリアル制作（40分）

#### 環境設定（5分）
各ソフトの色空間設定確認：
- **Blender**: Color Management → Filmic
- **Maya**: Color Management → ACES
- **Unity**: Player Settings → Linear
- **Unreal Engine**: Project Settings → Linear

**歴史的注意**: 2013年以前はソフトごとに異なる設定→統一されていない見た目

#### シェーダーボール制作（30分）

**基本4マテリアル**（各ソフトで制作）

1. **ステンレス鋼**
   - Base Color: #D4D4D4
   - Metallic: 1.0
   - Roughness: 0.15
   - 用途: 金属材質の理解

2. **ガラス**
   - Base Color: #FFFFFF
   - Metallic: 0.0
   - Roughness: 0.0
   - IOR: 1.52
   - 用途: 透明材質・屈折の理解

3. **白アクリル（つや消し）**
   - Base Color: #FFFFFF
   - Metallic: 0.0
   - Roughness: 0.5
   - 用途: 非金属拡散反射の理解

4. **18%グレー基準**
   - Base Color: #2E2E2E
   - Metallic: 0.0
   - Roughness: 1.0
   - 用途: 基準材質（設定確認用）

#### ソフト別パラメータマッピング（2013年後の統一化）

| パラメータ | Blender | Maya | Unity HDRP | Unreal Engine |
|-----------|---------|------|-------------|---------------|
| Base Color | Base Color | Base Color | Albedo | Base Color |
| Metallic | Metallic | Metalness | Metallic | Metallic |
| Roughness | Roughness | Roughness | Smoothness* | Roughness |
| Normal | Normal | Normal Map | Normal Map | Normal |

*Unity の Smoothness は Roughness の逆数（1 - Roughness）

#### 確認ポイント（5分）
- 同じ数値で同じ見た目になるか？
- ソフト間の微細な差異の原因は？
- 実物サンプルとの一致度は？
- **重要**: これが2013年のPBR革命の成果！

---

### 5. シェーディング・ライティング（15分）

#### HDRIライティング設定
**推奨HDRI環境:**
- **Studio Lighting**: 均一な照明でマテリアル確認
- **Outdoor Environment**: 自然光での見た目確認
- **Interior Environment**: 室内照明での質感確認

#### 最終レンダリング比較
1. **4つのソフトで同じシーンをレンダリング**
2. **実物サンプルと CGレンダリング の比較**
3. **パラメータ調整による見た目の変化確認**

#### 評価基準
- **物理的正確性**: 実物との一致度
- **一貫性**: ソフト間での見た目統一（2013年革命の成果）
- **芸術性**: 美しい質感表現

---

## 応用編（追加サンプル収集後）

### 追加材質サンプル
**Phase 1: 基本材質拡張**
- **アルミ球**（光沢金属）
- **木材球**（自然素材の拡散反射）
- **ゴム球**（高Roughness非金属）
- **光沢プラスチック球**（低Roughness非金属）

**Phase 2: 発展材質**
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

### 🔥 SIGGRAPH Course革命の重要文献（必読）

#### 2012年 転換点資料
1. **SIGGRAPH 2012 Course: "Practical Physically Based Shading in Film and Game Production"**
   - **URL**: https://blog.selfshadow.com/publications/s2012-shading-course/
   - **重要性**: PBR普及の真の転換点となった歴史的コース
   - **Disney BRDF論文**: https://media.disneyanimation.com/uploads/production/publication_asset/48/asset/s2012_pbs_disney_brdf_notes_v3.pdf

#### 2013年 標準化資料
2. **SIGGRAPH 2013 Course: "Physically Based Shading in Theory and Practice"**
   - **URL**: https://blog.selfshadow.com/publications/s2013-shading-course/
   - **重要性**: UE4 Real Shadingなど業界標準化を決定づけたコース
   - **UE4実装詳細**: Unreal Engine 4でのPBR実装解説

#### 理論的基盤
3. **Cook-Torrance原論文 (1982)**
   - **URL**: https://dl.acm.org/doi/10.1145/357290.357293
   - **タイトル**: "A Reflectance Model for Computer Graphics"
   - **重要性**: 32年ラグの起点となった理論的基盤

### 実測データベース
1. **Physically Based Database**
   - **URL**: https://physicallybased.info/
   - **内容**: 実測値データベース（CG業界標準）

2. **MERL BRDF Database**
   - **URL**: https://www.merl.com/research/license/BRDF
   - **内容**: 100種類の材質実測データ

### 測定手法参考資料
1. **PBR測定実践例**
   - **URL**: https://www.racoon-artworks.de/blog_PBRfromrulestomeasurements.php
   - **内容**: 実際の測定機材と手法

2. **LearnOpenGL PBR Series**
   - **URL**: https://learnopengl.com/PBR/Theory
   - **内容**: 理論から実装まで包括的解説

### ソフトウェア別ドキュメント
- **Blender**: Principled BSDF 公式ドキュメント
- **Maya**: aiStandardSurface 材質ガイド
- **Unity**: HDRP 材質作成ガイド
- **Unreal Engine**: 物理ベース材質ユーザーガイド

---

## 評価・課題

### 理解度チェックポイント
1. **32年ラグの3つの要因を説明できるか？**
   - VRAM制約、解像度制約、ポリゴン性能制約
2. **2012-2013年のSIGGRAPH Course革命の意義を理解しているか？**
   - Disney BRDF、Far Cry 3、UE4 Real Shadingの影響
3. **Metallic パラメータの物理的意味を理解しているか？**
4. **同じ材質を異なるソフトで再現できるか？**
5. **実測値の重要性と2013年以前との違いを理解しているか？**

### 提出課題
1. **4ソフトでの統一マテリアル制作**
   - 指定された実測値での材質再現
   - レンダリング結果の比較レポート
   - **新規追加**: 32年ラグとSIGGRAPH革命についての考察

2. **オリジナル材質の提案**
   - 実在材質の実測値調査
   - PBRパラメータへの変換
   - 制作過程の文書化

3. **歴史的比較レポート（新規）**
   - 2013年以前と以後のワークフロー比較
   - SIGGRAPH Course資料の要約
   - 技術史の考察

### 評価基準
- **技術的正確性（30%）**: 実測値の正確な適用
- **一貫性（25%）**: ソフト間での統一性
- **歴史理解（25%）**: 32年ラグとSIGGRAPH革命の理解
- **創造性（15%）**: 新しい材質表現への挑戦
- **文書化（5%）**: 制作過程の記録・共有

---

## まとめ

この実習を通じて学生は以下を習得します：

### 技術的スキル
1. **物理的根拠に基づく材質制作技術**
2. **ソフトウェア間での統一的なワークフロー**
3. **実測データの活用方法**
4. **科学的アプローチによる品質向上**

### 歴史的理解
1. **Cook-Torrance理論(1981)からの32年ラグの真因**
2. **2012-2013年SIGGRAPH Course革命の意義**
3. **技術普及における複数制約の同時解決の重要性**
4. **知識共有と実装ノウハウの価値**

### 本質的な学び
**なぜPBRが業界標準となったのか？**
- 単なる技術の進歩ではなく、理論・ハードウェア・知識共有・ワークフロー整備の複合的な成果
- 優れた理論があっても、実装ノウハウとコミュニティでの知識共有が揃わなければ普及しない
- SIGGRAPHのような学術・産業界の架け橋の重要性

**32年ラグから学ぶ教訓:**
- 技術革新は一直線ではなく、複数の制約が同時に解決される「転換点」で急激に進歩する
- 理論の価値は実用化されて初めて発揮される
- コミュニティでの知識共有が技術普及の決定的要因となる

2013年の技術転換点を理解することで、単なる技術習得にとどまらず、技術史の本質的な理解を深めることができます。実物観察から始まり、科学的データに基づく制作へと段階的に進むことで、確実な技術定着を図ります。