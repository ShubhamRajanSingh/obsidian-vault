# Accounts Merge

## Tags
- dsa
- union-find
- graphs
- leetcode

```java
class Solution {
    public List<List<String>> accountsMerge(List<List<String>> accounts) {
        Map<String, String> parent = new HashMap<>();
        Map<String, String> owner = new HashMap<>();

        for (List<String> acc : accounts) {
            String name = acc.get(0);
            for (int i = 1; i < acc.size(); i++) {
                parent.putIfAbsent(acc.get(i), acc.get(i));
                owner.put(acc.get(i), name);
                union(parent, acc.get(1), acc.get(i));
            }
        }

        Map<String, TreeSet<String>> groups = new HashMap<>();

        for (String email : parent.keySet()) {
            String root = find(parent, email);
            groups.computeIfAbsent(root, k -> new TreeSet<>()).add(email);
        }

        List<List<String>> ans = new ArrayList<>();

        for (String root : groups.keySet()) {
            List<String> list = new ArrayList<>();
            list.add(owner.get(root));
            list.addAll(groups.get(root));
            ans.add(list);
        }

        return ans;
    }

    private String find(Map<String, String> parent, String s) {
        if (!parent.get(s).equals(s)) {
            parent.put(s, find(parent, parent.get(s)));
        }
        return parent.get(s);
    }

    private void union(Map<String, String> parent, String a, String b) {
        parent.put(find(parent, a), find(parent, b));
    }
}
```
