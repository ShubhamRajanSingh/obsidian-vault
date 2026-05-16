# 173. Binary Search Tree Iterator

**Difficulty:** Medium **Topics:** Stack, Tree, Design, Binary Search Tree, Iterator **Link:** https://leetcode.com/problems/binary-search-tree-iterator/

---

## Problem Statement

Implement the `BSTIterator` class that represents an iterator over the **in-order traversal** of a BST:

- `BSTIterator(TreeNode root)` — Initializes the iterator with the `root` of the BST.
- `int next()` — Returns the next smallest number in the BST.
- `boolean hasNext()` — Returns `true` if there exists a number in the traversal to the right of the current pointer, otherwise returns `false`.

Both `next()` and `hasNext()` should run in **average O(1)** time and use **O(h)** space, where `h` is the height of the tree.

---

## Examples

### Example 1

```
        7
       / \
      3   15
         /  \
        9   20

BSTIterator iterator = new BSTIterator(root);
iterator.next();     // return 3
iterator.next();     // return 7
iterator.hasNext();  // return true
iterator.next();     // return 9
iterator.hasNext();  // return true
iterator.next();     // return 15
iterator.hasNext();  // return true
iterator.next();     // return 20
iterator.hasNext();  // return false
```

---

## Constraints

- The number of nodes in the tree is in the range `[1, 10^5]`.
- `0 <= Node.val <= 10^6`
- At most `10^5` calls will be made to `next` and `hasNext`.
- `next()` is always called when `hasNext()` returns `true`.

---

## Intuition

In-order traversal of a BST visits nodes in **ascending order** (left → root → right). The naive approach is to fully traverse the BST upfront, store all values in a list, and iterate over it. But that uses **O(n)** space.

The challenge: can we simulate in-order traversal **lazily** — computing only the next element on demand — using only **O(h)** space?

### Key Insight

In iterative in-order traversal, a **stack** is used to simulate the call stack of recursion. At any point, the stack holds the path from the current node back to the root — at most `h` nodes. We can maintain this stack across `next()` calls instead of running the full traversal upfront.

The trick is: **always push the leftmost path of the current node onto the stack.**

---

## Approach 1 — Controlled Iterative In-Order (O(h) Space)

### Core Idea

Maintain a stack. At initialization (and after each `next()` call), push all left children of the current node onto the stack. The top of the stack is always the next smallest element.

```
pushLeft(node):
  while node != null:
    stack.push(node)
    node = node.left
```

- `hasNext()` → stack is not empty
- `next()` → pop top, call `pushLeft(top.right)`, return top's value

### Why this works

When we pop a node `n`:

- Its left subtree is already fully exhausted (we pushed left spine first)
- Its right subtree needs to be explored next — so we push the left spine of `n.right`

This exactly mirrors recursive in-order traversal.

---

## Java Solution — Approach 1 (O(h) Space) ✅

```java
class BSTIterator {
    private Deque<TreeNode> stack;

    public BSTIterator(TreeNode root) {
        stack = new ArrayDeque<>();
        pushLeft(root); // push the leftmost path
    }

    public int next() {
        TreeNode node = stack.pop();    // next smallest
        pushLeft(node.right);           // explore right subtree
        return node.val;
    }

    public boolean hasNext() {
        return !stack.isEmpty();
    }

    // Push all left children of node onto the stack
    private void pushLeft(TreeNode node) {
        while (node != null) {
            stack.push(node);
            node = node.left;
        }
    }
}
```

---

## Visual Trace

```
        7
       / \
      3   15
         /  \
        9   20
```

**Initialization — `pushLeft(7)`:**

```
Push 7 → Push 3 (left of 7) → 3 has no left
Stack: [7, 3]  (3 on top)
```

**`next()` call 1:**

```
Pop 3 → pushLeft(3.right = null) → nothing pushed
Stack: [7]
Return: 3
```

**`next()` call 2:**

```
Pop 7 → pushLeft(7.right = 15)
  Push 15 → Push 9 (left of 15) → 9 has no left
Stack: [15, 9]  (9 on top)
Return: 7
```

**`next()` call 3:**

```
Pop 9 → pushLeft(9.right = null) → nothing pushed
Stack: [15]
Return: 9
```

**`next()` call 4:**

```
Pop 15 → pushLeft(15.right = 20)
  Push 20 → 20 has no left
Stack: [20]
Return: 15
```

**`next()` call 5:**

```
Pop 20 → pushLeft(20.right = null) → nothing pushed
Stack: []
Return: 20
```

**`hasNext()`:** `stack.isEmpty()` → `false` ✅

---

## Approach 2 — Flatten to List (O(n) Space)

Precompute the entire in-order traversal and store in a list. Simple but uses O(n) space — **does not meet the problem's space constraint**.

```java
class BSTIterator {
    private List<Integer> inorder;
    private int index;

    public BSTIterator(TreeNode root) {
        inorder = new ArrayList<>();
        index = 0;
        inorderTraversal(root);
    }

    private void inorderTraversal(TreeNode node) {
        if (node == null) return;
        inorderTraversal(node.left);
        inorder.add(node.val);
        inorderTraversal(node.right);
    }

    public int next() {
        return inorder.get(index++);
    }

    public boolean hasNext() {
        return index < inorder.size();
    }
}
```

||Time|Space|
|---|---|---|
|Constructor|O(n)|O(n)|
|`next()`|O(1)|—|
|`hasNext()`|O(1)|—|

Useful when simplicity matters and memory is not a constraint.

---

## Complexity Analysis — Approach 1

|Operation|Time|Space|
|---|---|---|
|Constructor|O(h)|O(h)|
|`next()`|**Amortized O(1)**|—|
|`hasNext()`|O(1)|—|
|Overall Space|—|O(h)|

### Why `next()` is Amortized O(1)?

Each node is pushed onto the stack **exactly once** and popped **exactly once** across all `next()` calls. Total operations across all `n` calls = `2n` → amortized O(1) per call.

A single `next()` call could push up to `h` nodes (when the popped node has a long right-left spine), but averaged over all calls the cost is O(1).

---

## Comparison

||Approach 1 (Stack)|Approach 2 (List)|
|---|---|---|
|Constructor|O(h)|O(n)|
|`next()`|Amortized O(1)|O(1)|
|`hasNext()`|O(1)|O(1)|
|Space|**O(h)** ✅|O(n)|
|Meets constraint?|✅ Yes|❌ No|

---

## Connecting to Iterative In-Order Traversal

The standard iterative in-order traversal looks like this:

```java
void inorder(TreeNode root) {
    Deque<TreeNode> stack = new ArrayDeque<>();
    TreeNode curr = root;

    while (curr != null || !stack.isEmpty()) {
        while (curr != null) {
            stack.push(curr);
            curr = curr.left;
        }
        curr = stack.pop();
        visit(curr);         // ← this is one "next()" call
        curr = curr.right;
    }
}
```

`BSTIterator` is exactly this loop **paused at each `visit()` call** and resumed on the next `next()` invocation. The stack preserves the state between calls.

---

## Extension — Two Iterators for Sorted Two-Sum in BST

A beautiful application: use **two iterators** (one forward, one backward) to find if any two nodes in the BST sum to a target — in O(n) time and O(h) space.

```java
class BSTIteratorReverse {
    private Deque<TreeNode> stack;

    public BSTIteratorReverse(TreeNode root) {
        stack = new ArrayDeque<>();
        pushRight(root); // push rightmost path (for descending order)
    }

    public int prev() {
        TreeNode node = stack.pop();
        pushRight(node.left);
        return node.val;
    }

    public boolean hasPrev() {
        return !stack.isEmpty();
    }

    private void pushRight(TreeNode node) {
        while (node != null) {
            stack.push(node);
            node = node.right;
        }
    }
}

// Two-pointer approach on BST — LC 653
boolean findTarget(TreeNode root, int k) {
    BSTIterator left = new BSTIterator(root);          // ascending
    BSTIteratorReverse right = new BSTIteratorReverse(root); // descending

    int lo = left.next(), hi = right.prev();

    while (lo < hi) {
        if (lo + hi == k) return true;
        else if (lo + hi < k) lo = left.next();
        else hi = right.prev();
    }

    return false;
}
```

---

## Common Mistakes to Avoid

1. **Pushing right child instead of right subtree's left spine** — After popping a node, call `pushLeft(node.right)`, not `stack.push(node.right)`. You need the entire left spine of the right subtree, not just the right child alone.
    
2. **Not initializing with `pushLeft(root)`** — The constructor must prime the stack with the leftmost path from root. Without this, the first `next()` call returns the root instead of the minimum.
    
3. **Confusing O(h) with O(1)** — The space is O(h), not O(1). For a balanced BST `h = O(log n)`; for a skewed BST `h = O(n)`.
    
4. **Calling `next()` without checking `hasNext()`** — The problem guarantees `next()` is only called when `hasNext()` is true, but in real code always guard with `hasNext()` first.
    

---

## Related Problems

|#|Problem|Relationship|
|---|---|---|
|LC 173|**BST Iterator** ← You are here|Lazy in-order traversal|
|LC 653|Two Sum IV — Input is a BST|Two BST iterators (forward + backward)|
|LC 230|Kth Smallest Element in a BST|Same stack-based in-order approach|
|LC 94|Binary Tree In-Order Traversal|Iterative version this iterator is built on|
|LC 285|Inorder Successor in BST|Next element in in-order traversal|