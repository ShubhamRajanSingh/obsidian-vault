# Time Based Key Value Store

## Tags
- dsa
- hashmap
- binary-search
- design
- leetcode

```java
class TimeMap {

    Map<String, List<Pair>> map;

    public TimeMap() {
        map = new HashMap<>();
    }

    public void set(String key,
                    String value,
                    int timestamp) {

        map.computeIfAbsent(key,
            x -> new ArrayList<>())
            .add(new Pair(timestamp, value));
    }

    public String get(String key,
                      int timestamp) {

        if(!map.containsKey(key)) return "";

        List<Pair> list = map.get(key);

        int left = 0;
        int right = list.size() - 1;

        String ans = "";

        while(left <= right) {
            int mid = left + (right - left) / 2;

            if(list.get(mid).time <= timestamp) {
                ans = list.get(mid).value;
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }

        return ans;
    }

    class Pair {
        int time;
        String value;

        Pair(int time, String value) {
            this.time = time;
            this.value = value;
        }
    }
}
```
