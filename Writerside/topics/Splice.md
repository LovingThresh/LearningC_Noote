# Splice

![list_splice.png](list_splice.png)

## 重载示例
以下是用中文描述的 `std::list::splice` 函数重载版本的详细示例：

1. **`void splice(const_iterator pos, list& other);`**
   这个重载版本将 `other` 列表中的所有元素转移到当前列表（`this`），插入到位置 `pos`。操作后，`other` 列表将变为空。
   ```c++
   #include <iostream>
   #include <list>

   int main() {
       std::list<int> list1 = {1, 2, 3};
       std::list<int> list2 = {4, 5, 6};
       list1.splice(list1.begin(), list2);

       for (int n : list1) {
           std::cout << n << ' ';
       }
       std::cout << std::endl;

       return 0;
   }
   ```

2. **`void splice(const_iterator pos, list&& other);`**
   与第一个类似，但它明确表明 `other` 可以是右值，如果 `other` 是临时对象，则可能更有效地进行转移。
   ```c++
   #include <iostream>
   #include <list>

   int main() {
       std::list<int> list1 = {1, 2, 3};
       list1.splice(list1.begin(), std::list<int>{7, 8, 9});

       for (int n : list1) {
           std::cout << n << ' ';
       }
       std::cout << std::endl;

       return 0;
   }
   ```

3. **`void splice(const_iterator pos, list& other, const_iterator it);`**
   将 `other` 中由 `it` 指向的元素移动到当前列表的 `pos` 位置。
   ```c++
   #include <iostream>
   #include <list>

   int main() {
       std::list<int> list1 = {1, 2, 3};
       std::list<int> list2 = {4, 5, 6};
       auto it = std::next(list2.begin(), 1); // 指向元素 5
       list1.splice(list1.begin(), list2, it);

       for (int n : list1) {
           std::cout << n << ' ';
       }
       std::cout << std::endl;

       return 0;
   }
   ```

4. **`void splice(const_iterator pos, list&& other, const_iterator it);`**
   类似第三个，但允许 `other` 为右值。
   ```c++
   #include <iostream>
   #include <list>

   int main() {
       std::list<int> list1 = {1, 2, 3};
       std::list<int> list2 = {7, 8, 9};
       list1.splice(list1.begin(), std::move(list2), list2.begin());

       for (int n : list1) {
           std::cout << n << ' ';
       }
       std::cout << std::endl;

       return 0;
   }
   ```

5. **`void splice(const_iterator pos, list& other, const_iterator first, const_iterator last);`**
   从 `other` 列表中转移 [first, last) 范围内的元素到当前列表的 `pos` 位置。
   ```c++
   #include <iostream>
   #include <list>

   int main() {
       std::list<int> list1 = {1, 2, 3};
       std::list<int> list2 = {4, 5, 6, 7};
       auto first = list2.begin();
       auto last = std::next(first, 2); // 不包括 6
       list1.splice(list1.begin(), list2, first, last);

       for (int n : list1) {
           std::cout << n << ' ';
       }
       std::cout << std::endl;

       return 0;
   }
   ```

6. **`void splice(const_iterator pos, list&& other, const_iterator first, const_iterator last);`**
   类似第五个，但 `other` 可以是右值。适用于从临时列表中有效地转移一段范围的元素。
   ```c++
   #include <iostream>
   #include <list>

   int main() {
       std::list<int> list1 = {1, 2, 3};
       std::list<int> list2 = {10, 11, 12, 13};
       t1.splice(list1.begin(), std::move(list2), 
                 std::next(list2.begin()), 
       std::next(list2.begin(), 3));

       for (int n : list1) {
           std::cout << n << ' ';
       }
       std::cout << std::endl;
       return 0;
   }
   ```

这些示例展示了如何在 C++ 中使用 `std::list` 的 `splice()` 函数的各种重载形式。