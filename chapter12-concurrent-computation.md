# 第12章 並行計算の理論

## はじめに

並行計算の理論は、複数の計算プロセスが同時に実行される状況を数学的に扱う分野です。マルチコアプロセッサ、分散システム、クラウドコンピューティングの普及により、並行性の理解と制御は現代のコンピュータサイエンスにおいて不可欠となっています。本章では、並行システムのモデル化、解析、検証のための理論的基礎を体系的に学びます。

並行性がもたらす非決定性、デッドロック、競合状態などの問題は、逐次プログラムでは現れない固有の複雑さを持ちます。これらの問題を理解し、解決するための形式的手法は、信頼性の高い並行システムの設計に欠かせません。本章で学ぶ理論は、オペレーティングシステム、データベース、分散アルゴリズムなど、幅広い分野の基礎となります。

## 12.1 並行計算モデル

### 12.1.1 並行性の基本概念

```mermaid
graph TD
    subgraph "並行計算の基本概念"
        subgraph "並行性 vs 並列性"
            Concurrency["並行性（Concurrency）<br/>・論理的な同時性<br/>・単一CPUでもインターリーブ<br/>・非決定性あり"]
            
            Parallelism["並列性（Parallelism）<br/>・物理的な同時実行<br/>・複数CPU/コア<br/>・性能向上が目的"]
            
            Both["両方の組み合わせ<br/>・マルチコアでの並行実行<br/>・分散システム"]
        end
        
        subgraph "実行モデルの比較"
            Sequential["逐次実行<br/>A1 → A2 → A3<br/>B1 → B2 → B3"]
            
            Interleaved["インターリーブ実行<br/>A1 → B1 → A2 → B2 → A3 → B3<br/>または<br/>B1 → A1 → B2 → A2 → B3 → A3"]
            
            Parallel_Exec["並列実行<br/>A1 || B1<br/>A2 || B2<br/>A3 || B3"]
        end
        
        subgraph "通信パラダイム"
            SharedMem["共有メモリ<br/>・共通変数へのアクセス<br/>・同期プリミティブ<br/>・競合状態の問題"]
            
            MessagePass["メッセージパッシング<br/>・send/receive操作<br/>・チャネル通信<br/>・デッドロックの問題"]
            
            Actor["アクターモデル<br/>・独立したアクター<br/>・非同期メッセージ<br/>・局所状態のみ"]
        end
        
        subgraph "並行システムの問題"
            RaceCondition["競合状態<br/>・共有データへの非同期アクセス<br/>・実行順序に依存した結果<br/>・再現困難なバグ"]
            
            Deadlock["デッドロック<br/>・相互に待機状態<br/>・循環的な資源依存<br/>・システム停止"]
            
            Starvation["飢餓状態<br/>・一部プロセスが永続的に待機<br/>・優先度による不公平<br/>・進行不能"]
        end
    end
    
    style Concurrency fill:#e3f2fd
    style Parallelism fill:#fff3e0
    style SharedMem fill:#e8f5e8
    style MessagePass fill:#f3e5f5
    style RaceCondition fill:#ffebee
    style Deadlock fill:#ffebee
```

**定義 12.1** **並行システム**は、複数のプロセスが同時に実行される可能性があるシステム。

**並行性 vs 並列性**：
- 並行性（Concurrency）：論理的な同時性、インターリーブ実行を含む
- 並列性（Parallelism）：物理的な同時実行

**インターリーブ意味論**：
並行実行を、可能なすべての実行順序（インターリーブ）の集合として捉える。

### 12.1.2 共有メモリモデル

**定義 12.2** **共有メモリシステム**では、プロセスが共通の変数を通じて通信する。

**原子的操作**：
- Read：x の値を読む
- Write：x に値を書く
- Read-Modify-Write：Test-and-Set、Compare-and-Swap

**メモリ一貫性モデル**：
- 逐次一貫性（Sequential Consistency）
- 弱い一貫性モデル（TSO、PSO、Relaxed）

### 12.1.3 メッセージパッシングモデル

**定義 12.3** **メッセージパッシングシステム**では、プロセスがメッセージ送受信により通信する。

**通信プリミティブ**：
- send(p, m)：プロセス p にメッセージ m を送信
- receive(q)：プロセス q からメッセージを受信

**同期性**：
- 同期通信：送信と受信が同時に起こる
- 非同期通信：送信後、受信まで遅延がある

## 12.2 プロセス代数

### 12.2.1 CCS（Calculus of Communicating Systems）

**定義 12.4** **CCS の構文**：
```
P ::= 0                    (停止)
    | α.P                  (プレフィックス)
    | P + Q                (選択)
    | P | Q                (並行合成)
    | P\L                  (制限)
    | P[f]                 (リラベリング)
    | A                    (プロセス定数)
```

**アクション**：
- a, b, c, ...：入力アクション
- ā, b̄, c̄, ...：出力アクション
- τ：内部アクション

### 12.2.2 操作的意味論

**遷移規則**（構造的操作意味論）：

```
(Act)   α.P --α--> P

(Sum1)  前提: P --α--> P'
        結論: P + Q --α--> P'

(Par1)  前提: P --α--> P'
        結論: P | Q --α--> P' | Q

(Com)   前提: P --a--> P', Q --ā--> Q'
        結論: P | Q --τ--> P' | Q'
```

### 12.2.3 双模倣等価性

**定義 12.5** 関係 R が**双模倣**（bisimulation）であるとは：
P R Q ならば、
1. P --α--> P' ⇒ ∃Q', Q --α--> Q' かつ P' R Q'
2. Q --α--> Q' ⇒ ∃P', P --α--> P' かつ P' R Q'

**定義 12.6** P と Q が**双模倣等価**（P ∼ Q）⟺ ある双模倣 R で P R Q

**定理 12.1** 双模倣等価性は合同関係である（すべての文脈で保存される）。

### 12.2.4 その他のプロセス代数

**CSP（Communicating Sequential Processes）**：
- 失敗意味論
- トレース、失敗、発散

**π計算**：
- 名前の移動性
- チャネルを値として送受信可能

## 12.3 Petri ネット

### 12.3.1 Petri ネットの定義

```mermaid
graph TD
    subgraph "Petriネットの構成要素"
        subgraph "基本要素"
            Place["プレース（Place）<br/>・円で表現<br/>・システムの状態<br/>・トークンを保持"]
            
            Transition["トランジション（Transition）<br/>・四角で表現<br/>・状態変化・イベント<br/>・発火により実行"]
            
            Arc["アーク（Arc）<br/>・有向辺<br/>・プレースとトランジション間<br/>・重み付き可能"]
            
            Token["トークン（Token）<br/>・黒丸で表現<br/>・リソース・データ<br/>・動的要素"]
        end
        
        subgraph "単純な例：相互排他"
            P1["プロセス1<br/>待機"]
            T1["プロセス1<br/>開始"]
            P2["臨界区間<br/>利用可能"]
            T2["プロセス1<br/>終了"]
            P3["プロセス1<br/>実行中"]
            
            P4["プロセス2<br/>待機"]
            T3["プロセス2<br/>開始"]
            T4["プロセス2<br/>終了"]
            P5["プロセス2<br/>実行中"]
            
            P1 --> T1
            P2 --> T1
            T1 --> P3
            P3 --> T2
            T2 --> P1
            T2 --> P2
            
            P4 --> T3
            P2 --> T3
            T3 --> P5
            P5 --> T4
            T4 --> P4
            T4 --> P2
        end
        
        subgraph "発火条件と結果"
            FireCondition["発火条件:<br/>・入力プレースに十分なトークン<br/>・アークの重み以上のトークン"]
            
            FireResult["発火結果:<br/>・入力プレースからトークン除去<br/>・出力プレースにトークン追加<br/>・原子的操作"]
            
            Concurrency["並行性:<br/>・異なるトランジションの同時発火<br/>・競合の場合は非決定的選択"]
        end
    end
    
    style Place fill:#e3f2fd
    style Transition fill:#fff3e0
    style Token fill:#e8f5e8
    style P2 fill:#ffebee
```

**定義 12.7** **Petri ネット**は 4つ組 N = (P, T, F, W) で定義される：
- P：プレースの有限集合
- T：トランジションの有限集合（P ∩ T = ∅）
- F ⊆ (P × T) ∪ (T × P)：フロー関係（アーク）
- W: F → ℕ：重み関数

**マーキング** M: P → ℕ は各プレースのトークン数を示す。

### 12.3.2 動的挙動と発火規則

```mermaid
graph TD
    subgraph "Petriネットの発火メカニズム"
        subgraph "発火前状態"
            Before["初期マーキング M₀"]
            P1_before["P1: ●●"]
            P2_before["P2: ●"]
            P3_before["P3: "]
            T1_before["T1（発火可能）"]
            
            Before --> P1_before
            Before --> P2_before
            Before --> P3_before
        end
        
        subgraph "発火条件チェック"
            Check["∀p ∈ •t: M(p) ≥ W(p,t)<br/>前置プレースに十分なトークン"]
            
            Enable["T1が発火可能<br/>・P1に2個のトークン必要→有<br/>・P2に1個のトークン必要→有"]
        end
        
        subgraph "発火後状態"
            After["新マーキング M₁"]
            P1_after["P1: "]
            P2_after["P2: "]
            P3_after["P3: ●●●"]
            T1_after["T1（発火完了）"]
            
            After --> P1_after
            After --> P2_after
            After --> P3_after
        end
        
        subgraph "発火効果"
            Remove["入力プレースからトークン除去<br/>M'(p) = M(p) - W(p,t)"]
            Add["出力プレースにトークン追加<br/>M'(p) = M(p) + W(t,p)"]
            Atomic["発火は原子的操作<br/>途中状態は存在しない"]
        end
        
        Before --> Check
        Check --> Enable
        Enable --> After
        
        Remove --> After
        Add --> After
        Atomic --> After
    end
    
    style Before fill:#e3f2fd
    style Check fill:#fff3e0
    style After fill:#e8f5e8
    style Enable fill:#f3e5f5
```

**発火規則**：
トランジション t が発火可能 ⟺ ∀p ∈ •t, M(p) ≥ W(p,t)

発火後のマーキング：M'(p) = M(p) - W(p,t) + W(t,p)

### 12.3.3 Petri ネットの解析性質

**到達可能性**：初期マーキング M₀ から到達可能なマーキングの集合 R(N, M₀)

**有界性**：すべての到達可能なマーキング M とプレース p に対して M(p) ≤ k

**活性**：任意の到達可能なマーキングから、さらにそのトランジションを発火可能

## 12.4 デッドロック

### 12.4.1 デッドロックの特徴

```mermaid
graph TD
    subgraph "デッドロックの4つの必要条件（Coffmanの条件）"
        subgraph "相互排他（Mutual Exclusion）"
            ME1["リソースRは一度に一つのプロセスのみ使用可能"]
            ME2["プロセスPがRを使用中"]
            ME3["プロセスQはRが解放されるまで待機"]
            
            ME1 --> ME2
            ME2 --> ME3
        end
        
        subgraph "保持と待機（Hold and Wait）"
            HW1["プロセスが少なくとも一つのリソースを保持"]
            HW2["同時に他のリソースを要求"]
            HW3["要求したリソースが他プロセスに保持されている"]
            
            HW1 --> HW2
            HW2 --> HW3
        end
        
        subgraph "非プリエンプション（No Preemption）"
            NP1["プロセスが保持するリソース"]
            NP2["強制的に取り上げ不可"]
            NP3["プロセス自身が自発的に解放するまで保持"]
            
            NP1 --> NP2
            NP2 --> NP3
        end
        
        subgraph "循環待機（Circular Wait）"
            CW1["プロセスP₁がプロセスP₂の保持するリソースを待機"]
            CW2["プロセスP₂がプロセスP₃の保持するリソースを待機"]
            CW3["..."]
            CW4["プロセスPₙがプロセスP₁の保持するリソースを待機"]
            
            CW1 --> CW2
            CW2 --> CW3
            CW3 --> CW4
            CW4 --> CW1
        end
    end
    
    style ME1 fill:#e3f2fd
    style HW1 fill:#fff3e0
    style NP1 fill:#e8f5e8
    style CW1 fill:#f3e5f5
```

### 12.4.2 資源割当グラフによるデッドロック検出

```mermaid
graph TD
    subgraph "資源割当グラフ（Resource Allocation Graph）"
        subgraph "グラフの構成要素"
            Process["プロセスノード<br/>（円）"]
            Resource["リソースノード<br/>（四角）"]
            Request["要求エッジ<br/>プロセス → リソース"]
            Allocation["割当エッジ<br/>リソース → プロセス"]
        end
        
        subgraph "デッドロック状況の例"
            P1["P1"]
            P2["P2"]
            R1["R1"]
            R2["R2"]
            
            P1 --> R2
            R1 --> P1
            P2 --> R1
            R2 --> P2
            
            DeadlockCycle["循環<br/>P1 → R2 → P2 → R1 → P1<br/>デッドロック発生"]
        end
        
        subgraph "デッドロック回避の例"
            P3["P3"]
            P4["P4"]
            P5["P5"]
            R3["R3"]
            R4["R4"]
            
            P3 --> R4
            R3 --> P3
            P4 --> R3
            R4 --> P5
            P5 --> R4
            
            NoDeadlock["循環なし<br/>リソース解放可能<br/>デッドロック回避"]
        end
        
        subgraph "検出アルゴリズム"
            Algorithm["1. グラフから循環を検出<br/>2. DFSによる後退エッジ検索<br/>3. 循環検出時はデッドロック"]
            
            Complexity["時間複雑度: O(V + E)<br/>V: ノード数（プロセス+リソース）<br/>E: エッジ数"]
        end
    end
    
    style DeadlockCycle fill:#ffebee
    style NoDeadlock fill:#e8f5e8
    style Algorithm fill:#e3f2fd
```

### 12.4.3 銀行家アルゴリズム（Banker's Algorithm）

```mermaid
graph TD
    subgraph "銀行家アルゴリズムによるデッドロック回避"
        subgraph "データ構造"
            Available["Available[m]<br/>各リソースタイプの利用可能数"]
            Max["Max[n][m]<br/>各プロセスの最大リソース需要"]
            Allocation["Allocation[n][m]<br/>各プロセスの現在割当"]
            Need["Need[n][m]<br/>各プロセスの残り需要<br/>Need = Max - Allocation"]
        end
        
        subgraph "安全性アルゴリズム"
            Step1["1. Work = Available<br/>Finish[i] = false (all i)"]
            Step2["2. Finish[i] = false かつ<br/>Need[i] ≤ Work のプロセス i を探す"]
            Step3["3. 見つかれば:<br/>Work = Work + Allocation[i]<br/>Finish[i] = true"]
            Step4["4. 全プロセスでFinish[i] = true<br/>なら安全状態"]
            
            Step1 --> Step2
            Step2 --> Step3
            Step3 --> Step2
            Step2 --> Step4
        end
        
        subgraph "リソース要求処理"
            Request["Request[i]<br/>プロセス i のリソース要求"]
            Check1["Request[i] ≤ Need[i]?"]
            Check2["Request[i] ≤ Available?"]
            TentativeAlloc["仮割当:<br/>Available -= Request[i]<br/>Allocation[i] += Request[i]<br/>Need[i] -= Request[i]"]
            SafetyCheck["安全性チェック実行"]
            
            Request --> Check1
            Check1 -->|Yes| Check2
            Check2 -->|Yes| TentativeAlloc
            TentativeAlloc --> SafetyCheck
            SafetyCheck -->|Safe| Grant["要求承認"]
            SafetyCheck -->|Unsafe| Rollback["割当取消<br/>要求拒否"]
            
            Check1 -->|No| Error1["エラー:<br/>最大需要超過"]
            Check2 -->|No| Wait["プロセス待機"]
        end
        
        subgraph "例：3プロセス、3リソースタイプ"
            ExampleState["初期状態:<br/>Available = [3,3,2]<br/>Max = [[7,5,3],[3,2,2],[9,0,2]]<br/>Allocation = [[0,1,0],[2,0,0],[3,0,2]]<br/>Need = [[7,4,3],[1,2,2],[6,0,0]]"]
            
            SafeSequence["安全シーケンス:<br/>P1 → P0 → P2<br/>または<br/>P1 → P2 → P0"]
        end
    end
    
    style Step4 fill:#e8f5e8
    style Grant fill:#e8f5e8
    style Rollback fill:#ffebee
    style SafeSequence fill:#f3e5f5
```

**時間複雑度**：安全性アルゴリズムは O(mn²)
- m：リソースタイプ数
- n：プロセス数

### 12.4.4 デッドロック対策の比較

1. **予防**：Coffmanの条件の一つを破る
2. **回避**：銀行家アルゴリズムによる動的チェック
3. **検出と回復**：定期的な検出と強制終了
4. **無視**：鳥駝政策（現実的なアプローチ）

本章では、並行計算の理論的基礎を学びました。これらの概念は、マルチコアシステム、分散システム、クラウドコンピューティングなど、現代の計算環境において不可欠な知識です。
