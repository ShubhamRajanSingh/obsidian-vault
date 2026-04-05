# 🔄 Topological Sort

## 📌 Idea
- Works only for DAG (Directed Acyclic Graph)
- Uses **indegree concept**

---

## 🧠 Approach
1. Compute indegree of all nodes
2. Push nodes with indegree = 0 into queue
3. Process nodes:
   - Remove node
   - Reduce indegree of neighbors
4. If cycle exists → topo sort not possible

---

## 💻 Code (Kahn’s Algorithm)
```java
Queue<Integer> q = new LinkedList<>();

for (int i = 0; i < n; i++) {
    if (indegree[i] == 0) q.add(i);
}

while (!q.isEmpty()) {
    int node = q.poll();
    
    for (int neigh : adj.get(node)) {
        indegree[neigh]--;
        if (indegree[neigh] == 0) {
            q.add(neigh);
        }
    }
}
```

---

## ⚠️ Key Insight
- If processed nodes < total nodes → cycle exists
