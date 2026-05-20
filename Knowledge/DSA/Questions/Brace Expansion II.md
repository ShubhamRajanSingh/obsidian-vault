---
tags:
  - dsa
  - recursion
  - parsing
  - stack
  - strings
  - leetcode
leetcode_id: 1096
complexity:
  time: Exponential
  space: Exponential
status: solved
language: java
---

# Brace Expansion II

## Problem Link
- https://leetcode.com/problems/brace-expansion-ii/

## Intuition
Expressions contain:
- union
- concatenation

Recursive parsing naturally evaluates nested expressions.

## Approach
1. Parse recursively.
2. Maintain current concatenation set.
3. Merge unions appropriately.
4. Return sorted result.

## Java Solution
```java
class Solution {

    int index = 0;

    public List<String> braceExpansionII(String expression) {
        Set<String> result = parse(expression);

        List<String> answer = new ArrayList<>(result);
        Collections.sort(answer);

        return answer;
    }

    private Set<String> parse(String s) {
        Set<String> result = new HashSet<>();
        Set<String> current = new HashSet<>();

        current.add("");

        while (index < s.length() && s.charAt(index) != '}') {
            char ch = s.charAt(index);

            if (ch == ',') {
                result.addAll(current);
                current = new HashSet<>();
                current.add("");
                index++;
            } else {
                Set<String> next = new HashSet<>();

                if (ch == '{') {
                    index++;
                    next = parse(s);
                    index++;
                } else {
                    next.add(String.valueOf(ch));
                    index++;
                }

                Set<String> combined = new HashSet<>();

                for (String a : current) {
                    for (String b : next) {
                        combined.add(a + b);
                    }
                }

                current = combined;
            }
        }

        result.addAll(current);

        return result;
    }
}
```

## Complexity Analysis
- Time Complexity: Exponential
- Space Complexity: Exponential

## Key Takeaways
- Recursive descent parsing.
- Set-based expression expansion.
