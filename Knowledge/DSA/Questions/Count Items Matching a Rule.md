# Count Items Matching a Rule

```java
class Solution {
    public int countMatches(List<List<String>> items,
                            String ruleKey,
                            String ruleValue){

        int idx = ruleKey.equals("type") ? 0 :
                  ruleKey.equals("color") ? 1 : 2;

        int ans=0;

        for(List<String> item:items)
            if(item.get(idx).equals(ruleValue)) ans++;

        return ans;
    }
}
```