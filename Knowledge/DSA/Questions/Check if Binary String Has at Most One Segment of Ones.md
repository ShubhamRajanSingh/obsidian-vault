# Check if Binary String Has at Most One Segment of Ones

```java
class Solution {
    public boolean checkOnesSegment(String s){
        return !s.contains("01");
    }
}
```