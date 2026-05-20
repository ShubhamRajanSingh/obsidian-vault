# Rank Teams by Votes

```java
class Solution {
    public String rankTeams(String[] votes){
        int n=votes[0].length();

        Map<Character,int[]> map=new HashMap<>();

        for(char ch:votes[0].toCharArray())
            map.put(ch,new int[n]);

        for(String v:votes){
            for(int i=0;i<n;i++)
                map.get(v.charAt(i))[i]++;
        }

        Character[] teams=new Character[n];

        for(int i=0;i<n;i++) teams[i]=votes[0].charAt(i);

        Arrays.sort(teams,(a,b)->{
            for(int i=0;i<n;i++){
                if(map.get(a)[i]!=map.get(b)[i])
                    return map.get(b)[i]-map.get(a)[i];
            }
            return a-b;
        });

        StringBuilder sb=new StringBuilder();

        for(char ch:teams) sb.append(ch);

        return sb.toString();
    }
}
```