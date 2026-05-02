# 155. Min Stack

**Difficulty:** Medium **Topics:** Stack, Design **Link:** https://leetcode.com/problems/min-stack/

---

## Problem Statement

Design a stack that supports push, pop, top, and retrieving the minimum element in **constant time**.

Implement the `MinStack` class:

- `MinStack()` — Initializes the stack object.
- `void push(int val)` — Pushes the element `val` onto the stack.
- `void pop()` — Removes the element on the top of the stack.
- `int top()` — Gets the top element of the stack.
- `int getMin()` — Retrieves the minimum element in the stack.

All operations must run in **O(1)** time.

---

## Examples

### Example 1

```
Input:
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

Output:
[null, null, null, null, -3, null, 0, -2]
```

**Explanation:**

```
MinStack minStack = new MinStack();
minStack.push(-2);   // stack: [-2],          min: -2
minStack.push(0);    // stack: [-2, 0],        min: -2
minStack.push(-3);   // stack: [-2, 0, -3],    min: -3
minStack.getMin();   // return -3
minStack.pop();      // stack: [-2, 0],        min: -2
minStack.top();      // return 0
minStack.getMin();   // return -2
```

---

## Constraints

- `-2^31 <= val <= 2^31 - 1`
- `pop`, `top`, and `getMin` will always be called on a **non-empty** stack.
- At most `3 * 10^4` calls will be made to `push`, `pop`, `top`, and `getMin`.

---

## Intuition

`push`, `pop`, and `top` are trivially O(1) with a regular stack. The challenge is `getMin` in O(1).

A naive approach would scan the entire stack to find the minimum — O(n). The trick is:

> Track the minimum **at every level** of the stack. When an element is pushed, also record what the minimum was **at that point in time**. When that element is popped, the previous minimum is automatically restored.

This is done using a **second stack** (`minStack`) that runs parallel to the main stack, storing the current minimum at each push.

---

## Core Idea: Auxiliary Min Stack

```
Operation       Main Stack       Min Stack
push(-2)        [-2]             [-2]         ← min is -2
push(0)         [-2, 0]          [-2, -2]     ← min is still -2
push(-3)        [-2, 0, -3]      [-2, -2, -3] ← new min is -3
getMin()        →  peek minStack = -3  ✅
pop()           [-2, 0]          [-2, -2]     ← -3 popped from both
top()           →  peek mainStack = 0  ✅
getMin()        →  peek minStack = -2  ✅
```

The min stack's top always answers "what is the minimum of everything currently in the main stack?"

---

## Java Solution

```java
class MinStack {
    private Deque<Integer> stack;    // main stack
    private Deque<Integer> minStack; // tracks current min at each level

    public MinStack() {
        stack = new ArrayDeque<>();
        minStack = new ArrayDeque<>();
    }

    public void push(int val) {
        stack.push(val);
        // Push the new minimum: either val itself or the previous minimum
        if (minStack.isEmpty()) {
            minStack.push(val);
        } else {
            minStack.push(Math.min(val, minStack.peek()));
        }
    }

    public void pop() {
        stack.pop();
        minStack.pop(); // always pop both together
    }

    public int top() {
        return stack.peek();
    }

    public int getMin() {
        return minStack.peek();
    }
}
```

---

## Dry Run

**Input:** `push(-2), push(0), push(-3), getMin, pop, top, getMin`

|Operation|val|stack (top→)|minStack (top→)|return|
|---|---|---|---|---|
|push(-2)|-2|[-2]|[-2]|—|
|push(0)|0|[0,-2]|[-2,-2]|—|
|push(-3)|-3|[-3,0,-2]|[-3,-2,-2]|—|
|getMin()|—|[-3,0,-2]|[-3,-2,-2]|**-3**|
|pop()|—|[0,-2]|[-2,-2]|—|
|top()|—|[0,-2]|[-2,-2]|**0**|
|getMin()|—|[0,-2]|[-2,-2]|**-2**|

---

## Why Both Stacks Must Be Popped Together

The min stack stores **one min value per element** pushed onto the main stack. They are always the same size. Popping one without the other would desync them — the min stack top would no longer correspond to the main stack's current top.

```
Main:  [-3, 0, -2]   ← -3 is current top
Min:   [-3, -2, -2]  ← -3 is current min

After pop() on both:
Main:  [0, -2]       ← 0 is current top
Min:   [-2, -2]      ← -2 is current min  ✅

If we only popped main:
Main:  [0, -2]       ← 0 is current top
Min:   [-3, -2, -2]  ← -3 is still at top ❌ (wrong min!)
```

---

## Alternative: Store (value, currentMin) Pairs — Single Stack

Instead of two stacks, store a pair `[value, minSoFar]` in a single stack:

```java
class MinStack {
    private Deque<int[]> stack; // each entry: [value, currentMin]

    public MinStack() {
        stack = new ArrayDeque<>();
    }

    public void push(int val) {
        int currentMin = stack.isEmpty() ? val : Math.min(val, stack.peek()[1]);
        stack.push(new int[]{val, currentMin});
    }

    public void pop() {
        stack.pop();
    }

    public int top() {
        return stack.peek()[0];
    }

    public int getMin() {
        return stack.peek()[1];
    }
}
```

Same O(1) time and O(n) space — just a matter of preference. The two-stack approach is more intuitive; the pair approach uses one data structure.

---

## Complexity Analysis

|Operation|Time|Space|
|---|---|---|
|`push`|O(1)|O(n)|
|`pop`|O(1)|—|
|`top`|O(1)|—|
|`getMin`|O(1)|—|

- **Time:** All operations are O(1) — just stack push/peek/pop.
- **Space:** O(n) — the auxiliary min stack grows proportionally with the number of elements pushed.

---

## Common Mistakes to Avoid

1. **Storing only a single `currentMin` variable** — This breaks on `pop`. If you pop the current minimum, you have no way to know what the previous minimum was.
    
    ```
    push(3) → min=3
    push(1) → min=1
    pop()   → what is min now? You've lost 3. ❌
    ```
    
2. **Not pushing to minStack when value equals current min** — This is only relevant if you use a space-optimized variant where you push to minStack only when a new minimum is found. In that case, you must also push on `==` (not just `<`), otherwise popping a duplicate minimum will desync the stacks.
    
3. **Forgetting to pop minStack in `pop()`** — Both stacks must always be popped together to stay in sync.
    

---

## Related Problems

| #   | Problem                      | Relationship                     |
| --- | ---------------------------- | -------------------------------- |
| 155 | **Min Stack** ← You are here | Track min with auxiliary stack   |
| 716 | Max Stack                    | Track max, also support popMax() |
| 232 | Implement Queue using Stacks | Stack-based design               |
| 295 | Find Median from Data Stream | Track median dynamically         |