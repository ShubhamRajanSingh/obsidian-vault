# Competitive Coding

- Sources
    
    [Leetcode](https://leetcode.com/problemset/)
    
    [CodeForces](https://codeforces.com/)
    
- Notes
    - Catalan number
        
        ```java
        public int nthcatalan(int n) {
                int[] dp = new int[n + 1];
                dp[0] = 1;
                for (int i = 1; i <= n; i++) {
                    int count = 0;
        
                    for (int j = 1; j <= i; j++) {
                        count += dp[j - 1] * dp[i - j];
                    }
        
                    dp[i] = count;
                }
                return dp[n];
            }
        ```
        
        > i
        dp[i] = ∑     dp[j−1]×dp[i−j]
                    j=1
        > 
    - Topological Sort
        
        use the concept of indegree. maintain an array of indegree . then start traversing from node with indegree as zero. if cycle exists then the graph is not in topo sort
        
    - Subset Sum 1D array
        
        ```java
        class Solution {
            public boolean subsetSum(int[] nums, int target) {
                boolean[] dp = new boolean[target + 1];
                dp[0] = true; // Base case: sum 0 is always possible
                
                for (int num : nums) {
                    for (int j = target; j >= num; j--) {
                        dp[j] = dp[j] || dp[j - num];
                    }
                }
                
                return dp[target];
            }
        }
        
        ```
        
    - Find Combinatorics
        
        [https://chatgpt.com/c/69909ec9-1958-8324-a3ca-f73332c39f33](https://chatgpt.com/c/69909ec9-1958-8324-a3ca-f73332c39f33)
        
        ![Screenshot (3).png](Screenshot_(3).png)
        
        ```java
        public static int uniquePaths(int m, int n) {
            int N = m + n - 2;
            int R = Math.min(m - 1, n - 1); // choose smaller for efficiency
        
            long result = 1;
        
            for (int i = 1; i <= R; i++) {
                result = result * (N - R + i) / i;
            }
        
            return (int) result;
        }
        
        ```
        
- Questions
    - FLIPPING
        
        [https://leetcode.com/problems/minimum-number-of-k-consecutive-bit-flips/editorial/](https://leetcode.com/problems/minimum-number-of-k-consecutive-bit-flips/editorial/)
        
    - 378 Kth Smallest element in a sorted matrix
    - Maximum Product subarray
        
        ```java
        with index
        import java.util.*;
        
        public class Solution {
            public int[] maxProductSubarray(int[] nums) {
                if (nums == null || nums.length == 0) {
                    return new int[0];  // Return an empty array if input is invalid
                }
        
                int maxProduct = nums[0];
                int minSoFar = nums[0];
                int maxSoFar = nums[0];
        
                // To track the actual subarray
                int start = 0, end = 0, tempStart = 0;
        
                for (int i = 1; i < nums.length; i++) {
                    int current = nums[i];
        
                    // If current number is greater than maxSoFar * current, we update tempStart
                    if (current > maxSoFar * current && current > minSoFar * current) {
                        tempStart = i;  // Start a new subarray
                    }
        
                    // Calculate new possible max and min products
                    int tempMax = Math.max(current, Math.max(maxSoFar * current, minSoFar * current));
                    minSoFar = Math.min(current, Math.min(maxSoFar * current, minSoFar * current));
                    maxSoFar = tempMax;
        
                    // If maxSoFar is updated, we update the actual start and end
                    if (maxSoFar > maxProduct) {
                        maxProduct = maxSoFar;
                        start = tempStart;  // Update start to tempStart
                        end = i;             // End is the current index
                    }
                }
        
                // Extract the subarray that gives the maximum product
                int[] resultSubarray = Arrays.copyOfRange(nums, start, end + 1);
        
                // Return the subarray and the maximum product
                return resultSubarray;
            }
        
            public static void main(String[] args) {
                Solution solution = new Solution();
                int[] nums = {2, 3, -2, 4};
                int[] result = solution.maxProductSubarray(nums);
                System.out.println("Max Product Subarray: " + Arrays.toString(result));  // Output: [2, 3]
            }
        }
        
        ```
        
        ```
            public class Solution {
            public int maxProduct(int[] nums) {
                if (nums == null || nums.length == 0) {
                    return 0;
                }
        
                // Initialize the result and the min/max products so far
                int result = nums[0];
                int maxSoFar = nums[0];
                int minSoFar = nums[0];
        
                // Traverse through the array starting from the second element
                for (int i = 1; i < nums.length; i++) {
                    int current = nums[i];
        
                    // Temporary maxSoFar because we need both minSoFar and maxSoFar for calculation
                    int tempMax = Math.max(current, Math.max(maxSoFar * current, minSoFar * current));
        
                    // Update minSoFar
                    minSoFar = Math.min(current, Math.min(maxSoFar * current, minSoFar * current));
        
                    // Update maxSoFar using the temporary max
                    maxSoFar = tempMax;
        
                    // Update the result with the maximum product found so far
                    result = Math.max(result, maxSoFar);
                }
        
                return result;
            }
        }
        // Initialize the result and the min/max products so far
            int result = nums[0];
            int maxSoFar = nums[0];
            int minSoFar = nums[0];
        
            // Traverse through the array starting from the second element
            for (int i = 1; i < nums.length; i++) {
                int current = nums[i];
        
                // Temporary maxSoFar because we need both minSoFar and maxSoFar for calculation
                int tempMax = Math.max(current, Math.max(maxSoFar * current, minSoFar * current));
        
                // Update minSoFar
                minSoFar = Math.min(current, Math.min(maxSoFar * current, minSoFar * current));
        
                // Update maxSoFar using the temporary max
                maxSoFar = tempMax;
        
                // Update the result with the maximum product found so far
                result = Math.max(result, maxSoFar);
            }
        
            return result;
        }
        
        ```
        
        }
        
    - [**Longest consecutive subsequence**](https://leetcode.com/problems/longest-increasing-subsequence)
        
        > APPROACH 1 :
        DP :
        Steps: Initialize dp array to 1, as each element is at least a subsequence of length 1.
        For each element 𝑛 𝑢 𝑚 𝑠 [ 𝑖 ] nums[i], iterate over all previous elements 𝑛 𝑢 𝑚 𝑠 [ 𝑗 ] nums[j] (where 𝑗 < 𝑖 j<i):
        If 𝑛 𝑢 𝑚 𝑠 [ 𝑗 ] < 𝑛 𝑢 𝑚 𝑠 [ 𝑖 ] nums[j]<nums[i], update 𝑑 𝑝 [ 𝑖 ] = max ⁡ ( 𝑑 𝑝 [ 𝑖 ] , 𝑑 𝑝 [ 𝑗 ] + 1 ) dp[i]=max(dp[i],dp[j]+1).
        Return the maximum value in dp.
        > 
        
        > 
        > 
        > 
        > ### Approach 2: Binary Search (Optimized with Patience Sorting)
        > 
        > ### Key Idea:
        > 
        > Use a **greedy approach with binary search** to construct the LIS efficiently:
        > 
        > 1. Maintain an array `sub` where:
        >     - `sub[i]` is the smallest ending value of an increasing subsequence of length i+1.
        >         
        >         i+1i+1
        >         
        > 2. Iterate over the array `nums`:
        >     - Use binary search to find the position of `nums[i]` in `sub`.
        >     - If nums[i] is larger than all elements, append it to `sub`.
        >         
        >         nums[i]nums[i]
        >         
        >     - Otherwise, replace the first element in `sub` greater than or equal to nums[i].
        >         
        >         nums[i]nums[i]
        >         
        > 3. The length of `sub` at the end is the length of the LIS.
        > 
        > - Initialize `sub` as an empty list.
        > - Process each number:
        >     - `10`: Add to `sub`: `[10]`
        >     - `9`: Replace `10`: `[9]`
        >     - `2`: Replace `9`: `[2]`
        >     - `5`: Add to `sub`: `[2, 5]`
        >     - `3`: Replace `5`: `[2, 3]`
        >     - `7`: Add to `sub`: `[2, 3, 7]`
        >     - `101`: Add to `sub`: `[2, 3, 7, 101]`
        >     - `18`: Replace `101`: `[2, 3, 7, 18]`
        > - Final `sub` length is 444.
        > 
        > ```
        >    public int lengthOfLIS(int[] nums) {
        > ArrayList<Integer> sub = new ArrayList<>();
        >  for (int num : nums) {
        >         // Use binary search to find the position
        >         int pos = binarySearch(sub, num);
        > 
        >         if (pos == sub.size()) {
        >             sub.add(num); // Append to the sequence
        >         } else {
        >             sub.set(pos, num); // Replace the element
        >         }
        >     }
        > 
        >     return sub.size(); // The length of `sub` is the LIS length
        > }
        > 
        > private int binarySearch(ArrayList<Integer> sub, int target) {
        >     int left = 0, right = sub.size() - 1;
        >     while (left <= right) {
        >         int mid = left + (right - left) / 2;
        >         if (sub.get(mid) < target) {
        >             left = mid + 1;
        >         } else {
        >             right = mid - 1;
        >         }
        >     }
        >     return left;
        > }
        > 
        > ```
        > 
        
        Approach 2
        
    - city skyline
        
        ```java
        import java.util.*;
        
        public class SkylineProblem {
            public List<List<Integer>> getSkyline(int[][] buildings) {
                List<int[]> events = new ArrayList<>();
                // Step 1: Create events
                for (int[] building : buildings) {
                    events.add(new int[]{building[0], -building[2], building[1]}); // Start event
                    events.add(new int[]{building[1], 0, 0});                     // End event
                }
        
                // Step 2: Sort events
                Collections.sort(events, (a, b) -> {
                    if (a[0] != b[0]) return Integer.compare(a[0], b[0]); // Sort by x-coordinate
                    return Integer.compare(a[1], b[1]);                 // Sort by height
                });
        
                // Step 3: Process events with a max-heap
                PriorityQueue<int[]> maxHeap = new PriorityQueue<>((a, b) -> Integer.compare(b[0], a[0]));
                maxHeap.offer(new int[]{0, Integer.MAX_VALUE}); // Dummy building
                int prevHeight = 0;
        
                List<List<Integer>> result = new ArrayList<>();
                for (int[] event : events) {
                    int x = event[0], height = event[1], right = event[2];
        
                    if (height < 0) { // Start event
                        maxHeap.offer(new int[]{-height, right});
                    } else { // End event
                        maxHeap.removeIf(b -> b[1] == x);
                    }
        
                    // Get the current max height
                    int currentHeight = maxHeap.peek()[0];
                    if (currentHeight != prevHeight) {
                        result.add(Arrays.asList(x, currentHeight));
                        prevHeight = currentHeight;
                    }
                }
        
                return result;
            }
        
            public static void main(String[] args) {
                SkylineProblem solution = new SkylineProblem();
                int[][] buildings = {{2, 9, 10}, {3, 7, 15}, {5, 12, 12}, {15, 20, 10}, {19, 24, 8}};
                System.out.println(solution.getSkyline(buildings));
                // Output: [[2, 10], [3, 15], [7, 12], [12, 0], [15, 10], [20, 8], [24, 0]]
            }
        }
        
        ```
        
        > 
        > 
        > 
        > ### Input
        > 
        > ```
        > text
        > Copy code
        > Buildings: [[2, 9, 10], [3, 7, 15], [5, 12, 12], [15, 20, 10], [19, 24, 8]]
        > 
        > ```
        > 
        > ### Step-by-Step Execution
        > 
        > ---
        > 
        > ### **Step 1: Create Events**
        > 
        > For each building `[L, R, H]`, create two events:
        > 
        > 1. **Start event**: `(L, -H, R)` — represents the start of a building.
        > 2. **End event**: `(R, 0, 0)` — represents the end of a building.
        > 
        > After processing all buildings:
        > 
        > ```
        > text
        > Copy code
        > Events (before sorting):
        > [(2, -10, 9), (9, 0, 0), (3, -15, 7), (7, 0, 0), (5, -12, 12),
        >  (12, 0, 0), (15, -10, 20), (20, 0, 0), (19, -8, 24), (24, 0, 0)]
        > 
        > ```
        > 
        > ---
        > 
        > ### **Step 2: Sort Events**
        > 
        > Sort events by:
        > 
        > 1. **x-coordinate** (ascending).
        > 2. Height:
        >     - Start events (`H`) before end events (`0`).
        >     - For the same x-coordinate, taller buildings come first (higher negative heights).
        > 
        > After sorting:
        > 
        > ```
        > text
        > Copy code
        > Events (sorted):
        > [(2, -10, 9), (3, -15, 7), (5, -12, 12), (7, 0, 0), (9, 0, 0),
        >  (12, 0, 0), (15, -10, 20), (19, -8, 24), (20, 0, 0), (24, 0, 0)]
        > 
        > ```
        > 
        > ---
        > 
        > ### **Step 3: Process Events**
        > 
        > We use a **max-heap** to keep track of active building heights and their right edges.
        > 
        > ### **Initial State**:
        > 
        > - Heap: `[(0, ∞)]` — contains a dummy building with height 0 and infinite right edge.
        > - Result: `[]` — the list of key points.
        > - `prevHeight`: `0` — the height of the last processed skyline.
        > 
        > ### **Iterating Through Events**:
        > 
        > 1. **Event `(2, -10, 9)`**:
        >     - Start of a building. Add (−10,9) to the heap.
        >         
        >         (−10,9)(-10, 9)
        >         
        >     - Heap: `[(10, 9), (0, ∞)]`.
        >     - Current max height: `10`. It changes from `0` to `10`.
        >     - Add key point: `[2, 10]`.
        >     - Result: `[[2, 10]]`.
        > 2. **Event `(3, -15, 7)`**:
        >     - Start of a building. Add (−15,7) to the heap.
        >         
        >         (−15,7)(-15, 7)
        >         
        >     - Heap: `[(15, 7), (0, ∞), (10, 9)]`.
        >     - Current max height: `15`. It changes from `10` to `15`.
        >     - Add key point: `[3, 15]`.
        >     - Result: `[[2, 10], [3, 15]]`.
        > 3. **Event `(5, -12, 12)`**:
        >     - Start of a building. Add (−12,12) to the heap.
        >         
        >         (−12,12)(-12, 12)
        >         
        >     - Heap: `[(15, 7), (12, 12), (10, 9), (0, ∞)]`.
        >     - Current max height: `15`. No change.
        >     - Result: `[[2, 10], [3, 15]]`.
        > 4. **Event `(7, 0, 0)`**:
        >     - End of the building with height `15` (right edge = 7).
        >     - Remove (15,7) from the heap.
        >         
        >         (15,7)(15, 7)
        >         
        >     - Heap: `[(12, 12), (0, ∞), (10, 9)]`.
        >     - Current max height: `12`. It changes from `15` to `12`.
        >     - Add key point: `[7, 12]`.
        >     - Result: `[[2, 10], [3, 15], [7, 12]]`.
        > 5. **Event `(9, 0, 0)`**:
        >     - End of the building with height `10` (right edge = 9).
        >     - Remove (10,9) from the heap.
        >         
        >         (10,9)(10, 9)
        >         
        >     - Heap: `[(12, 12), (0, ∞)]`.
        >     - Current max height: `12`. No change.
        >     - Result: `[[2, 10], [3, 15], [7, 12]]`.
        > 6. **Event `(12, 0, 0)`**:
        >     - End of the building with height `12` (right edge = 12).
        >     - Remove (12,12) from the heap.
        >         
        >         (12,12)(12, 12)
        >         
        >     - Heap: `[(0, ∞)]`.
        >     - Current max height: `0`. It changes from `12` to `0`.
        >     - Add key point: `[12, 0]`.
        >     - Result: `[[2, 10], [3, 15], [7, 12], [12, 0]]`.
        > 7. **Event `(15, -10, 20)`**:
        >     - Start of a building. Add (−10,20) to the heap.
        >         
        >         (−10,20)(-10, 20)
        >         
        >     - Heap: `[(10, 20), (0, ∞)]`.
        >     - Current max height: `10`. It changes from `0` to `10`.
        >     - Add key point: `[15, 10]`.
        >     - Result: `[[2, 10], [3, 15], [7, 12], [12, 0], [15, 10]]`.
        > 8. **Event `(19, -8, 24)`**:
        >     - Start of a building. Add (−8,24) to the heap.
        >         
        >         (−8,24)(-8, 24)
        >         
        >     - Heap: `[(10, 20), (0, ∞), (8, 24)]`.
        >     - Current max height: `10`. No change.
        >     - Result: `[[2, 10], [3, 15], [7, 12], [12, 0], [15, 10]]`.
        > 9. **Event `(20, 0, 0)`**:
        >     - End of the building with height `10` (right edge = 20).
        >     - Remove (10,20) from the heap.
        >         
        >         (10,20)(10, 20)
        >         
        >     - Heap: `[(8, 24), (0, ∞)]`.
        >     - Current max height: `8`. It changes from `10` to `8`.
        >     - Add key point: `[20, 8]`.
        >     - Result: `[[2, 10], [3, 15], [7, 12], [12, 0], [15, 10], [20, 8]]`.
        > 10. **Event `(24, 0, 0)`**:
        >     - End of the building with height `8` (right edge = 24).
        >     - Remove (8,24) from the heap.
        >         
        >         (8,24)(8, 24)
        >         
        >     - Heap: `[(0, ∞)]`.
        >     - Current max height: `0`. It changes from `8` to `0`.
        >     - Add key point: `[24, 0]`.
        >     - Result: `[[2, 10], [3, 15], [7, 12], [12, 0], [15, 10], [20, 8], [24, 0]]`.
        
        ### Approach: Sweep Line with Priority Queue
        
        This approach combines sorting and a max-heap to efficiently find key points.
        
        ### Steps:
        
        1. **Create Events**:
            - For each building [L,R,H]:
                
                [L,R,H][L, R, H]
                
                - Add a **start event**: (L,−H,R), indicating the start of the building.
                    
                    (L,−H,R)(L, -H, R)
                    
                - Add an **end event**: (R,0,0), indicating the end of the building.
                    
                    (R,0,0)(R, 0, 0)
                    
            - Sort the events by:
                - xxx-coordinate (ascending).
                - Height (−H for start events, 0 for end events).
                    
                    −H-H
                    
                    00
                    
        2. **Process Events**:
            - Use a max-heap to keep track of active building heights. The heap stores (H,R), where H is the height and R is the right edge.
                
                (H,R)(H, R)
                
                HH
                
                RR
                
            - For each event:
                - If it's a start event, add it to the heap.
                - If it's an end event, remove its corresponding height from the heap.
            - After each event, check if the maximum height has changed:
                - If yes, add a key point (x,maxHeight).
                    
                    (x,maxHeight)(x, maxHeight)
                    
    - Longest increasing Subsequence
    - [https://takeuforward.org/data-structure/count-inversions-in-an-array/](https://takeuforward.org/data-structure/count-inversions-in-an-array/)
    
    [https://takeuforward.org/data-structure/count-the-number-of-subarrays-with-given-xor-k/](https://takeuforward.org/data-structure/count-the-number-of-subarrays-with-given-xor-k/)
    
    [https://takeuforward.org/data-structure/find-intersection-of-two-linked-lists/](https://takeuforward.org/data-structure/find-intersection-of-two-linked-lists/) (refer to the most optimal approach)
    
    - find the kth permutation. striver
        
        ```java
        import java.util.*;
        
        public class Main
        {
            static String getPermutation(int n, int k) {
                int fact = 1;
                ArrayList < Integer > numbers = new ArrayList < > ();
                for (int i = 1; i < n; i++) {
                    fact = fact * i;
                    numbers.add(i);
                }
                numbers.add(n);
                String ans = "";
                k = k - 1;
                while (true) {
                    ans = ans + "" + numbers.get(k / fact);
                    numbers.remove(k / fact);
                    if (numbers.size() == 0) {
                        break;
                    }
        
                    k = k % fact;
                    fact = fact / numbers.size();
                }
                return ans;
            }
        
            public static void main(String args[]) {
                int n = 4, k = 16;
                String ans = getPermutation(n, k);
                System.out.println("The Kth permutation sequence is " + ans);
        
            }
        }
        ```
        
    - [https://takeuforward.org/data-structure/area-of-largest-rectangle-in-histogram/](https://takeuforward.org/data-structure/area-of-largest-rectangle-in-histogram/)
    - rabin-karp (especially the hash function)
    - [https://takeuforward.org/data-structure/sliding-window-maximum/](https://takeuforward.org/data-structure/sliding-window-maximum/)
    - next permutation
        - iterate from left and find where exactly we go from high to low
        - mark the low point and iterate from next element to right until u find a greater element
        - swap the two numbers
        - reverse the list

knuth morris pratt

LIS using binary search