# 01矩阵

> 给定一个由 0 和 1 组成的矩阵，找出每个元素到最近的 0 的距离。
>
> 两个相邻元素间的距离为 1 。

## 题目分析

这道题看似是多点到多点之间的路径距离，但其实**可以将所有0看作是一个单源点**，将题目变成**求单点到多点之间的最短距离**。

对0的集合单源点使用广度优先遍历，下一个遍历点的距离等于当前点的距离加1，更新剩下的所有为1的点即可，当然，更新过程中要注意遍历的重复情况，在本题中，只更新值为1且当前距离为0（未更新过）的点。

**技巧：**

**用一个数组存储上下左右四个方向，使用一个循环即可完成一次遍历。**

## 代码

```java
class Solution {
    int[][] d = {{0,1},{0,-1},{1,0},{-1,0}};

    public int[][] updateMatrix(int[][] matrix) {
        int[][] ans = new int[matrix.length][matrix[0].length];
        Deque<Integer>queue = new LinkedList<>();
        for(int i=0;i<matrix.length;i++)
            for(int j=0;j<matrix[0].length;j++){
                if(matrix[i][j]==0){
                    queue.addLast(i);
                    queue.addLast(j);
                }
            }
        while(!queue.isEmpty()){
            int x = queue.pollFirst();
            int y = queue.pollFirst();
            for(int i=0;i<4;i++){
                int nx = x + d[i][0];
                int ny = y + d[i][1];
                if(nx>=0 && ny>=0 && nx<matrix.length && ny<matrix[0].length && matrix[nx][ny]==1 && ans[nx][ny]==0){
                ans[nx][ny]=ans[x][y]+1;
                queue.addLast(nx);
                queue.addLast(ny);
                }
            }
        }
        return ans;
    }
}
```



