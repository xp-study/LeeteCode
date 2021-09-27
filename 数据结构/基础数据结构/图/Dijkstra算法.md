# 单源最短路径问题

![单源最短路径问题](./images/Dijkstra算法/1.jpg)

![单源最短路径问题](./images/Dijkstra算法/2.jpg)

![单源最短路径问题](./images/Dijkstra算法/3.jpg)

![单源最短路径问题](./images/Dijkstra算法/4.jpg)

![单源最短路径问题](./images/Dijkstra算法/5.jpg)

![单源最短路径问题](./images/Dijkstra算法/6.jpg)

第一层，遍历顶点A：

![单源最短路径问题](./images/Dijkstra算法/7.jpg)

第二层，遍历A的邻接顶点B和C：

![单源最短路径问题](./images/Dijkstra算法/8.jpg)

第三层，遍历顶点B的邻接顶点D、E，遍历顶点C的邻接顶点F：

![单源最短路径问题](./images/Dijkstra算法/9.jpg)

第四层，遍历顶点E的邻接顶点G，也就是目标节点：

![单源最短路径问题](./images/Dijkstra算法/10.jpg)

由此得出，图中顶点A到G的（第一条）最短路径是A-B-E-G：

![单源最短路径问题](./images/Dijkstra算法/11.jpg)

![单源最短路径问题](./images/Dijkstra算法/12.jpg)

换句话说，就是寻找从A到G之间，权值之和最小的路径。

![单源最短路径问题](./images/Dijkstra算法/13.jpg)

![单源最短路径问题](./images/Dijkstra算法/14.jpg)

![单源最短路径问题](./images/Dijkstra算法/15.jpg)

————————————

![单源最短路径问题](./images/Dijkstra算法/16.jpg)

![单源最短路径问题](./images/Dijkstra算法/17.jpg)

![单源最短路径问题](./images/Dijkstra算法/18.jpg)

![单源最短路径问题](./images/Dijkstra算法/19.jpg)

![单源最短路径问题](./images/Dijkstra算法/20.jpg)

![单源最短路径问题](./images/Dijkstra算法/21.jpg)

究竟什么是迪杰斯特拉算法？它是如何寻找图中顶点的最短路径呢？

这个算法的本质，是不断刷新起点与其他各个顶点之间的 “距离表”。

让我们来演示一下迪杰斯特拉的详细过程：

第1步，创建距离表。表中的Key是顶点名称，Value是**从起点A到对应顶点的已知最短距离**。但是，一开始我们并不知道A到其他顶点的最短距离是多少，Value默认是无限大：

![单源最短路径问题](./images/Dijkstra算法/22.jpg)

第2步，遍历起点A，找到起点A的邻接顶点B和C。从A到B的距离是5，从A到C的距离是2。把这一信息刷新到距离表当中：

![单源最短路径问题](./images/Dijkstra算法/23.jpg)

第3步，从距离表中找到从A出发距离最短的点，也就是顶点C。

第4步，遍历顶点C，找到顶点C的邻接顶点D和F（A已经遍历过，不需要考虑）。从C到D的距离是6，所以A到D的距离是2+6=8；从C到F的距离是8，所以从A到F的距离是2+8=10。把这一信息刷新到表中：

![单源最短路径问题](./images/Dijkstra算法/24.jpg)

接下来重复第3步、第4步所做的操作：

第5步，也就是第3步的重复，从距离表中找到从A出发距离最短的点（C已经遍历过，不需要考虑），也就是顶点B。

第6步，也就是第4步的重复，遍历顶点B，找到顶点B的邻接顶点D和E（A已经遍历过，不需要考虑）。从B到D的距离是1，所以A到D的距离是5+1=6，**小于距离表中的8**；从B到E的距离是6，所以从A到E的距离是5+6=11。把这一信息刷新到表中：

![单源最短路径问题](./images/Dijkstra算法/25.jpg)

（在第6步，A到D的距离从8刷新到6，可以看出距离表所发挥的作用。距离表通过迭代刷新，用新路径长度取代旧路径长度，最终可以得到从起点到其他顶点的最短距离）

第7步，从距离表中找到从A出发距离最短的点（B和C不用考虑），也就是顶点D。

第8步，遍历顶点D，找到顶点D的邻接顶点E和F。从D到E的距离是1，所以A到E的距离是6+1=7，**小于距离表中的11**；从D到F的距离是2，所以从A到F的距离是6+2=8，**小于距离表中的10**。把这一信息刷新到表中：

![单源最短路径问题](./images/Dijkstra算法/26.jpg)

第9步，从距离表中找到从A出发距离最短的点，也就是顶点E。

第10步，遍历顶点E，找到顶点E的邻接顶点G。从E到G的距离是7，所以A到G的距离是7+7=14。把这一信息刷新到表中：

![单源最短路径问题](./images/Dijkstra算法/27.jpg)

第11步，从距离表中找到从A出发距离最短的点，也就是顶点F。

第10步，遍历顶点F，找到顶点F的邻接顶点G。从F到G的距离是3，所以A到G的距离是8+3=11，**小于距离表中的14**。把这一信息刷新到表中：

![单源最短路径问题](./images/Dijkstra算法/28.jpg)

就这样，除终点以外的全部顶点都已经遍历完毕，距离表中存储的是从起点A到所有顶点的最短距离。显然，从A到G的最短距离是11。（路径：A-C-D-F-G）

按照上面的思路，我们来看一下代码实现：

```java
    public int[] Dijkstra(int[][] times, int n, int k) {
        Map<Integer, List<int[]>> graph = new HashMap<>();
        for (int[] time : times) {
            graph.computeIfAbsent(time[0], t -> new ArrayList<>()).add(new int[]{time[1], time[2]});
        }

        int[] distances = new int[n + 1];
        Arrays.fill(distances, Integer.MAX_VALUE);

        boolean[] visited = new boolean[n + 1];

        // 起点的距离设置为0
        distances[k] = 0;

        // 优先队列 按照从小到大排列
        PriorityQueue<Integer> queue = new PriorityQueue((o1, o2) -> distances[(int) o1] - distances[(int) o2]);
        // 将起点放入队列
        queue.offer(k);
        while (!queue.isEmpty()) {
            Integer cur = queue.poll();
            if (visited[cur]) {
                continue;
            }
            visited[cur] = true;

            List<int[]> neighbors = graph.getOrDefault(cur, new ArrayList<int[]>());

            for (int[] neighbor : neighbors) {
                int to = neighbor[0];
                if (visited[to]) {
                    continue;
                }
                // 松弛操作
                distances[to] = Math.min(distances[to], distances[cur] + neighbor[1]);
                queue.add(to);
            }
        }
        return distances;
    }
```

堆的写法复杂度如下（v是点的个数,e是边的个数）：

* 时间复杂度：O(ve)

* 空间复杂度：O(v+e)。

