# 理論計算機科学教科書 - 章末問題解答集

## 第1章 理論計算機科学への招待 - 解答例

### 基礎問題

**問題1**: 以下のアルゴリズムの時間計算量を求めよ

**(a) 配列の要素をすべて0で初期化**
```python
for i in range(n):
    array[i] = 0
```

**解答**：
- 配列の各要素に対して定数時間の操作を実行
- n個の要素に対してそれぞれ1回ずつ操作
- **時間計算量：O(n)**

**(b) 二重ループによる行列の初期化**
```python
for i in range(n):
    for j in range(n):
        matrix[i][j] = 0
```

**解答**：
- 外側のループがn回実行
- 各外側のループに対して内側のループがn回実行
- 総操作回数：n × n = n²
- **時間計算量：O(n²)**

**(c) 二分探索**
```python
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```

**解答**：
- 各反復で探索範囲が半分になる
- 最大反復回数：⌊log₂ n⌋ + 1
- **時間計算量：O(log n)**

**問題2**: 以下の漸近記法の関係を判定せよ

**(a) n² + 3n + 1 と O(n²) の関係**

**解答**：
- n² + 3n + 1 = O(n²) を示す
- |n² + 3n + 1| ≤ c·n² を満たすc > 0と

<!-- 完全版は講師向け資料をご参照ください -->