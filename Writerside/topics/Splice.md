# Splice

![list_splice.png](list_splice.png)

## ����ʾ��
������������������ `std::list::splice` �������ذ汾����ϸʾ����

1. **`void splice(const_iterator pos, list& other);`**
   ������ذ汾�� `other` �б��е�����Ԫ��ת�Ƶ���ǰ�б�`this`�������뵽λ�� `pos`��������`other` �б���Ϊ�ա�
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
   ���һ�����ƣ�������ȷ���� `other` ��������ֵ����� `other` ����ʱ��������ܸ���Ч�ؽ���ת�ơ�
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
   �� `other` ���� `it` ָ���Ԫ���ƶ�����ǰ�б�� `pos` λ�á�
   ```c++
   #include <iostream>
   #include <list>

   int main() {
       std::list<int> list1 = {1, 2, 3};
       std::list<int> list2 = {4, 5, 6};
       auto it = std::next(list2.begin(), 1); // ָ��Ԫ�� 5
       list1.splice(list1.begin(), list2, it);

       for (int n : list1) {
           std::cout << n << ' ';
       }
       std::cout << std::endl;

       return 0;
   }
   ```

4. **`void splice(const_iterator pos, list&& other, const_iterator it);`**
   ���Ƶ������������� `other` Ϊ��ֵ��
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
   �� `other` �б���ת�� [first, last) ��Χ�ڵ�Ԫ�ص���ǰ�б�� `pos` λ�á�
   ```c++
   #include <iostream>
   #include <list>

   int main() {
       std::list<int> list1 = {1, 2, 3};
       std::list<int> list2 = {4, 5, 6, 7};
       auto first = list2.begin();
       auto last = std::next(first, 2); // ������ 6
       list1.splice(list1.begin(), list2, first, last);

       for (int n : list1) {
           std::cout << n << ' ';
       }
       std::cout << std::endl;

       return 0;
   }
   ```

6. **`void splice(const_iterator pos, list&& other, const_iterator first, const_iterator last);`**
   ���Ƶ�������� `other` ��������ֵ�������ڴ���ʱ�б�����Ч��ת��һ�η�Χ��Ԫ�ء�
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

��Щʾ��չʾ������� C++ ��ʹ�� `std::list` �� `splice()` �����ĸ���������ʽ��