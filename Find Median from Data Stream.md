---

tags:

- dsa
- heap
- design
- hard
- leetcode
- priority-queue
- two-heaps aliases:
- Find Median from Data Stream
- LeetCode 295 difficulty: Hard topic: Heap / Priority Queue pattern: Two Heaps date: 2026-04-28 related: "[[Median of Two Sorted Arrays]]"

---

# Find Median from Data Stream

## 📌 Problem Statement

> Design a data structure that supports:
> 
> - `addNum(int num)` — adds a number to the data stream
> - `findMedian()` — returns the median of all numbers added so far

**LeetCode #295** | Difficulty: 🔴 Hard

### Example

```
MedianFinder mf = new MedianFinder();

mf.addNum(1)      → stream: [1]         median = 1.0
mf.addNum(2)      → stream: [1, 2]      median = 1.5
mf.addNum(3)      → stream: [1, 2, 3]   median = 2.0
mf.addNum(4)      → stream: [1,2,3,4]   median = 2.5
```

---

## 🧠 Intuition — Why Two Heaps?

### What is a median?

The median splits the sorted stream into two equal halves:

```
Sorted stream:  [ 1  2  3  4  5  6 ]
                 ←left half→|←right half→
                      ↑          ↑
                    max=3      min=4
                 median = (3+4)/2 = 3.5
```

We need:

- **Max of the left half** (largest small number)
- **Min of the right half** (smallest large number)

These are exactly what **heaps** give us in O(1):

- A **Max-Heap** for the left half → top = max of left
- A **Min-Heap** for the right half → top = min of right

---

## 📐 Pictorial Concept

### The Two-Heap Structure

```
      MAX-HEAP (left)          MIN-HEAP (right)
      smaller half             larger half

         [3]                      [4]
        /   \                    /   \
      [2]   [1]               [5]   [6]

      top = 3                  top = 4
      (largest of left)        (smallest of right)

Median = (3 + 4) / 2.0 = 3.5
```

### Invariants we maintain at ALL times

```
1. maxHeap.size() == minHeap.size()          (even total)
   OR
   maxHeap.size() == minHeap.size() + 1      (odd total — left has extra)

2. maxHeap.peek() <= minHeap.peek()
   (every element in left ≤ every element in right)
```

---

## ➕ addNum Logic (Step by Step)

Every insertion follows **3 steps**:

```
Step 1: Always add to maxHeap first
Step 2: Balance — push maxHeap's top to minHeap
        (ensures maxHeap.top ≤ minHeap.top)
Step 3: If minHeap got too big → push minHeap's top back to maxHeap
        (ensures size invariant: maxHeap.size >= minHeap.size)
```

### Why this 3-step dance?

- Step 1 alone could violate invariant 2 (a large number goes to left)
- Step 2 fixes invariant 2 by always sending the left's maximum to the right
- Step 3 fixes invariant 1 by rebalancing sizes

---

## 📊 Trace: Adding Numbers One by One

### Stream: [5, 3, 8, 1, 9, 2]

---

#### addNum(5)

```
Step 1: maxHeap = [5]         minHeap = []
Step 2: push maxHeap.top(5) → minHeap = [5],  maxHeap = []
Step 3: minHeap.size(1) > maxHeap.size(0)
        push minHeap.top(5) → maxHeap = [5],  minHeap = []

maxHeap = [5]    minHeap = []
Median = 5.0  (odd → maxHeap.top)
```

---

#### addNum(3)

```
Step 1: maxHeap = [5, 3]      minHeap = []
Step 2: push maxHeap.top(5) → minHeap = [5],  maxHeap = [3]
Step 3: sizes equal (1 == 1) → no rebalance

maxHeap = [3]    minHeap = [5]
Median = (3 + 5) / 2.0 = 4.0
```

---

#### addNum(8)

```
Step 1: maxHeap = [8, 3]      minHeap = [5]
        (8 goes to maxHeap even though it's large — Step 2 fixes it)
Step 2: push maxHeap.top(8) → minHeap = [5, 8],  maxHeap = [3]
Step 3: minHeap.size(2) > maxHeap.size(1)
        push minHeap.top(5) → maxHeap = [5, 3],  minHeap = [8]

maxHeap = [5, 3]    minHeap = [8]
Median = 5.0  (odd → maxHeap.top)
```

---

#### addNum(1)

```
Step 1: maxHeap = [5, 3, 1]   minHeap = [8]
Step 2: push maxHeap.top(5) → minHeap = [5, 8],  maxHeap = [3, 1]
Step 3: sizes equal (2 == 2) → no rebalance

maxHeap = [3, 1]    minHeap = [5, 8]
Median = (3 + 5) / 2.0 = 4.0
```

---

#### addNum(9)

```
Step 1: maxHeap = [9, 3, 1]   minHeap = [5, 8]
Step 2: push maxHeap.top(9) → minHeap = [5, 8, 9],  maxHeap = [3, 1]
Step 3: minHeap.size(3) > maxHeap.size(2)
        push minHeap.top(5) → maxHeap = [5, 3, 1],  minHeap = [8, 9]

maxHeap = [5, 3, 1]    minHeap = [8, 9]
Median = 5.0  (odd → maxHeap.top)
```

---

#### addNum(2)

```
Step 1: maxHeap = [5, 3, 2, 1]   minHeap = [8, 9]
Step 2: push maxHeap.top(5) → minHeap = [5, 8, 9],  maxHeap = [3, 2, 1]
Step 3: sizes equal (3 == 3) → no rebalance

maxHeap = [3, 2, 1]    minHeap = [5, 8, 9]
Median = (3 + 5) / 2.0 = 4.0
```

### Final State Visualization

```
      MAX-HEAP (left)          MIN-HEAP (right)
         [3]                      [5]
        /   \                    /   \
      [2]   [1]               [8]   [9]

Median = (3 + 5) / 2.0 = 4.0 ✅
(sorted stream was [1,2,3,5,8,9] → median = (3+5)/2 = 4.0 ✅)
```

---

## ✅ Full Java Solution

```java
class MedianFinder {

    // Max-Heap: holds the smaller half
    private PriorityQueue<Integer> maxHeap;

    // Min-Heap: holds the larger half
    private PriorityQueue<Integer> minHeap;

    public MedianFinder() {
        maxHeap = new PriorityQueue<>(Collections.reverseOrder()); // Max-Heap
        minHeap = new PriorityQueue<>();                           // Min-Heap (default)
    }

    public void addNum(int num) {
        // Step 1: Always add to maxHeap first
        maxHeap.offer(num);

        // Step 2: Ensure max of left ≤ min of right
        minHeap.offer(maxHeap.poll());

        // Step 3: Rebalance sizes — maxHeap should have equal or one more element
        if (minHeap.size() > maxHeap.size()) {
            maxHeap.offer(minHeap.poll());
        }
    }

    public double findMedian() {
        if (maxHeap.size() > minHeap.size()) {
            return maxHeap.peek();              // odd total → middle element
        }
        return (maxHeap.peek() + minHeap.peek()) / 2.0; // even → average of two middles
    }
}
```

---

## 🔑 Key Formulas

```
Size invariant:
  maxHeap.size() == minHeap.size()      → even total
  maxHeap.size() == minHeap.size() + 1  → odd total

Order invariant:
  maxHeap.peek() <= minHeap.peek()

findMedian:
  odd  → maxHeap.peek()
  even → (maxHeap.peek() + minHeap.peek()) / 2.0

addNum (always 3 steps):
  maxHeap.offer(num)
  minHeap.offer(maxHeap.poll())
  if (minHeap.size() > maxHeap.size()) maxHeap.offer(minHeap.poll())
```

---

## 🪤 Common Mistakes

|Mistake|Fix|
|---|---|
|Adding directly to correct heap based on value|Always add to maxHeap first — Step 2 corrects placement|
|Forgetting Step 2|maxHeap could contain elements larger than minHeap's top|
|Forgetting Step 3|minHeap could become larger — median calc breaks|
|`(maxHeap.peek() + minHeap.peek()) / 2` (integer division)|Use `/ 2.0` to get decimal result|
|Using only one heap|Can't get O(1) median — need both max and min efficiently|

---

## ⏱️ Complexity Analysis

|Operation|Time|Space|
|---|---|---|
|`addNum`|O(log n)|—|
|`findMedian`|O(1)|—|
|Overall space|—|O(n)|

> Naive approach (sorted list + binary search insert) gives O(n) per insert.  
> Two heaps gives **O(log n)** insert and **O(1)** median — optimal.

---

## 💡 Follow-Up: What if numbers are in range [0, 100]?

Use a **frequency array** of size 101. Track count of each number.  
`findMedian` walks the array to find the middle — O(100) = O(1).

## 💡 Follow-Up: What if 99% of numbers are in [0, 100]?

Use the frequency array for in-range numbers + two heaps for outliers.

---

## 🔗 Related Notes

- [[Median of Two Sorted Arrays]]
- [[Kth Element of Two Sorted Arrays]]
- [[Sliding Window Median]]
- [[Priority Queue / Heap]]