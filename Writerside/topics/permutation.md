# permutation

�� C++ ��׼���У������У�permutation����صĺ�����Ҫ���� `std::next_permutation` �� `std::prev_permutation`����Щ�����������ɸ������е���һ������һ���ֵ������У��Ǵ������������ǿ�󹤾ߡ�����ͨ������ʵ���㷨��Ҫ����һ�����ϵ��������еĳ�����

### std::next_permutation

`std::next_permutation` ��������������������һ�����ֵ����������С���������������������У��򷵻� `true`����������Ѿ���������У��������ڸ�������У����򷵻� `false` �������лָ�Ϊ��С���У����ֵ�����С�����Ӷ�����ѭ�������������С�

#### �÷�ʾ��
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
�����
```
1 2 3 
1 3 2 
2 1 3 
2 3 1 
3 1 2 
3 2 1 
```
��δ������������ `[1, 2, 3]` ���������С�

### std::prev_permutation

�� `std::next_permutation` �෴��`std::prev_permutation` ��������������������һ�����ֵ����С�����С���������������������У��򷵻� `true`����������Ѿ�����С���У��򷵻� `false` �������лָ�Ϊ������У�ͬ������ѭ��������

#### �÷�ʾ��
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
�����
```
3 2 1 
3 1 2 
2 3 1 
2 1 3 
1 3 2 
1 2 3 
```
��δ������������ `[3, 2, 1]` ���������У����ݼ��ֵ������С�

### �ܽ�
`std::next_permutation` �� `std::prev_permutation` �ṩ��һ�ּ򵥶�ֱ�ӵķ�ʽ�������������ϵ��������У�����ݹ顣���ǹ㷺�����㷨��������ѧ��������������Ҫ�������п������еĳ��ϡ���Щ����ͨ����������Ԫ��ֱ���޸�ԭ���У�ʹ��ÿ�ε��ú󶼵õ�һ���µ����С�