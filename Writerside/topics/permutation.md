# permutation

在 C++ 标准库中，与排列（permutation）相关的函数主要包括 `std::next_permutation` 和 `std::prev_permutation`。这些函数用于生成给定序列的下一个或上一个字典序排列，是处理排列问题的强大工具。它们通常用于实现算法需要遍历一个集合的所有排列的场景。

### std::next_permutation

`std::next_permutation` 重新排列序列以生成下一个按字典序更大的排列。如果可以生成这样的排列，则返回 `true`；如果序列已经是最大排列（即不存在更大的排列），则返回 `false` 并将序列恢复为最小排列（即字典序最小），从而方便循环遍历所有排列。

#### 用法示例
```c++
#include <algorithm>
#include <vector>
#include <iostream>

int main() {
    std::vector<int> v = {1, 2, 3};
    do {
        for (int x : v) {
            std::cout << x << " ";
        }
        std::cout << "\n";
    } while (std::next_permutation(v.begin(), v.end()));
}
```
输出：
```
1 2 3 
1 3 2 
2 1 3 
2 3 1 
3 1 2 
3 2 1 
```
这段代码遍历了数组 `[1, 2, 3]` 的所有排列。

### std::prev_permutation

与 `std::next_permutation` 相反，`std::prev_permutation` 重新排列序列以生成下一个按字典序更小的排列。如果可以生成这样的排列，则返回 `true`；如果序列已经是最小排列，则返回 `false` 并将序列恢复为最大排列，同样方便循环遍历。

#### 用法示例
```c++
#include <algorithm>
#include <vector>
#include <iostream>

int main() {
    std::vector<int> v = {3, 2, 1};
    do {
        for (int x : v) {
            std::cout << x << " ";
        }
        std::cout << "\n";
    } while (std::prev_permutation(v.begin(), v.end()));
}
```
输出：
```
3 2 1 
3 1 2 
2 3 1 
2 1 3 
1 3 2 
1 2 3 
```
这段代码遍历了数组 `[3, 2, 1]` 的所有排列，按递减字典序排列。

### 总结
`std::next_permutation` 和 `std::prev_permutation` 提供了一种简单而直接的方式来遍历给定集合的所有排列，无需递归。它们广泛用于算法竞赛、数学问题求解和其他需要考虑所有可能排列的场合。这些函数通过重新排列元素直接修改原序列，使得每次调用后都得到一个新的排列。