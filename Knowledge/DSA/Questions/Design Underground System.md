# Design Underground System

```java
class UndergroundSystem {
    Map<Integer,Pair> in=new HashMap<>();
    Map<String,long[]> stats=new HashMap<>();

    public void checkIn(int id,String station,int t){
        in.put(id,new Pair(station,t));
    }

    public void checkOut(int id,String station,int t){
        Pair p=in.remove(id);
        String key=p.station+"->"+station;

        stats.putIfAbsent(key,new long[2]);
        stats.get(key)[0]+=t-p.time;
        stats.get(key)[1]++;
    }

    public double getAverageTime(String s,String e){
        long[] x=stats.get(s+"->"+e);
        return (double)x[0]/x[1];
    }

    class Pair{
        String station;
        int time;

        Pair(String s,int t){
            station=s;
            time=t;
        }
    }
}
```