# N Meetings in One Room

## Tags
#DSA #Greedy #Scheduling #InterviewPrep

## Approach (Greedy - Finish Time)
- Sort by end time
- Select non-overlapping meetings

```java
import java.util.*;

class Meeting {
    int start, end;
    Meeting(int s, int e) {
        start = s;
        end = e;
    }
}

class Solution {
    public int maxMeetings(int start[], int end[]) {
        int n = start.length;
        Meeting[] meetings = new Meeting[n];

        for (int i = 0; i < n; i++)
            meetings[i] = new Meeting(start[i], end[i]);

        Arrays.sort(meetings, (a, b) -> a.end - b.end);

        int count = 1;
        int lastEnd = meetings[0].end;

        for (int i = 1; i < n; i++) {
            if (meetings[i].start > lastEnd) {
                count++;
                lastEnd = meetings[i].end;
            }
        }

        return count;
    }
}
```