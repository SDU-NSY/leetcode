# 前言

- 使用之前需要引入头文件`#include<algorithm>`
- 该函数的功能是对指定范围内的值进行翻转。

# 用法

- 语法`reverse(begin, begin + n)`，`n` 是翻转元素的个数
  - 翻转范围为`[begin,begin + n)`，是左闭右开区间

# 举例

### 普通数组

```c++
int a[] = {1,2,3,4};
reverse(a, a + 2);
for(int b : a) cout << b << ' ';
>>2 1 3 4
```

### STL容器

```c++
vector<int> a = {1, 2, 3, 4};
reverse(a.begin(), a.end());
for(int b : a) cout << b << ' ';
>>4 3 2 1
```



