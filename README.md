# 理論計算機科学教科書

## 📚 概要

本書は、理論計算機科学の包括的な教科書です。数学的基礎から最先端の研究トピックまで、体系的に学習できるように構成されています。

- **🎯 対象読者**: 計算機科学を学ぶ大学生・大学院生・研究者
- **📖 構成**: 全12章 + 包括的な学習支援教材
- **🔧 特徴**: 140以上の図表、豊富な練習問題、実装例、実世界での応用例

## 🗺️ 全体像

```mermaid
graph TB
    subgraph "理論計算機科学の体系と相互関連"
        subgraph "基礎理論層"
            Ch1["第1章: 理論計算機科学への招待<br/>・アルゴリズムとは何か<br/>・計算量の基礎<br/>・問題の分類"]
            
            Ch2["第2章: 計算理論の基礎<br/>・チューリング機械<br/>・計算可能性<br/>・Church-Turingの提唱"]
            
            Ch3["第3章: 形式言語とオートマトン<br/>・有限オートマトン<br/>・正規言語<br/>・文脈自由言語"]
        end
        
        subgraph "中核理論層"
            Ch4["第4章: 計算可能性<br/>・決定可能性<br/>・停止問題<br/>・Riceの定理"]
            
            Ch5["第5章: 計算複雑性理論<br/>・P vs NP問題<br/>・NP完全性<br/>・複雑性クラス"]
        end
        
        subgraph "アルゴリズム層"
            Ch6["第6章: アルゴリズム<br/>・設計手法<br/>・動的計画法<br/>・近似アルゴリズム"]
            
            Ch7["第7章: データ構造<br/>・基本構造<br/>・木とグラフ<br/>・高度な構造"]
            
            Ch8["第8章: グラフ理論<br/>・基本アルゴリズム<br/>・最短路・最大流<br/>・NP困難問題"]
        end
        
        subgraph "応用理論層"
            Ch9["第9章: 論理学・形式的手法<br/>・命題・述語論理<br/>・プログラム検証<br/>・モデル検査"]
            
            Ch10["第10章: 情報理論<br/>・エントロピー<br/>・符号化<br/>・通信路容量"]
            
            Ch11["第11章: 暗号理論<br/>・公開鍵暗号<br/>・電子署名<br/>・ゼロ知識証明"]
            
            Ch12["第12章: 並行計算<br/>・並行モデル<br/>・分散アルゴリズム<br/>・同期問題"]
        end
        
        %% 関係性
        Ch1 -.-> Ch2
        Ch1 -.-> Ch3
        Ch2 --> Ch3
        Ch2 ==> Ch4
        Ch3 ==> Ch4
        Ch2 ==> Ch5
        Ch4 ==> Ch5
        Ch5 ==> Ch6
        Ch1 -.-> Ch6
        Ch6 <--> Ch7
        Ch6 --> Ch8
        Ch7 --> Ch8
        Ch5 --> Ch8
        Ch3 --> Ch9
        Ch4 --> Ch9
        Ch1 -.-> Ch10
        Ch6 --> Ch10
        Ch5 ==> Ch11
        Ch10 --> Ch11
        Ch2 --> Ch12
        Ch8 --> Ch12
        Ch9 --> Ch12
    end
    
    %% スタイリング
    classDef foundation fill:#e3f2fd,stroke:#1976d2,stroke-width:2px
    classDef core fill:#fff3e0,stroke:#f57c00,stroke-width:2px
    classDef algorithm fill:#e8f5e9,stroke:#388e3c,stroke-width:2px
    classDef applied fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px
    
    class Ch1,Ch2,Ch3 foundation
    class Ch4,Ch5 core
    class Ch6,Ch7,Ch8 algorithm
    class Ch9,Ch10,Ch11,Ch12 applied
```

## 📖 章構成

### 🏛️ 基礎理論層

| 章 | タイトル | 内容 | 推定学習時間 |
|---|----------|------|-------------|
| [第1章](chapter1-complete-with-exercises.md) | 数学的基礎 | 集合論、論理学、証明技法、関係と関数、グラフ理論の基礎 | 4-6週間 |
| [第2章](chapter2-complete-with-exercises.md) | 計算理論の基礎 | チューリング機械、計算可能性、Church-Turingの提唱 | 3-4週間 |
| [第3章](chapter3-complete-with-exercises.md) | 形式言語とオートマトン理論 | 有限オートマトン、正規言語、文脈自由言語 | 4-5週間 |

### 🎯 中核理論層

| 章 | タイトル | 内容 | 推定学習時間 |
|---|----------|------|-------------|
| [第4章](chapter4-computability.md) | 計算可能性 | 決定可能性、停止問題、Riceの定理、還元可能性 | 3-4週間 |
| [第5章](chapter5-complexity.md) | 計算複雑性理論 | P vs NP問題、NP完全性、複雑性クラス、多項式階層 | 4-6週間 |

### ⚙️ アルゴリズム層

| 章 | タイトル | 内容 | 推定学習時間 |
|---|----------|------|-------------|
| [第6章](chapter6-algorithms.md) | アルゴリズムの数学的解析 | 設計手法、分割統治法、動的計画法、貪欲法、近似アルゴリズム | 5-7週間 |
| [第7章](chapter7-data-structures.md) | データ構造の理論 | 基本構造、平衡木、高度なデータ構造、確率的データ構造 | 4-5週間 |
| [第8章](chapter8-graph-theory.md) | グラフ理論とネットワーク | 基本アルゴリズム、最短路、最大流、マッチング、グラフ彩色 | 5-6週間 |

### 🚀 応用理論層

| 章 | タイトル | 内容 | 推定学習時間 |
|---|----------|------|-------------|
| [第9章](chapter9-logic-formal-methods.md) | 論理学・形式的手法 | 命題・述語論理、時相論理、プログラム検証、モデル検査 | 4-5週間 |
| [第10章](chapter10-information-theory.md) | 情報理論 | エントロピー、符号化理論、通信路容量、誤り訂正符号 | 3-4週間 |
| [第11章](chapter11-cryptography.md) | 暗号理論の数学的基礎 | 対称・公開鍵暗号、デジタル署名、ゼロ知識証明 | 4-5週間 |
| [第12章](chapter12-concurrent-computation.md) | 並行計算の理論 | 並行計算モデル、プロセス代数、分散アルゴリズム | 3-4週間 |

## 📚 学習支援教材

### 📝 練習問題と解答

- [**練習問題解答集**](chapter-solutions.md) - 全章の詳細な解答と解説
- [**難易度別練習問題ガイド**](exercise-difficulty-guide.md) - レベル別学習パス (⭐〜⭐⭐⭐⭐)

### 📖 参考資料

- [**用語集・索引**](glossary-index.md) - 3,500語以上の専門用語解説（英日対訳付き）
- [**数学記法統一ガイド**](mathematical-notation-guide.md) - 全章で使用する記法の統一仕様
- [**章間相互参照**](cross-references.md) - 概念の関連性と学習順序

### 💻 実装と応用

- [**アルゴリズム実装集**](algorithm-implementations.md) - Python実装例と計算量解析
- [**実世界応用例集**](real-world-applications.md) - 各章の理論の実際の活用事例

### 📊 学習管理

- [**学習進捗チェックリスト**](learning-progress-checklist.md) - 自己評価用チェックリスト
- [**完全版目次**](complete-textbook-toc.md) - 詳細な章節構成

## 🎯 学習パス

### 🔰 初学者向け（6-8ヶ月）
```
第1章 → 第2章 → 第3章 → 第6章(基礎) → 第7章(基礎) → 第8章(基礎)
```

### 📈 標準コース（12-15ヶ月）
```
第1章 → 第2章 → 第3章 → 第4章 → 第5章 → 第6章 → 第7章 → 第8章 → 第9章
```

### 🎓 完全マスター（18-24ヶ月）
```
全章を順番に学習 + 発展的トピック + 最新研究動向の調査
```

### 🔬 研究者向け
```
基礎確認（第1-3章） → 専門分野集中（第4-12章から選択） → 最新論文読解
```

## 📱 各プラットフォームでの閲覧

### 🌐 GitHub Pages
- **URL**: [GitHub Pages版](https://itdojp.github.io/theoretical-computer-science-textbook-public/)
- **特徴**: インタラクティブな図表、完全な数式表示、検索機能
- **推奨**: デスクトップでの学習、研究用途

### 📖 Kindle
- **形式**: 統合PDF版、EPUB版
- **特徴**: オフライン閲覧、ハイライト・メモ機能
- **推奨**: 移動中の学習、復習用途

### ✍️ Zenn
- **URL**: [Zenn版](https://zenn.dev/ootakazuhiko/books/theoretical-cs)
- **特徴**: 章別記事、コメント機能、SNS連携
- **推奨**: 議論・質問、学習コミュニティ

## 🏆 特徴

### 📊 豊富な視覚化
- **140以上のMermaid図表**: 概念の直観的理解を支援
- **アルゴリズムフローチャート**: 実装の詳細な手順
- **複雑性関係図**: 問題クラス間の関係性

### 🎯 実践的学習
- **段階的難易度**: ⭐〜⭐⭐⭐⭐の4段階評価
- **実装例**: Python/擬似コードによる具体例
- **応用事例**: Google、Amazon等の実世界での活用

### 🔄 相互関連性
- **章間リンク**: 概念の発展と関連性を明示
- **前提知識**: 各章で必要な予備知識を明記
- **発展学習**: より高度なトピックへの道筋

## 🚀 使い方

### 📖 読み方
1. [**全体像**](#🗺️-全体像)で理論の構造を把握
2. [**学習パス**](#🎯-学習パス)から目標に適したコースを選択
3. 各章の[**難易度ガイド**](exercise-difficulty-guide.md)を確認
4. [**進捗チェックリスト**](learning-progress-checklist.md)で理解度を評価

### 💻 実習
1. [**実装集**](algorithm-implementations.md)でコーディング練習
2. [**応用例**](real-world-applications.md)で理論と実践を結びつけ
3. [**練習問題**](chapter-solutions.md)で知識を定着

### 🤝 コミュニティ
- **GitHub Issues**: 質問・議論・改善提案
- **Zenn**: 学習記録・疑問点の共有
- **Pull Request**: 内容の改善・追加への貢献

## 📝 ライセンス

MIT License - 学習・教育目的での自由な利用を促進

## 🤝 貢献

- **内容改善**: より良い説明、新しい例題
- **誤植修正**: タイポ、数式の間違い
- **実装例**: 新しいプログラミング言語での実装
- **応用事例**: 最新の実世界での活用例

## 📞 連絡先

- **GitHub**: [Issues](https://github.com/itdojp/theoretical-computer-science-textbook-private/issues)
- **Email**: your.email@example.com

---

**🎓 理論を学び、実践に活かし、未来を創造する**

> "理論なしには実践は盲目であり、実践なしには理論は空虚である" - Immanuel Kant