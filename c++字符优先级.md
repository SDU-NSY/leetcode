# 前言

在用如下代码使用迭代器访问二维数组时，编译器报错，十分疑惑，遂查找相关内容 

```c++
vector<vector<int>> a = {{2, 1}, {1, 2}};
cout << *(a.begin() + 1)[0];
```

# 尝试

这里就更疑惑了，就算解引用不成功，`->begin()`也应该是个指针啊，怎么可以输出`1`呢？

```c++
vector<vector<int>> a = {{2, 1}, {1, 2}};
cout << *(a.begin() + 1)->begin();
>> 1
```

# 询问

询问了`ACMer hyc`带佬，他猜测是优先级问题，`*`的优先级比较低，尝试将代码改成如下形式，成功输出，确认是优先级问题

```c++
vector<vector<int>> a = {{2, 1}, {1, 2}};
cout << (*(a.begin() + 1))[0];
>> 1
```

下表中说明 `->`优先级大于 `*`

![image-20220319234323480](C:\Users\nishiyu\AppData\Roaming\Typora\typora-user-images\image-20220319234323480.png)

`c++`运算符优先级详情可见[C++ 符号优先级_Stephen. K的博客-CSDN博客_c++符号优先级](https://blog.csdn.net/zb_915574747/article/details/99704639)

