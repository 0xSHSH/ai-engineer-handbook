# 9️⃣ Coding Interview (DSA)

> Part of the [Interview Handbook](README.md). Pattern-based prep — recognizing which of ~15 patterns a new problem maps to beats memorizing individual solutions.

## 📑 Contents
- [Roadmap](#roadmap)
- [Complexity Cheat Sheet](#complexity-cheat-sheet)
- [Core Patterns with Worked Examples](#core-patterns-with-worked-examples)
- [Practice Set (Blind 75 / NeetCode-style, organized by pattern)](#practice-set-blind-75--neetcode-style-organized-by-pattern)

---

## Roadmap
1. **Weeks 1-2**: Arrays, Strings, Hash Maps, Two Pointers, Sliding Window.
2. **Weeks 3-4**: Linked Lists, Stacks/Queues, Binary Search, Recursion basics.
3. **Weeks 5-6**: Trees (traversals, BST), Heaps, Tries.
4. **Weeks 7-8**: Graphs (BFS/DFS, topological sort, union-find).
5. **Weeks 9-10**: Dynamic Programming (1D → 2D → on trees/graphs), Backtracking, Greedy.
6. **Weeks 11-12**: Bit manipulation, mock interviews, timed practice on mixed sets.

The **Blind 75 / NeetCode 150** lists are the standard curated sets — the value isn't the exact 75 or 150 problems, it's that they cover every pattern below at least twice.

## Complexity Cheat Sheet
| Structure/Op | Time | Notes |
|---|---|---|
| Array access | O(1) | Index-based |
| Array search (unsorted) | O(n) | |
| Binary search (sorted) | O(log n) | Requires sorted/monotonic data |
| Hash map get/set | O(1) avg | O(n) worst case (collisions) |
| Balanced BST ops | O(log n) | |
| Heap push/pop | O(log n) | Peek is O(1) |
| Sorting | O(n log n) | Comparison-sort lower bound |
| BFS/DFS on graph | O(V + E) | |
| DP table fill | O(states × transitions) | e.g., O(n·m) for a 2D table |

## Core Patterns with Worked Examples

### 1. Two Pointers
```python
def two_sum_sorted(nums, target):
    lo, hi = 0, len(nums) - 1
    while lo < hi:
        s = nums[lo] + nums[hi]
        if s == target:
            return [lo, hi]
        elif s < target:
            lo += 1
        else:
            hi -= 1
    return []
```

### 2. Sliding Window
```python
def longest_unique_substring(s):
    seen = {}
    start = best = 0
    for i, ch in enumerate(s):
        if ch in seen and seen[ch] >= start:
            start = seen[ch] + 1
        seen[ch] = i
        best = max(best, i - start + 1)
    return best
```

### 3. Binary Search (on answer, not just arrays)
```python
def min_days_to_ship(weights, capacity_guess_range):
    def can_ship(capacity, days_limit):
        days, load = 1, 0
        for w in weights:
            if load + w > capacity:
                days += 1
                load = 0
            load += w
        return days <= days_limit
    lo, hi = max(weights), sum(weights)
    while lo < hi:
        mid = (lo + hi) // 2
        if can_ship(mid, days_limit=5):
            hi = mid
        else:
            lo = mid + 1
    return lo
```

### 4. Trees (DFS/BFS)
```python
def max_depth(root):
    if not root:
        return 0
    return 1 + max(max_depth(root.left), max_depth(root.right))

from collections import deque
def level_order(root):
    if not root: return []
    result, q = [], deque([root])
    while q:
        level = []
        for _ in range(len(q)):
            node = q.popleft()
            level.append(node.val)
            if node.left: q.append(node.left)
            if node.right: q.append(node.right)
        result.append(level)
    return result
```

### 5. Graphs (BFS shortest path, topological sort)
```python
def topo_sort(num_nodes, edges):
    from collections import defaultdict, deque
    graph = defaultdict(list)
    indegree = [0] * num_nodes
    for u, v in edges:
        graph[u].append(v)
        indegree[v] += 1
    q = deque([n for n in range(num_nodes) if indegree[n] == 0])
    order = []
    while q:
        node = q.popleft()
        order.append(node)
        for nei in graph[node]:
            indegree[nei] -= 1
            if indegree[nei] == 0:
                q.append(nei)
    return order if len(order) == num_nodes else []  # empty = cycle detected
```

### 6. Dynamic Programming
```python
# Classic 1D DP: House Robber
def rob(nums):
    prev, curr = 0, 0
    for n in nums:
        prev, curr = curr, max(curr, prev + n)
    return curr

# Classic 2D DP: Longest Common Subsequence
def lcs(a, b):
    dp = [[0]*(len(b)+1) for _ in range(len(a)+1)]
    for i in range(1, len(a)+1):
        for j in range(1, len(b)+1):
            if a[i-1] == b[j-1]:
                dp[i][j] = dp[i-1][j-1] + 1
            else:
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
    return dp[-1][-1]
```

### 7. Backtracking
```python
def subsets(nums):
    result = []
    def backtrack(start, path):
        result.append(path[:])
        for i in range(start, len(nums)):
            path.append(nums[i])
            backtrack(i + 1, path)
            path.pop()
    backtrack(0, [])
    return result
```

### 8. Heap / Priority Queue
```python
import heapq
def k_closest_points(points, k):
    heap = [(x*x + y*y, x, y) for x, y in points]
    heapq.heapify(heap)
    return [(x, y) for _, x, y in heapq.nsmallest(k, heap)]
```

### 9. Trie
```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end = False

class Trie:
    def __init__(self):
        self.root = TrieNode()
    def insert(self, word):
        node = self.root
        for ch in word:
            node = node.children.setdefault(ch, TrieNode())
        node.is_end = True
    def search(self, word):
        node = self.root
        for ch in word:
            if ch not in node.children:
                return False
            node = node.children[ch]
        return node.is_end
```

### 10. Bit Manipulation
```python
def count_set_bits(n):
    count = 0
    while n:
        n &= (n - 1)  # clears the lowest set bit
        count += 1
    return count
```

---

## Practice Set (Blind 75 / NeetCode-style, organized by pattern)

| Pattern | Representative problems |
|---|---|
| Arrays & Hashing | Two Sum, Group Anagrams, Product of Array Except Self, Top K Frequent Elements |
| Two Pointers | Valid Palindrome, 3Sum, Container With Most Water |
| Sliding Window | Best Time to Buy/Sell Stock, Longest Substring Without Repeating Characters, Minimum Window Substring |
| Stack | Valid Parentheses, Daily Temperatures, Largest Rectangle in Histogram |
| Binary Search | Search in Rotated Sorted Array, Find Minimum in Rotated Sorted Array, Koko Eating Bananas |
| Linked List | Reverse Linked List, Merge Two Sorted Lists, Reorder List, Detect Cycle |
| Trees | Invert Binary Tree, Validate BST, Lowest Common Ancestor, Serialize/Deserialize Binary Tree |
| Tries | Implement Trie, Word Search II |
| Heap | Kth Largest Element, Merge K Sorted Lists, Find Median from Data Stream |
| Backtracking | Combination Sum, Word Search, Permutations, N-Queens |
| Graphs | Number of Islands, Clone Graph, Course Schedule, Pacific Atlantic Water Flow |
| DP (1D) | Climbing Stairs, House Robber, Coin Change, Longest Increasing Subsequence |
| DP (2D) | Unique Paths, Longest Common Subsequence, Edit Distance |
| Greedy | Jump Game, Gas Station, Task Scheduler |
| Intervals | Merge Intervals, Insert Interval, Non-overlapping Intervals |
| Bit Manipulation | Single Number, Number of 1 Bits, Counting Bits, Missing Number |

**Interview execution tips**: restate the problem in your own words, state a brute-force approach and its complexity *before* optimizing, ask about input constraints (can input be empty? negative? duplicates?), and narrate your complexity analysis at the end even if not asked.

---
*Part of the [AI Engineer Handbook](../../README.md) · [Interview Handbook](README.md) · Next: [SQL Guide](10_sql_guide.md).*
