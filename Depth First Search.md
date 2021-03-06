# Depth First Search
Depth First Search (DFS) is an algorithm used for traversing or searching tree or graph data structures. The algorithm starts at the root node of tree (or some arbitrary nodes of graph) and then explores as far as possible along each path before backtracing. 

## Pseudocode
- Recursive version:
```
recursive DFS(g, u): // g is the graph, u is the start node
  mark u as visited
  for all adjacent nodes v of u in the graph
    if v is not visited
      recursively call DFS(g, v)
```
- Iterative version:
```
iterative DFS(g, u): // g is the graph, u is the start node
  let S be a Stack 
  S.Push(u)
  while S is not empty
    v = Stack.Pop();
    if v is not marked as visited
      mark v as visited
      for all adjacent nodes w of v in the graph
        Stack.Push(w)
```

## DFS backtracing
![image](https://github.com/idanhuang/DataStructure-and-Algorithm/blob/master/image/DFS_backtrack.png) <br/>

DFS will explore along a path as far as possible. When there are no more vertex along the current path can be explored, DFS moves backwards on the same path to find the next unvisited vertex to traverse. All the vertices on the current path will be visited before the next path is selected.

Example 1: In the left graph, DFS will start from vertex s, then vertices 1, 2 and 3 (suppose DFS chooses 1 before 4). After visiting vertex 3, DFS hits a dead end and will back out to 2 then back to 1. Then DFS will select the next path to explore, DFS will visit vertices 4, 5 and 6 successively. When hitting vertex 6, DFS will back out to 5, 4 and s.

Example 2: In the right graph, DFS will start from vertex s, then vertices 1, 2, and 3 (suppose DFS chooses 3 before 5 when at vertex 2). When DFS hits a dead end at vertex 3, it back out to 2, then traverses vertex 5, then to 4. Then back out to 5 before travering vertex 6. Then DFS will back out to 5, then 2 then 1, then s.

## DFS Implementation
```C#
using System;
using System.Collections.Generic;
using System.Linq;

public class Graph
{
    public Dictionary<int, List<int>> graph = null;

    public Graph(List<int> vertices)
    {
        graph = new Dictionary<int, List<int>>();
        foreach (int vertex in vertices)
            graph[vertex] = new List<int>();
    }

    public void AddEdge(int source, int destination)
    {
        if (graph.ContainsKey(source))
            graph[source].Add(destination);
        else
            graph[source] = new List<int>() { destination };
    }
}

public class DFS
{
    public List<int> RecursiveDFS(Dictionary<int, List<int>> graph, int u, List<int> path, HashSet<int> visited)
    {
        path.Add(u);
        visited.Add(u);

        foreach(int v in graph[u])
        {
            if(!visited.Contains(v))
            {
                visited.Add(v);
                RecursiveDFS(graph, v, path, visited);
            }
        }

        return path;
    }

    public HashSet<int> IterativeDFS(Dictionary<int, List<int>> graph, int u, HashSet<int> visited)
    {
        Stack<int> stack = new Stack<int>();
        stack.Push(u);

        while(stack.Any())
        {
            int v = stack.Pop();
            if (!visited.Contains(v))
            {
                visited.Add(v);
                foreach (int w in graph[v])
                {
                    stack.Push(w);
                }
            }
        }

        return visited;
    }

    public static void Main(string[] args)
    {
        Graph g = new Graph(new List<int>() { 0, 1, 2, 3, 4, 5, 6 });
        g.AddEdge(0, 1);
        g.AddEdge(1, 2);
        g.AddEdge(2, 3);
        g.AddEdge(2, 5);
        g.AddEdge(5, 4);
        g.AddEdge(5, 6);

        DFS dfs = new DFS();
        HashSet<int> visited = new HashSet<int>();
        List<int> path = new List<int>();
        List<int> res = dfs.RecursiveDFS(g.graph, 0, path, visited);
        foreach(int node in res)
        {
            Console.WriteLine(node);
        }

        HashSet<int> res2 = dfs.IterativeDFS(g.graph, 0, visited);
        foreach(int node in res2)
        {
            Console.WriteLine(node);
        }
    }
}
```
## Applications of DFS 
#### 1. Find minimum spanning tree and shortest paths
For a weighted graph, DFS traversal of the graph produces the minmum spanning tree and all pair shortest path tree.
#### 2. Detect cycle in a graph. 
A graph has a cycle if and only if there is a back edge during DFS.
#### 3. Find paths between two given tertices u and v. 
(1) Call DFS(graph, u) (2) Use stack to track path between start node and current node (3) When destination node v is encountered, build the path as the content of the stack.
#### 4. Topological sorting.
#### 5. Test if a graph is bipartite
#### 6. Find strongly connected component
#### 7. Solve puzzles with only one solution


## References
1. https://en.wikipedia.org/wiki/Depth-first_search
2. https://www.geeksforgeeks.org/applications-of-depth-first-search/
3. http://www.cs.cmu.edu/afs/cs/academic/class/15210-s12/www/lectures/lecture09.pdf
4. https://www.hackerearth.com/practice/algorithms/graphs/depth-first-search/tutorial/
