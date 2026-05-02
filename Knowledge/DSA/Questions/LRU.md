---

tags:

- dsa
- design
- medium
- leetcode
- linked-list
- hashmap
- cache aliases:
- LRU Cache
- Least Recently Used Cache
- LeetCode 146 difficulty: Medium topic: Design / Linked List + HashMap pattern: Doubly Linked List + HashMap date: 2026-04-28

---

# LRU Cache

## 📌 Problem Statement

> Design a data structure that follows the **Least Recently Used (LRU)** cache eviction policy.
> 
> Implement the `LRUCache` class:
> 
> - `LRUCache(int capacity)` — initializes the cache with a given capacity
> - `int get(int key)` — returns the value if the key exists, else `-1`
> - `void put(int key, int value)` — inserts or updates the key. If capacity is exceeded, **evict the least recently used key**
> 
> Both operations must run in **O(1)** average time.

**LeetCode #146** | Difficulty: 🟡 Medium

### Example

```
LRUCache cache = new LRUCache(2);  // capacity = 2

cache.put(1, 1)   // cache: {1=1}
cache.put(2, 2)   // cache: {1=1, 2=2}
cache.get(1)      // returns 1,  cache: {2=2, 1=1}  ← 1 is now most recent
cache.put(3, 3)   // evicts key 2 (LRU), cache: {1=1, 3=3}
cache.get(2)      // returns -1 (not found)
cache.get(3)      // returns 3,  cache: {1=1, 3=3}
cache.put(4, 4)   // evicts key 1 (LRU), cache: {3=3, 4=4}
cache.get(1)      // returns -1 (not found)
cache.get(3)      // returns 3
cache.get(4)      // returns 4
```

---

## 🧠 Intuition

We need two things simultaneously:

- **O(1) lookup** by key → `HashMap`
- **O(1) track of usage order** (move to front, evict from back) → `Doubly Linked List`

Neither alone is enough:

- HashMap alone → can't track order
- Linked List alone → O(n) lookup

**Combined → O(1) for both.**

---

## 📐 Data Structure Design

### The Node

```
class Node {
    int key, val;
    Node prev, next;
}
```

### The Overall Structure

```
HashMap<key → Node>   +   Doubly Linked List (ordered by recency)

                MOST RECENT                  LEAST RECENT
                    ↓                              ↓
  [HEAD] ←→ [Node A] ←→ [Node B] ←→ [Node C] ←→ [TAIL]
  (dummy)                                         (dummy)

HEAD.next = Most Recently Used
TAIL.prev = Least Recently Used (eviction candidate)
```

We use **dummy HEAD and TAIL** nodes to avoid null checks at boundaries.

---

## 📐 Pictorial: Operations

### Initial State (capacity = 2)

```
HEAD ←→ TAIL
```

---

### put(1, 1)

```
Insert Node(1,1) at front (after HEAD)

HEAD ←→ [1,1] ←→ TAIL

HashMap: { 1 → Node(1,1) }
```

---

### put(2, 2)

```
Insert Node(2,2) at front

HEAD ←→ [2,2] ←→ [1,1] ←→ TAIL

HashMap: { 1 → Node(1,1), 2 → Node(2,2) }
```

---

### get(1) → returns 1

```
Key 1 found → move Node(1,1) to front

Before: HEAD ←→ [2,2] ←→ [1,1] ←→ TAIL
After:  HEAD ←→ [1,1] ←→ [2,2] ←→ TAIL
                  ↑
             most recent
```

---

### put(3, 3) — capacity exceeded!

```
Evict TAIL.prev = Node(2,2)   ← Least Recently Used

Before: HEAD ←→ [1,1] ←→ [2,2] ←→ TAIL
Remove Node(2,2):
        HEAD ←→ [1,1] ←→ TAIL

Insert Node(3,3) at front:
        HEAD ←→ [3,3] ←→ [1,1] ←→ TAIL

HashMap: { 1 → Node(1,1), 3 → Node(3,3) }
         (key 2 removed)
```

---

### get(2) → returns -1

```
Key 2 not in HashMap → return -1
```

---

### put(4, 4) — capacity exceeded again!

```
Evict TAIL.prev = Node(1,1)

Before: HEAD ←→ [3,3] ←→ [1,1] ←→ TAIL
Remove Node(1,1):
        HEAD ←→ [3,3] ←→ TAIL

Insert Node(4,4) at front:
        HEAD ←→ [4,4] ←→ [3,3] ←→ TAIL

HashMap: { 3 → Node(3,3), 4 → Node(4,4) }
```

---

## 🔧 Core Helper Methods

### removeNode(node) — detach a node from the list

```
node.prev.next = node.next
node.next.prev = node.prev

Before: ... ←→ [prev] ←→ [node] ←→ [next] ←→ ...
After:  ... ←→ [prev] ←→ [next] ←→ ...
```

### insertFront(node) — insert right after HEAD

```
node.next = head.next
node.prev = head
head.next.prev = node
head.next = node

Before: HEAD ←→ [first] ←→ ...
After:  HEAD ←→ [node] ←→ [first] ←→ ...
```

### moveToFront(node) — mark as most recently used

```
removeNode(node)
insertFront(node)
```

---

## ✅ Full Java Solution

```java
class LRUCache {

    // Doubly Linked List Node
    class Node {
        int key, val;
        Node prev, next;

        Node(int key, int val) {
            this.key = key;
            this.val = val;
        }
    }

    private int capacity;
    private Map<Integer, Node> map;   // key → Node
    private Node head, tail;          // dummy head and tail

    public LRUCache(int capacity) {
        this.capacity = capacity;
        this.map = new HashMap<>();

        // Dummy sentinel nodes — avoid null checks
        head = new Node(0, 0);
        tail = new Node(0, 0);
        head.next = tail;
        tail.prev = head;
    }

    public int get(int key) {
        if (!map.containsKey(key)) return -1;

        Node node = map.get(key);
        moveToFront(node);     // mark as most recently used
        return node.val;
    }

    public void put(int key, int value) {
        if (map.containsKey(key)) {
            // Update existing node
            Node node = map.get(key);
            node.val = value;
            moveToFront(node);
        } else {
            // Insert new node
            Node newNode = new Node(key, value);
            map.put(key, newNode);
            insertFront(newNode);

            // Evict LRU if over capacity
            if (map.size() > capacity) {
                Node lru = tail.prev;       // least recently used
                removeNode(lru);
                map.remove(lru.key);        // remove from map too!
            }
        }
    }

    // ─── Helper Methods ────────────────────────────────────

    private void removeNode(Node node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }

    private void insertFront(Node node) {
        node.next = head.next;
        node.prev = head;
        head.next.prev = node;
        head.next = node;
    }

    private void moveToFront(Node node) {
        removeNode(node);
        insertFront(node);
    }
}
```

---

## 📊 Step-by-Step Trace (Full Example)

|Operation|Action|List State (MRU → LRU)|HashMap Keys|Return|
|---|---|---|---|---|
|`put(1,1)`|Insert 1 at front|`[1]`|`{1}`|—|
|`put(2,2)`|Insert 2 at front|`[2,1]`|`{1,2}`|—|
|`get(1)`|Move 1 to front|`[1,2]`|`{1,2}`|`1`|
|`put(3,3)`|Evict 2 (LRU), insert 3|`[3,1]`|`{1,3}`|—|
|`get(2)`|Not found|`[3,1]`|`{1,3}`|`-1`|
|`get(3)`|Move 3 to front|`[3,1]`|`{1,3}`|`3`|
|`put(4,4)`|Evict 1 (LRU), insert 4|`[4,3]`|`{3,4}`|—|
|`get(1)`|Not found|`[4,3]`|`{3,4}`|`-1`|
|`get(3)`|Move 3 to front|`[3,4]`|`{3,4}`|`3`|
|`get(4)`|Move 4 to front|`[4,3]`|`{3,4}`|`4`|

---

## 🔑 Why Dummy HEAD and TAIL?

Without dummy nodes, every insert/remove needs null checks:

```java
// Without dummy — messy null checks needed everywhere
if (node.prev != null) node.prev.next = node.next;
if (node.next != null) node.next.prev = node.prev;
if (node == head) head = node.next;
```

With dummy nodes — always clean:

```java
// With dummy — no null checks ever
node.prev.next = node.next;
node.next.prev = node.prev;
```

The dummy HEAD and TAIL are never removed — they act as stable anchors.

---

## 🪤 Common Mistakes

|Mistake|Fix|
|---|---|
|Forgetting to remove evicted key from HashMap|Always `map.remove(lru.key)` after `removeNode(lru)`|
|Not moving node to front on `get`|Every access makes it MRU — `moveToFront` on every hit|
|Not moving node to front on `put` update|Updating an existing key also counts as a use|
|Evicting before inserting|Insert first, then check `map.size() > capacity`|
|Using `LinkedHashMap` in interview|Know the manual DLL + HashMap approach — interviewers expect it|
|`tail.next` instead of `tail.prev` for LRU|LRU is at `tail.prev` (just before dummy tail)|

---

## 💡 Alternative: LinkedHashMap (Cheating 😄)

Java's `LinkedHashMap` maintains insertion order and can be made access-ordered:

```java
class LRUCache extends LinkedHashMap<Integer, Integer> {
    private int capacity;

    public LRUCache(int capacity) {
        super(capacity, 0.75f, true); // true = access-order
        this.capacity = capacity;
    }

    public int get(int key) {
        return super.getOrDefault(key, -1);
    }

    public void put(int key, int value) {
        super.put(key, value);
    }

    @Override
    protected boolean removeEldestEntry(Map.Entry<Integer, Integer> eldest) {
        return size() > capacity;
    }
}
```

> ⚠️ This works but **interviewers want the manual DLL + HashMap approach**.  
> Use `LinkedHashMap` only if explicitly allowed.

---

## ⏱️ Complexity Analysis

|Operation|Time|Reason|
|---|---|---|
|`get`|O(1)|HashMap lookup + DLL pointer update|
|`put`|O(1)|HashMap insert + DLL insert/remove|
|Space|O(capacity)|HashMap + DLL store at most `capacity` nodes|

---

## 🔗 Similar Design Problems

|Problem|Key Data Structure|
|---|---|
|**LRU Cache**|HashMap + Doubly Linked List|
|LFU Cache (LC 460)|HashMap + Doubly Linked List + Frequency Map|
|Design Twitter|HashMap + PriorityQueue|
|Snapshot Array|HashMap + Binary Search|
|Time-Based Key-Value Store|HashMap + Binary Search|

---

## 🔗 Related Notes

- [[Find Median from Data Stream]]
- [[Design HashMap]]
- [[Doubly Linked List]]
- [[HashMap]]