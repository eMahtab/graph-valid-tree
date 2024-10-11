# Graph Valid Tree
## https://leetcode.com/problems/graph-valid-tree

You have a graph of n nodes labeled from 0 to n - 1. You are given an integer n and a list of edges where edges[i] = [ai, bi] indicates that there is an undirected edge between nodes ai and bi in the graph.

Return true if the edges of the given graph make up a valid tree, and false otherwise.

![Graph Valid Tree](example.JPG?raw=true)

## Approach :
A graph will be a valid tree only if, it doesn't have cycles and it doesn't have disconnected components.

So if a graph doesn't have cycles and its connected (if we start from any vertex we can reach all other vertices), it will be a valid tree.

# Implementation 1 : DFS
```java
class Solution {
    public boolean validTree(int n, int[][] edges) {
        Map<Integer,Set<Integer>> graph = new HashMap<>();
        for(int[] edge : edges) {
            int u = edge[0];
            int v = edge[1];
            graph.putIfAbsent(u, new HashSet<Integer>());
            graph.get(u).add(v);
            graph.putIfAbsent(v, new HashSet<Integer>());
            graph.get(v).add(u);
        }
        Set<Integer> visited = new HashSet<>();
        boolean containsCycle = checkCycle(0, -1, graph, visited);
        return !containsCycle && visited.size() == n;
    }

    private boolean checkCycle(int vertex, int parent, Map<Integer,Set<Integer>> graph, Set<Integer> visited) {
        if(visited.contains(vertex))
            return true;
        visited.add(vertex);    
        Set<Integer> neighbors = graph.get(vertex);
        if(neighbors != null) {
            for(int neighbor : neighbors) {
                if(neighbor != parent) {
                    boolean hasCycle = checkCycle(neighbor, vertex, graph, visited);
                    if(hasCycle)
                     return true;
                }
            }
        }
        return false;
    }
}
```
# Implementation 2 : BFS
```java
class Solution {
    public boolean validTree(int n, int[][] edges) {
        Map<Integer,Set<Integer>> graph = new HashMap<>();
        for(int[] edge : edges) {
            int u = edge[0];
            int v = edge[1];
            graph.putIfAbsent(u, new HashSet<Integer>());
            graph.get(u).add(v);
            graph.putIfAbsent(v, new HashSet<Integer>());
            graph.get(v).add(u);
        }
        Set<Integer> visited = new HashSet<>();
        Queue<int[]> queue = new LinkedList<>();
        queue.add(new int[]{0, -1});
        while(!queue.isEmpty()) {
            int[] node = queue.remove();
            int vertex = node[0], parent = node[1];
            if(visited.contains(vertex))
               return false;
            visited.add(vertex);
            Set<Integer> neighbors = graph.get(vertex);
            if(neighbors != null) {
                for(int neighbor : neighbors) {
                    if(neighbor == parent)
                       continue;
                    queue.add(new int[]{neighbor, vertex});   
                }
            }    
        }
        return visited.size() == n;
    }
}
```

# References :
1. https://www.youtube.com/watch?v=bXsUuownnoQ
