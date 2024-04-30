# rotate &amp; reverse

�� C++ ��׼���У�`std::rotate` �� `std::reverse` ���������õ��㷨�����Ƕ������޸����е�˳�򣬵����Ե���;�Ͳ�����ʽ������ͬ��

### std::rotate
`std::rotate` ���ڽ������е�Ԫ�������������ת��ʹ���ض���Ԫ�س�Ϊ���еĿ�ʼ��������˵��`std::rotate` ��������������������`first`��`middle`��`last`�������Ľ���ǽ� `[first, middle)` ��Χ�ڵ�Ԫ����ת�����еĺ󲿣��� `[middle, last)` ��Χ�ڵ�Ԫ�����ƶ������е�ǰ�����ܵ���˵��`middle` ָ���Ԫ�ؽ���Ϊ�����еĵ�һ��Ԫ�ء�

#### �÷�ʾ��
������һ������ `v = {1, 2, 3, 4, 5}`���������ǵ��� `std::rotate(v.begin(), v.begin() + 2, v.end())`��

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

������� `3 4 5 1 2`����ΪԪ�� `3` ��Ϊ�������еĿ�ʼ��

### std::reverse
�� `std::rotate` ��ȣ�`std::reverse` ���򵥡���ֻ��Ҫ����������������`first` �� `last`������㷨�� `[first, last)` ��Χ�ڵ�Ԫ��˳��ߵ��������� `rotate` �����ƶ�Ԫ�ص����еĲ�ͬ���֣�����ֱ����ԭ�ط�ת���������˳��

#### �÷�ʾ��
ͬ�������� `v = {1, 2, 3, 4, 5}`�����ǵ��� `std::reverse(v.begin(), v.end())`��

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

������� `5 4 3 2 1`����Ϊ�������е�˳�򱻷�ת�ˡ�

### �ܽ�
`std::rotate` �� `std::reverse` �����޸����е����ù��ߡ�`rotate` ���ڻ���ĳ������ת���У��� `reverse` ���ڼ򵥵ط�ת�������С��������㷨���������µ����У�����ֱ���޸�ԭ���С���ʹ����Щ�㷨ʱ����Ӧ��ȷ�����ݸ����ǵĵ�������Χ����Ч�ģ��Ա���δ������Ϊ��