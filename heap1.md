# 📚 Complete Notes: Heaps & Priority Queues (with Examples)

> Based on the lecture transcript (Hindi) + the C++ code file. Every concept is explained with a **dry-run example**.

---

## 1. What is a Heap?

A **heap** is a **complete binary tree** that satisfies the **heap property**.

| Type | Property | Example (valid) |
|------|----------|-----------------|
| **Max Heap** | Every parent ≥ its children | `[55, 54, 53, 52, 50]` |
| **Min Heap** | Every parent ≤ its children | `[2, 5, 10, 20, 12]` |

**Two things must hold:**
1. It is a **complete binary tree**.
2. Parent–child ordering follows the heap property.

> ⚠️ A heap is **NOT** a BST. Left child need not be smaller than right child — only parent vs child matters.

---

## 2. Complete Binary Tree (CBT) — Definition

> All levels are **completely filled**, except possibly the last level, and the last level is filled **strictly from left to right**.

### Example — Which is complete?

**✅ Complete:**
```
        1
      /   \
     2     3
    / \   /
   4   5 6
```
All levels full except last; last level filled left→right (`4,5,6`).

**❌ NOT complete (gap on left):**
```
        1
      /   \
     2     3
      \   /
       5 6
```
Node `2` has a **right child but no left child** → violates "fill from left".

**❌ NOT complete (right filled, left missing):**
```
        1
      /   \
     2     3
    / \     \
   4   5     7
```
Last level has a **gap at position of 6** but `7` exists → not left-filled.

### Why "left-filled" matters
When we insert a new node, it **must go to the first empty spot from the left**. That's why array representation works perfectly (no gaps).

---

## 3. Array Representation (1-Based Indexing)

For a node at index `i`:

| Relation | Formula | Example (`i = 2`) |
|----------|---------|-------------------|
| Left child | `2 * i` | `4` |
| Right child | `2 * i + 1` | `5` |
| Parent | `i / 2` | `1` |
| Root | `1` | — |
| Leaf nodes | `n/2 + 1` … `n` | — |
| Non-leaf nodes | `1` … `n/2` | — |

### Example mapping
Heap array: `arr = [-1, 55, 54, 53, 52, 50]` (index 0 unused)

```
Index:   1    2    3    4    5
Value:  55   54   53   52   50

Tree:
            55 (i=1)
          /          \
       54 (i=2)     53 (i=3)
       /     \
   52 (i=4)  50 (i=5)
```

Check: left child of `i=2` is `i=4` → value `52` ✅; right child of `i=2` is `i=5` → `50` ✅.

### Why n/2 for leaves?
For `n = 5`: leaves = indices `5/2 + 1 = 3` to `5` → indices `3, 4, 5`. Verify: `53`, `52`, `50` are all leaves ✅. So **heapify only needs to run on indices `1 … n/2`** (here `1 … 2`).

---

## 4. Max Heap Class — Implementation

```cpp
class heap {
public:
    int arr[100];
    int size;

    heap() {
        arr[0] = -1;   // dummy, 1-based indexing
        size = 0;
    }
```

---

## 5. Insertion in Heap — `O(log n)`

### Algorithm
1. Place new value at the **end** (`index = size`).
2. Compare with **parent** (`index/2`).
3. If parent is **smaller** (max heap), **swap** and move up.
4. Repeat until heap property restored or root reached.

```cpp
void insert(int val) {
    size = size + 1;
    int index = size;
    arr[index] = val;

    while (index > 1) {
        int parent = index / 2;
        if (arr[parent] < arr[index]) {
            swap(arr[parent], arr[index]);
            index = parent;              // ⚠️ must update index
        } else {
            return;
        }
    }
}
```

> ⚠️ **Bug in original code:** `index = parent` is missing after swap → infinite loop. Always update `index`.

### Dry Run — Insert 50, 55, 53, 52, 54

| Step | Action | Array (1-based) | Explanation |
|------|--------|-----------------|-------------|
| 1 | insert 50 | `[50]` | First element → root |
| 2 | insert 55 | `[55, 50]` | 55 > 50 → swap with parent |
| 3 | insert 53 | `[55, 50, 53]` | 53 < 55 → no swap |
| 4 | insert 52 | `[55, 50, 53, 52]` | 52 < 50? No → stop |
| 5 | insert 54 | `[55, 54, 53, 52, 50]` | 54 > 50 (parent) → swap |

**Final Max Heap:** `[55, 54, 53, 52, 50]` ✅

```
        55
      /    \
    54      53
   /  \
  52   50
```

---

## 6. Deletion (Delete Root) — `O(log n)`

### Algorithm (3 steps)
1. **Put last element at root**: `arr[1] = arr[size]`.
2. **Remove last element**: `size--`.
3. **Heapify root** down to its correct position.

```cpp
void deleteFromHeap() {
    if (size == 0) {
        cout << "Nothing to delete" << endl;
        return;                       // ⚠️ must return
    }

    arr[1] = arr[size];               // step 1
    size--;                           // step 2

    int index = 1;                    // step 3
    while (index < size) {
        int leftIndex  = 2 * index;
        int rightIndex = 2 * index + 1;

        if (leftIndex < size && arr[leftIndex] > arr[index]) {
            swap(arr[leftIndex], arr[index]);
            index = leftIndex;
        }
        else if (rightIndex < size && arr[rightIndex] > arr[index]) {
            swap(arr[rightIndex], arr[index]);
            index = rightIndex;
        }
        else return;
    }
}
```

> ⚠️ Note: `leftIndex < size` should ideally be `leftIndex <= size`. Minor boundary bug in original.

### Dry Run — Delete root from `[55, 54, 53, 52, 50]`

| Step | Action | Array | Explanation |
|------|--------|-------|-------------|
| Start | — | `[55, 54, 53, 52, 50]` | Max heap |
| 1 | `arr[1] = arr[5]` | `[50, 54, 53, 52]` | 50 placed at root |
| 2 | `size = 4` | `[50, 54, 53, 52]` | Last element removed |
| 3 | heapify root | `[54, 52, 53, 50]` | 50 < 54 → swap; 50 < 52 → swap |

**Result:** `[54, 52, 53, 50]` ✅ (verify: 54 ≥ 52, 53 ✅; 52 ≥ 50 ✅)

---

## 7. Heapify Algorithm — `O(log n)`

Heapify fixes a **single violated node** by pushing it **down** to its correct position.

```cpp
void heapify(int arr[], int n, int i) {
    int largest = i;
    int left  = 2 * i;
    int right = 2 * i + 1;

    if (left <= n && arr[largest] < arr[left])
        largest = left;

    if (right <= n && arr[largest] < arr[right])
        largest = right;

    if (largest != i) {
        swap(arr[largest], arr[i]);
        heapify(arr, n, largest);      // recurse on affected subtree
    }
}
```

### Dry Run — heapify(arr, 5, 1) on `[-1, 54, 53, 55, 52, 50]`

`i = 1` (value 54), left = 2 (53), right = 3 (55)
- 54 < 53? No. 54 < 55? Yes → largest = 3.
- Swap `arr[1]` ↔ `arr[3]` → `[-1, 55, 53, 54, 52, 50]`
- Recurse `heapify(arr, 5, 3)`: node 54, left = 6 (> n), right = 7 (> n) → stop.

**Result:** root is now 55 ✅

---

## 8. Building a Max Heap from an Arbitrary Array — `O(n)`

```cpp
for (int i = n/2; i > 0; i--)   // only non-leaf nodes
    heapify(arr, n, i);
```

**Why start at `n/2`?** Leaf nodes (indices `n/2 + 1 … n`) are already valid heaps (no children). So we only heapify internal nodes.

### Dry Run — Build heap from `[-1, 54, 53, 55, 52, 50]`, n = 5

Initial: `[54, 53, 55, 52, 50]` (ignoring index 0)

```
        54
      /    \
    53      55
   /  \
  52   50
```

**i = 2** (node 53): left = 4 (52), right = 5 (50). 53 > 52, 53 > 50 → no swap.

**i = 1** (node 54): left = 2 (53), right = 3 (55). 54 < 55 → swap with 55.
→ `[55, 53, 54, 52, 50]`
Recurse `heapify(..., 3)`: node 54 has no children → stop.

**Final Max Heap:** `[55, 53, 54, 52, 50]`

```
        55
      /    \
    53      54
   /  \
  52   50
```
✅ Verify: 55 ≥ 53, 54 ✅; 53 ≥ 52, 50 ✅.

**Time complexity:** Sum over nodes of height ≈ **O(n)** (not O(n log n)).

---

## 9. Heap Sort — `O(n log n)`

### Algorithm
1. Build a **max heap**.
2. Swap root (max) with last element → largest goes to its final sorted position.
3. Reduce heap size by 1, heapify root.
4. Repeat while size > 1.

```cpp
void heapSort(int arr[], int n) {
    int size = n;
    while (size > 1) {
        swap(arr[size], arr[1]);      // step 1
        size--;                       // step 2
        heapify(arr, size, 1);        // step 3
    }
}
```

### Dry Run — Sort `[55, 53, 54, 52, 50]`

| Iter | Swap root↔last | After swap | Heapify | Result |
|------|----------------|------------|---------|--------|
| 1 | `55 ↔ 50` | `[50, 53, 54, 52 \| 55]` | `[54, 53, 50, 52]` | `[54,53,50,52 \| 55]` |
| 2 | `54 ↔ 52` | `[52, 53, 50 \| 54, 55]` | `[53, 52, 50]` | `[53,52,50 \| 54,55]` |
| 3 | `53 ↔ 50` | `[50, 52 \| 53,54,55]` | `[52, 50]` | `[52,50 \| 53,54,55]` |
| 4 | `52 ↔ 50` | `[50 \| 52,53,54,55]` | size = 1, stop | `[50,52,53,54,55]` |

**Sorted (ascending):** `[50, 52, 53, 54, 55]` ✅

> Max heap + swap-to-end ⇒ **ascending** order.  
> Min heap + swap-to-end ⇒ **descending** order.

---

## 10. Priority Queue (STL) — Internally a Heap

### Max Heap (default)

```cpp
priority_queue<int> pq;
pq.push(10); pq.push(2); pq.push(5); pq.push(20); pq.push(12);

cout << pq.top();   // 20
pq.pop();           // removes 20
cout << pq.size();  // 4
```

### Min Heap

```cpp
priority_queue<int, vector<int>, greater<int>> minHeap;
minHeap.push(10); minHeap.push(2); minHeap.push(5); minHeap.push(20); minHeap.push(12);

cout << minHeap.top();   // 2
minHeap.pop();           // removes 2
cout << minHeap.size();  // 4
```

### Operation table

| Operation | Meaning | Complexity | Example |
|-----------|---------|------------|---------|
| `push(x)` | insert | O(log n) | `pq.push(20)` |
| `pop()` | remove top | O(log n) | `pq.pop()` |
| `top()` | peek max/min | O(1) | `pq.top() → 20` |
| `size()` | count | O(1) | `pq.size() → 5` |
| `empty()` | check | O(1) | `pq.empty() → false` |

---

## 11. Full Program Output (from code)

```
Inserting 50 55 53 52 54:
55 54 53 52 50

After deleteFromHeap():
54 52 53 50

Printing Max Heap:
55 53 54 52 50

Printing After Heap Sort:
50 52 53 54 55

Top element (max heap): 20
Size of pq: 4
Top element (min heap): 2
Size of pq: 4
```

---

## 12. Complexity Summary

| Operation | Time | Space |
|-----------|------|-------|
| Insert | O(log n) | O(1) |
| Delete root | O(log n) | O(1) |
| Heapify (one node) | O(log n) | O(log n) stack |
| Build heap | **O(n)** | O(1) |
| Heap sort | O(n log n) | O(1) |
| Peek top | O(1) | O(1) |
| STL push/pop | O(log n) | O(1) |

---

## 13. Interview Q&A (with short examples)

**Q1. What is a heap?**  
→ Complete binary tree + heap property. Max heap: `parent ≥ children` e.g. `[55,54,53,52,50]`.

**Q2. What is a complete binary tree?**  
→ All levels full except last; last filled **left→right**. Example: `[1,2,3,4,5,6]` ✅, but `[1,2,3,4,-,6]` ❌ (gap at index 5).

**Q3. Heap vs BST?**  
→ BST: left < root < right (sorted). Heap: only parent–child ordering; siblings unordered. Example: in heap `[55, 54, 53]`, 54 and 53 have no ordering relation.

**Q4. Why is build-heap O(n)?**  
→ Most nodes are near leaves; heapify cost = height. Sum `Σ (n/2^h) × h` converges to **O(n)**.

**Q5. How to delete an arbitrary node?**  
→ Replace with last node, decrease size, then **heapify down** if smaller than children, or **bubble up** if larger than parent.

**Q6. Why `n/2` for leaf nodes?**  
→ For n=5, leaves = `3,4,5` = `n/2+1 … n`. So only `1…n/2` = `1,2` need heapify.

**Q7. Heap vs Priority Queue?**  
→ Priority Queue is an **ADT**; heap is the **implementation**. STL `priority_queue` uses a heap internally.

**Q8. Where are heaps used?**  
→ Priority scheduling, Dijkstra, K largest/smallest, median stream, task schedulers, top-K queries.

**Q9. Heap sort vs Quick sort?**  
→ Heap sort: O(n log n) worst case, in-place, not stable. Quick sort: O(n log n) average, O(n²) worst.

**Q10. Can a heap be a BST?**  
→ Only in trivial cases (≤ 2 nodes). Generally they contradict.

---

## 14. Common Pitfalls (from code review)

| ❌ Bug | ✅ Fix |
|--------|-------|
| Missing `index = parent` in insert | Update index after swap |
| No `return` after `size == 0` in delete | Add `return` |
| `leftIndex < size` in delete loop | Use `<= size` |
| Starting build-heap from `i = n` | Start from `i = n/2` |
| Using 0-based indexing with `2i, 2i+1` | Either use 1-based, or use `2i+1, 2i+2` |

---

## 15. One-Page Cheat Sheet

```
HEAP = Complete Binary Tree + Heap Property
  Max Heap : parent ≥ children   → [55,54,53,52,50]
  Min Heap : parent ≤ children   → [2,5,10,20,12]

Array (1-based):  left=2i, right=2i+1, parent=i/2
Leaves: n/2+1 … n      Non-leaves: 1 … n/2

Insert   → add at end, bubble up           O(log n)
Delete   → last→root, size--, heapify down  O(log n)
Heapify  → push node down                  O(log n)
Build    → heapify i=n/2 … 1               O(n)
HeapSort → swap root↔last, size--, heapify O(n log n)

Max heap + swap-to-end  → ascending sort
Min heap + swap-to-end  → descending sort

STL:
  priority_queue<int> pq;                                  // max heap
  priority_queue<int, vector<int>, greater<int>> minHeap;  // min heap
  push O(log n) | pop O(log n) | top O(1)
```
