# 460. LFU Cache
#REVISIT 
**Difficulty:** Hard **Topics:** Hash Table, Linked List, Design, Doubly-Linked List **Link:** https://leetcode.com/problems/lfu-cache/

---

## Problem Statement

Design and implement a data structure for a **Least Frequently Used (LFU)** cache.

Implement the `LFUCache` class:

- `LFUCache(int capacity)` — Initializes the object with the `capacity` of the data structure.
- `int get(int key)` — Gets the value of the `key` if it exists in the cache. Otherwise, returns `-1`.
- `void put(int key, int value)` — Update the value of `key` if present, or insert the `key-value` pair. When the cache reaches its `capacity`, it should **invalidate and remove the least frequently used key** before inserting a new item. For ties in frequency, the **least recently used** (LRU) key among them is evicted.

Both `get` and `put` must run in **O(1)** average time complexity.

---

## Examples

### Example 1

```
Input:
["LFUCache", "put", "put", "get", "put", "get", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [3], [4, 4], [1], [3], [4]]

Output:
[null, null, null, 1, null, -1, 3, null, -1, 3, 4]
```

**Explanation:**

```
LFUCache lfu = new LFUCache(2);
lfu.put(1, 1);   // cache={1:1}, cnt(1)=1
lfu.put(2, 2);   // cache={1:1, 2:2}, cnt(1)=1, cnt(2)=1
lfu.get(1);      // return 1
                 // cache={1:1, 2:2}, cnt(1)=2, cnt(2)=1
lfu.put(3, 3);   // capacity reached → evict key 2 (least freq, then least recent)
                 // cache={1:1, 3:3}, cnt(1)=2, cnt(3)=1
lfu.get(2);      // return -1 (not found)
lfu.get(3);      // return 3
                 // cache={1:1, 3:3}, cnt(1)=2, cnt(3)=2
lfu.put(4, 4);   // capacity reached → evict key 1 (both freq=2 but key 1 is LRU)
                 // cache={3:3, 4:4}, cnt(3)=2, cnt(4)=1
lfu.get(1);      // return -1 (not found)
lfu.get(3);      // return 3
lfu.get(4);      // return 4
```

---

## Constraints

- `1 <= capacity <= 10^4`
- `0 <= key <= 10^5`
- `0 <= value <= 10^9`
- At most `2 * 10^5` calls will be made to `get` and `put`.

---

## Intuition

The LFU cache evicts the **least frequently used** key. When there's a tie in frequency, it evicts the **least recently used** among those — making it a combination of frequency tracking and recency tracking.

### Key insight:

We need **three** data structures working together:

1. **`key → (value, freq)`** — To look up a key's value and current frequency in O(1).
2. **`freq → OrderedSet of keys`** — To know which keys have a given frequency, in insertion order (so we can evict the LRU among them) in O(1).
3. **`minFreq`** — Track the current minimum frequency so we always know what to evict.

When a key is accessed (get or put), its frequency increases by 1, so it moves from `freq[f]` to `freq[f+1]`. We use a **doubly linked list** (or `LinkedHashSet` / `OrderedDict`) per frequency bucket to maintain recency within the same frequency.

---

## Data Structures Design

```
keyMap:   { key → Node(value, freq) }
freqMap:  { freq → DoublyLinkedList of keys (most recent at tail) }
minFreq:  int
```

A `DoublyLinkedList` per frequency bucket maintains **O(1) insert at tail** and **O(1) delete from head** (LRU end).

```
freqMap[1]:  [key_A] ← [key_B] ← [key_C]   (key_A is LRU, key_C is MRU)
freqMap[2]:  [key_D] ← [key_E]
```

---

## Step-by-Step Algorithm

### `get(key)`:

1. If `key` not in `keyMap` → return `-1`
2. Increment `key`'s frequency (call `_increment(key)`)
3. Return `keyMap[key].val`

### `put(key, value)`:

1. If `capacity == 0` → return
2. If `key` already in `keyMap`:
    - Update its value
    - Call `_increment(key)` (frequency goes up)
3. Else (new key):
    - If cache is full → evict LRU from `freqMap[minFreq]` (head of that list)
    - Insert new key with `freq = 1`
    - Set `minFreq = 1`

### `_increment(key)`:

1. Get current freq `f` from `keyMap[key]`
2. Remove key from `freqMap[f]`
3. If `freqMap[f]` is now empty and `f == minFreq` → `minFreq++`
4. Increment `keyMap[key].freq` to `f+1`
5. Add key to `freqMap[f+1]` (at tail = most recent)

---

## Java Solution (O(1) per operation)

```java
import java.util.*;

class LFUCache {

    // Node stores key, value, and current frequency
    private static class Node {
        int key, val, freq;
        Node prev, next;

        Node(int key, int val) {
            this.key = key;
            this.val = val;
            this.freq = 1;
        }
    }

    // Doubly Linked List: head = LRU (eviction side), tail = MRU
    private static class DLinkedList {
        Node head, tail;
        int size;

        DLinkedList() {
            head = new Node(0, 0); // sentinel head
            tail = new Node(0, 0); // sentinel tail
            head.next = tail;
            tail.prev = head;
            size = 0;
        }

        // Add node right before the tail (most recently used position)
        void addToTail(Node node) {
            node.prev = tail.prev;
            node.next = tail;
            tail.prev.next = node;
            tail.prev = node;
            size++;
        }

        // Remove a specific node from the list
        void removeNode(Node node) {
            node.prev.next = node.next;
            node.next.prev = node.prev;
            size--;
        }

        // Remove and return the LRU node (node right after head)
        Node removeLRU() {
            if (size == 0) return null;
            Node lru = head.next;
            removeNode(lru);
            return lru;
        }

        boolean isEmpty() {
            return size == 0;
        }
    }

    private final int capacity;
    private int minFreq;
    private final Map<Integer, Node> keyMap;            // key → Node
    private final Map<Integer, DLinkedList> freqMap;    // freq → DLinkedList

    public LFUCache(int capacity) {
        this.capacity = capacity;
        this.minFreq = 0;
        this.keyMap = new HashMap<>();
        this.freqMap = new HashMap<>();
    }

    public int get(int key) {
        if (!keyMap.containsKey(key)) return -1;
        Node node = keyMap.get(key);
        increment(node);
        return node.val;
    }

    public void put(int key, int value) {
        if (capacity == 0) return;

        if (keyMap.containsKey(key)) {
            // Key exists: update value and increment frequency
            Node node = keyMap.get(key);
            node.val = value;
            increment(node);
        } else {
            // Evict if at capacity
            if (keyMap.size() == capacity) {
                DLinkedList minList = freqMap.get(minFreq);
                Node evicted = minList.removeLRU();
                keyMap.remove(evicted.key);
            }

            // Insert new node with freq = 1
            Node newNode = new Node(key, value);
            keyMap.put(key, newNode);
            freqMap.computeIfAbsent(1, k -> new DLinkedList()).addToTail(newNode);
            minFreq = 1; // New key always has freq=1, which is the new minimum
        }
    }

    // Move a node from its current frequency bucket to the next one
    private void increment(Node node) {
        int f = node.freq;
        DLinkedList oldList = freqMap.get(f);
        oldList.removeNode(node);

        // If the minimum frequency bucket is now empty, increment minFreq
        if (f == minFreq && oldList.isEmpty()) {
            minFreq++;
        }

        // Move node to freq+1 bucket
        node.freq++;
        freqMap.computeIfAbsent(node.freq, k -> new DLinkedList()).addToTail(node);
    }
}
```

---

## Dry Run

**Input:** `capacity = 2`

|Operation|keyMap|freqMap|minFreq|Output|
|---|---|---|---|---|
|`put(1, 1)`|{1:(val=1,f=1)}|{1:[1]}|1|—|
|`put(2, 2)`|{1:(f=1), 2:(f=1)}|{1:[1,2]}|1|—|
|`get(1)`|{1:(f=2), 2:(f=1)}|{1:[2], 2:[1]}|1|1|
|`put(3, 3)`|evict key=2 (minFreq=1, LRU)|{1:[3], 2:[1]}|1|—|
||{1:(f=2), 3:(f=1)}||||
|`get(2)`|—|—|—|-1|
|`get(3)`|{1:(f=2), 3:(f=2)}|{2:[1,3]}|2|3|
|`put(4, 4)`|evict key=1 (minFreq=2, LRU)|{2:[3], 1:[4]}|1|—|
||{3:(f=2), 4:(f=1)}||||
|`get(1)`|—|—|—|-1|
|`get(3)`|{3:(f=3), 4:(f=1)}|{3:[3], 1:[4]}|1|3|
|`get(4)`|{3:(f=3), 4:(f=2)}|{3:[3], 2:[4]}|2|4|

---

## Complexity Analysis

|Operation|Time Complexity|Space Complexity|
|---|---|---|
|`get`|O(1)|O(capacity)|
|`put`|O(1)|O(capacity)|

- **Time:** Every operation — node lookup, list insertion, list deletion — is O(1) due to hash maps and doubly linked list pointers.
- **Space:** O(capacity) for `keyMap` + O(capacity) total nodes across all `freqMap` lists.

---

## Why Not Simpler Approaches?

|Approach|`get`/`put`|Why it fails|
|---|---|---|
|Sorted array by freq|O(n log n)|Sorting on every update is too slow|
|Min-heap|O(log n)|Heap operations are O(log n), not O(1)|
|Single HashMap only|O(n)|Can't find min-freq key in O(1)|
|**Doubly linked list + two HashMaps**|**O(1)** ✅|**Optimal**|

---

## Alternative: Python Solution (using `OrderedDict`)

Python's `collections.OrderedDict` maintains insertion order, giving O(1) `move_to_end` and `popitem(last=False)` — a clean substitute for a doubly linked list.

```python
from collections import defaultdict, OrderedDict

class LFUCache:
    def __init__(self, capacity: int):
        self.capacity = capacity
        self.min_freq = 0
        self.key_map = {}                              # key → [value, freq]
        self.freq_map = defaultdict(OrderedDict)       # freq → OrderedDict{key: None}

    def get(self, key: int) -> int:
        if key not in self.key_map:
            return -1
        self._increment(key)
        return self.key_map[key][0]

    def put(self, key: int, value: int) -> None:
        if self.capacity == 0:
            return
        if key in self.key_map:
            self.key_map[key][0] = value
            self._increment(key)
        else:
            if len(self.key_map) == self.capacity:
                # Evict LRU from min_freq bucket
                evict_key, _ = self.freq_map[self.min_freq].popitem(last=False)
                del self.key_map[evict_key]

            self.key_map[key] = [value, 1]
            self.freq_map[1][key] = None
            self.min_freq = 1

    def _increment(self, key: int) -> None:
        val, freq = self.key_map[key]
        del self.freq_map[freq][key]
        if not self.freq_map[freq] and freq == self.min_freq:
            self.min_freq += 1
        self.key_map[key][1] += 1
        self.freq_map[freq + 1][key] = None
```

---

## Common Mistakes to Avoid

1. **Forgetting to update `minFreq` after eviction** — When inserting a new key, `minFreq` must always reset to `1`.
2. **Not checking if `freqMap[f]` is empty before incrementing `minFreq`** — Only increment `minFreq` if the current `minFreq` bucket becomes empty.
3. **Using a regular HashMap for the frequency bucket** — A regular HashMap doesn't preserve insertion order, so you can't efficiently find the LRU key within a frequency group.
4. **Off-by-one in eviction** — Eviction happens _before_ inserting the new key, so the size check should be `keyMap.size() == capacity`, not `> capacity`.
5. **Not handling `capacity = 0`** — Always guard against this edge case at the start of `put`.

---

## Similar Problems

|#|Problem|Key Idea|
|---|---|---|
|146|LRU Cache|Single frequency, evict LRU|
|460|**LFU Cache** ← You are here|Multi-frequency, evict LFU then LRU|
|432|All O(1) Data Structure|Inc/dec and get max/min in O(1)|
|716|Max Stack|Pop max in O(1)|