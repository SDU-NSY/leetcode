## BFS和DFS

#### 图像渲染

**题目**

- 有一幅以二维整数数组表示的图画，每一个整数表示该图画的像素值大小，数值在 0 到 65535 之间。

  给你一个坐标 (sr, sc) 表示图像渲染开始的像素值（行 ，列）和一个新的颜色值 newColor，让你重新上色这幅图像。

  为了完成上色工作，从初始坐标开始，记录初始坐标的上下左右四个方向上像素值与初始坐标相同的相连像素点，接着再记录这四个方向上符合条件的像素点与他们对应四个方向上像素值与初始坐标相同的相连像素点，……，重复该过程。将所有有记录的像素点的颜色值改为新的颜色值。

  最后返回经过上色渲染后的图像。


**思路**

- 本题就是从一个位置sr，sc开始，修改当前位置颜色为新的颜色。然后将相邻位置的颜色与该位置之前的颜色进行比较，如果颜色相同，则修改相邻位置的颜色，并且从该位置开始继续判断相邻位置是否需要修改颜色。所以想到，从sr，sc开始进行bfs，而且该搜索不需要visit数组标记满足条件的位置是否已经访问过。因为当访问满足条件的位置时，该位置的颜色将会被修改，因此下一次再遍历到该位置时，该位置不满足判定条件，所以不会再访问已经访问过的位置。
- BFS

```c++
class Solution {
public:
    int dx[4] = {1 , 0 , 0 , -1};
    int dy[4] = {0 , 1 , -1 , 0};
    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int newColor) {
        int currentColor = image[sr][sc];
        if(currentColor == newColor) return image;
        int mx = image.size() , my = image[0].size();
        queue<pair<int,int>> q;
        image[sr][sc] = newColor;
        q.emplace(sr,sc);
        while(!q.empty()){
            int x = q.front().first;
            int y = q.front().second;
            q.pop();
            for(int i = 0 ; i < 4 ; i++){
                int newx = x + dx[i] , newy = y + dy[i];
                if(newx >= 0 && newx < mx && newy >= 0 && newy < my && image[newx][newy] == currentColor){
                    image[newx][newy] = newColor;
                    q.emplace(newx,newy);
                }
            }
        }
        return image;
    }
};
```

- DFS
- 本题为一般的搜索问题，在我的其他题解中，详细地解释了题目，并用bfs进行解答，这里使用dfs。

```c++
class Solution {
public:
    int dx[4] = {1 , 0 , 0 , -1};
    int dy[4] = {0 , 1 , -1 , 0};

    void dfs(int x , int y , vector<vector<int>>& image , int newColor , int currentColor){
        int mx = image.size() , my = image[0].size();
        image[x][y] = newColor;
        for(int i = 0 ; i < 4 ; i++){
            int newx = x + dx[i] , newy = y + dy[i];
            if(newx >= 0 && newx < mx && newy >= 0 && newy < my && image[newx][newy] == currentColor)
            dfs(newx , newy , image , newColor , currentColor);
        }
    }
    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int newColor) {
        int currentColor = image[sr][sc];
        if(currentColor == newColor) return image;
        dfs(sr,sc , image , newColor , currentColor);
        return image;
    }
};
```

#### 岛屿的最大面积

**题目**

- 给定一个包含了一些 0 和 1 的非空二维数组 grid 。

  一个 岛屿 是由一些相邻的 1 (代表土地) 构成的组合，这里的「相邻」要求两个 1 必须在水平或者竖直方向上相邻。你可以假设 grid 的四个边缘都被 0（代表水）包围着。

  找到给定的二维数组中最大的岛屿面积。(如果没有岛屿，则返回面积为 0 。

**解题思路**

- 本题易知，为搜索型问题。将搜索到的为1的位置改为0，可以避免使用visit数组来标记一个位置是否已经被访问过。因为每个位置都可能不满足grid[x][y]为1的条件，所以可以将判定条件放在dfs函数的开始位置。dfs函数遍历每个位置，每个位置只会返回0或1，通过将这些返回值相加可以确定该位置所在的岛屿的大小。最大的岛屿取这些岛屿的最大值即可

- DFS

```c++
class Solution {
public:
    int dx[4] = {-1 , 0 , 0 , 1};
    int dy[4] = {0 , -1 , 1 , 0};
    int dfs(vector<vector<int>>& grid , int x , int y){
        if(x < 0 || x == grid.size() || y < 0 || y == grid[0].size() || grid[x][y] == 0)
        return 0;
        int ans = 1;
        grid[x][y] = 0;
        for(int i = 0 ; i < 4 ; i++){
            int newx = x + dx[i] , newy = y + dy[i];
            ans = ans + dfs(grid , newx , newy);
        }
        return ans;
    }
    int maxAreaOfIsland(vector<vector<int>>& grid) {
        int ans = 0;
        for(int i = 0 ; i < grid.size() ; i++){
            for(int j = 0 ; j < grid[0].size() ; j++){
                ans = max(ans , dfs(grid , i , j));
            }
        }
        return ans;
    }
};
```

- BFS
- 本题使用bfs遍历所有位置，在遍历grid[i][j] = 1的位置的过程中，修改grid[i][j] = 0，并计算岛屿的大小，最后输出最大值

```c++
class Solution {
public:
    int dx[4] = {-1 , 0 , 0 , 1};
    int dy[4] = {0 , -1 , 1 , 0};
    queue<pair<int,int>> q;
    int ans = 0;
    int maxAreaOfIsland(vector<vector<int>>& grid){
       queue<pair<int,int>> q;
       for(int i = 0 ; i < grid.size() ; i++){
           for(int j = 0 ; j < grid[0].size() ; j++){
               if(grid[i][j] == 1){
                   int cnt = 0;
                   while(!q.empty()) q.pop();
                   q.emplace(i,j);
                   grid[i][j] = 0;
                   while(!q.empty()){
                       int x = q.front().first , y = q.front().second;
                       cnt++;
                       q.pop();
                       for(int i = 0 ; i < 4 ; i++){
                           int newx = x + dx[i] , newy = y + dy[i];
                           if(newx >= 0 && newx < grid.size() && newy >= 0 && newy < grid[0].size() && grid[newx][newy] == 1){
                               grid[newx][newy] = 0;
                               q.emplace(newx , newy);
                           }
                       }
                   }
                   ans = max(ans , cnt);
               }
           }
       }
       return ans;
    }
};
```

#### 01矩阵

==图的多源BFS==

**题目**

- 给定一个由 0 和 1 组成的矩阵 mat ，请输出一个大小相同的矩阵，其中每一个格子是 mat 中对应位置元素到最近的 0 的距离。

- 两个相邻元素间的距离为 1

- **示例 1：**

  ![img](https://pic.leetcode-cn.com/1626667201-NCWmuP-image.png)

  ```c++
  输入：mat = [[0,0,0],[0,1,0],[0,0,0]]
  输出：[[0,0,0],[0,1,0],[0,0,0]]
  ```

**示例 2：**

![img](https://pic.leetcode-cn.com/1626667205-xFxIeK-image.png)

```
输入：mat = [[0,0,0],[0,1,0],[1,1,1]]
输出：[[0,0,0],[0,1,0],[1,2,1]]
```

**解题思路**

- 本题考察的是多源BFS。在看题解之前，我使用了暴力解法，即遍历所有点，对每个点使用BFS，最终结果显然超时。本题在进行BFS前，先让所有的0都入队列。我们可以从下两个方面理解。
  第一：先让所有0入队列，意味着我们会先对这些0做BFS，且每次只进行一层，然后将这一层中遇到的可以进行BFS的位置加入队列，然后我们使用队列中的下一个0进行BFS。也就是说，对所有0进行BFS，等于我们对于距离每个0为1的点都进行了检测。因此不需要考虑在队列中进行BFS的0的顺序，因为不论我们使用哪个0，只要在第一层遍历中检测到了这个位置，那么这个位置的距离就是1。然后所有0出队列之后，我们再取点，相当于进行第二层检测。所以多源BFS可以理解为分层向四周扩散，每个dis[newx][newy] = dis[x][y] + 1.

  第二：我们可以把所有0看成一个整体，如果所有0能看为一个整体的话，那么每个1距离0的最短距离就是这个1到这个整体的距离。所以我们可以将这个整体看作一个超级源点，这个超级源点与所有的0相连，且到所有0的距离都为0.这样的话，我们从这个超级源点开始做BFS，就能得到所有1到达这个超级源点的距离。从超级源点进行BFS的第一步就是将所有的0入队列，然后将超级源点从队列中清除。这就等价于直接将所有的0入队列，然后进行BFS。

```c++
#include<cstring>
class Solution {
public:
    int dx[4] = {-1 , 0 , 0 , 1};
    int dy[4] = {0 , -1 , 1 , 0};
    vector<vector<int>> updateMatrix(vector<vector<int>>& mat) {
        int n = mat.size() , m = mat[0].size();
        vector<vector<int>> dist(n , vector<int>(m));
        vector<vector<int>> visit(n , vector<int>(m));
        queue<pair<int,int>> q;
        for(int i = 0; i < n ; i++)
            for(int j = 0 ; j < m ;j++)
                if(mat[i][j] == 0) 
                    q.emplace(i , j),visit[i][j] = 1;

        while(!q.empty()){
            int x = q.front().first , y = q.front().second;
            q.pop();
            for(int i = 0 ; i < 4 ; i++){
                int newx = x + dx[i] , newy = y + dy[i];
                if(newx >= 0 && newx < n && newy >= 0 && newy < m && !visit[newx][newy]){
                    dist[newx][newy] = dist[x][y] + 1;
                    q.emplace(newx , newy);
                    visit[newx][newy] = 1;
                }
            }
        }
        return dist;
    }
};
```



