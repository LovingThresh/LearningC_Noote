# rotate &amp; reverse

在 C++ 标准库中，`std::rotate` 和 `std::reverse` 是两个常用的算法，它们都用于修改序列的顺序，但各自的用途和操作方式有所不同。

### std::rotate
`std::rotate` 用于将序列中的元素向左或向右旋转，使得特定的元素成为序列的开始。具体来说，`std::rotate` 接受三个迭代器参数：`first`、`middle`、`last`。操作的结果是将 `[first, middle)` 范围内的元素旋转到序列的后部，而 `[middle, last)` 范围内的元素则移动到序列的前部。总的来说，`middle` 指向的元素将成为新序列的第一个元素。

#### 用法示例
假设有一个向量 `v = {1, 2, 3, 4, 5}`，并且我们调用 `std::rotate(v.begin(), v.begin() + 2, v.end())`：

```c++
#include <vector>
#include <algorithm>
#include <iostream>

int main() {
    std::vector<int> v = {1, 2, 3, 4, 5};
    std::rotate(v.begin(), v.begin() + 2, v.end());

    for (int x : v) {
        std::cout << x << " ";
    }
}
```

输出将是 `3 4 5 1 2`，因为元素 `3` 成为了新序列的开始。

### std::reverse
与 `std::rotate` 相比，`std::reverse` 更简单。它只需要两个迭代器参数：`first` 和 `last`。这个算法将 `[first, last)` 范围内的元素顺序颠倒。它不像 `rotate` 那样移动元素到序列的不同部分，而是直接在原地反转整个区间的顺序。

#### 用法示例
同样的向量 `v = {1, 2, 3, 4, 5}`，我们调用 `std::reverse(v.begin(), v.end())`：

```c++
#include <vector>
#include <algorithm>
#include <iostream>

int main() {
    std::vector<int> v = {1, 2, 3, 4, 5};
    std::reverse(v.begin(), v.end());

    for (int x : v) {
        std::cout << x << " ";
    }
}
```

输出将是 `5 4 3 2 1`，因为整个序列的顺序被反转了。

### 总结
`std::rotate` 和 `std::reverse` 都是修改序列的有用工具。`rotate` 用于基于某个点旋转序列，而 `reverse` 用于简单地反转整个序列。这两个算法都不生成新的序列，而是直接修改原序列。在使用这些算法时，你应该确保传递给它们的迭代器范围是有效的，以避免未定义行为。