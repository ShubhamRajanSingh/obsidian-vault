---
tags:
  - dsa
  - heap
  - stacks
  - design
  - leetcode
leetcode_id: 1172
complexity:
  time: O(log n)
  space: O(n)
status: solved
language: java
---

# Dinner Plate Stacks

## Problem Link
- https://leetcode.com/problems/dinner-plate-stacks/

## Intuition
We need:
- leftmost available stack for push
- rightmost non-empty stack for pop

Heaps efficiently maintain both conditions.

## Approach
1. Maintain list of stacks.
2. Min-heap stores available stack indices.
3. Track rightmost non-empty stack.
4. Handle push/pop/popAtStack operations.

## Java Solution
```java
class DinnerPlates {

    int capacity;
    List<Stack<Integer>> stacks;
    PriorityQueue<Integer> available;

    public DinnerPlates(int capacity) {
        this.capacity = capacity;
        stacks = new ArrayList<>();
        available = new PriorityQueue<>();
    }

    public void push(int val) {
        while (!available.isEmpty() &&
               available.peek() < stacks.size() &&
               stacks.get(available.peek()).size() == capacity) {

            available.poll();
        }

        if (available.isEmpty()) {
            stacks.add(new Stack<>());
            available.offer(stacks.size() - 1);
        }

        int idx = available.peek();
        stacks.get(idx).push(val);

        if (stacks.get(idx).size() == capacity) {
            available.poll();
        }
    }

    public int pop() {
        while (!stacks.isEmpty() &&
               stacks.get(stacks.size() - 1).isEmpty()) {

            stacks.remove(stacks.size() - 1);
        }

        if (stacks.isEmpty()) {
            return -1;
        }

        return popAtStack(stacks.size() - 1);
    }

    public int popAtStack(int index) {
        if (index >= stacks.size() ||
            stacks.get(index).isEmpty()) {

            return -1;
        }

        available.offer(index);

        return stacks.get(index).pop();
    }
}
```

## Complexity Analysis
- Time Complexity: O(log n)
- Space Complexity: O(n)

## Key Takeaways
- Heap + stack hybrid design.
- Indexed resource management.
