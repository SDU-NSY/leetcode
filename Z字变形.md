分析可得，`Z`字变形先竖着填充，然后斜着填充，再竖着填充。

因此我们可以建立一个矩阵，先将这些字符都填充到其指定的位置上，然后按行取出即可。

我们可以将填充位置拆分成`numRows * numRows`的小矩阵看，每个小矩阵内的填充又分为竖着的部分和斜着的部分。

我们以`[row, col]`来定位当前填充位置

填充条件可以定义为`row < numRows - 1 && col % (numRows - 1) == 0`。

满足，则处于竖着填充的状态，执行`row++`

否则，处于斜着填充的状态，执行`row-- col++`

```c++
class Solution {
public:
    string convert(string s, int numRows) {
        if(numRows == 1) return s;
        string res = "";
        vector<vector<char>> con(numRows, vector<char>(s.size())); 
        for(int i = 0; i < numRows; i++)
            for(int j = 0; j < s.size(); j++)
                con[i][j] = '?'; //初始化，为了将未填充的位置和填充了的位置区分开
        
        int row = 0, col = 0;
        for(int i = 0; i < s.size(); i++){
            con[row][col] = s[i];
            if(row < numRows - 1 && col % (numRows - 1) == 0) row++;
            else row--, col++;
        }
        for(int i = 0; i < numRows; i++)
            for(int j = 0; j < s.size(); j++)
                if(con[i][j] != '?')
                    res += con[i][j];
        return res;
    }
};
```

