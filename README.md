# Graph Valid Tree
## https://leetcode.com/problems/graph-valid-tree


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
