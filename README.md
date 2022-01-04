# Graph Valid Tree
## https://leetcode.com/problems/graph-valid-tree

You have a graph of n nodes labeled from 0 to n - 1. You are given an integer n and a list of edges where edges[i] = [ai, bi] indicates that there is an undirected edge between nodes ai and bi in the graph.

Return true if the edges of the given graph make up a valid tree, and false otherwise.

![Graph Valid Tree](example.JPG?raw=true)

## Approach :
A graph will be a valid tree only if, it doesn't have cycles and it doesn't have disconnected components.

So if a graph doesn't have cycles and its connected (if we start from any vertex we can reach all other vertices), it will be a valid tree.

# Implementation 1 :
```java
class Solution {
    public boolean validTree(int n, int[][] edges) {
        if(n == 0)
            return true;
        Map<Integer,List<Integer>> graph = new HashMap<>();
        for(int[] edge : edges) {
            graph.putIfAbsent(edge[0], new ArrayList<Integer>());
            graph.get(edge[0]).add(edge[1]);
            graph.putIfAbsent(edge[1], new ArrayList<Integer>());
            graph.get(edge[1]).add(edge[0]);
        }
        
        int parentVertex = -1;
        Set<Integer> visited = new HashSet<>();
        return dfs(0,-1, graph, visited) && visited.size() == n;
        
    }
    
    private boolean dfs(int n, int parentVertex, Map<Integer,List<Integer>> graph, Set<Integer> visited) {
        if(visited.contains(n)) 
            return false;
        visited.add(n);
        // traverse neighbors
        List<Integer> neighbors = graph.get(n);
        if(neighbors != null) {
           for(int neighbor : neighbors) {
             if(neighbor != parentVertex) {
                boolean result = dfs(neighbor, n, graph, visited);
                if(!result)
                    return false;
             }
           }
        }
        
        return true;
    }
}
```

# References :
1. https://www.youtube.com/watch?v=bXsUuownnoQ
