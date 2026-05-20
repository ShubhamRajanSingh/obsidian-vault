---
tags:
  - dsa
  - stack
  - parsing
  - recursion
  - strings
  - leetcode
leetcode_id: 1106
complexity:
  time: O(n)
  space: O(n)
status: solved
language: java
---

# Parsing A Boolean Expression

## Problem Link
- https://leetcode.com/problems/parsing-a-boolean-expression/

## Intuition
Expressions are naturally nested.
Stack evaluation handles nested operators efficiently.

## Approach
1. Traverse expression.
2. Push symbols except commas.
3. Evaluate when ')' appears.
4. Apply:
   - AND
   - OR
   - NOT

## Java Solution
```java
class Solution {
    public boolean parseBoolExpr(String expression) {
        Stack<Character> stack = new Stack<>();

        for (char ch : expression.toCharArray()) {
            if (ch == ',') {
                continue;
            }

            if (ch != ')') {
                stack.push(ch);
            } else {
                Set<Character> values = new HashSet<>();

                while (stack.peek() != '(') {
                    values.add(stack.pop());
                }

                stack.pop();

                char operator = stack.pop();

                if (operator == '&') {
                    stack.push(values.contains('f') ? 'f' : 't');
                } else if (operator == '|') {
                    stack.push(values.contains('t') ? 't' : 'f');
                } else {
                    stack.push(values.contains('t') ? 'f' : 't');
                }
            }
        }

        return stack.pop() == 't';
    }
}
```

## Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(n)

## Key Takeaways
- Expression evaluation using stack.
- Nested operator parsing.
