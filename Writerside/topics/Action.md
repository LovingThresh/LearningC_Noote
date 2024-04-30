# Action

�õģ�������һЩ��C++�г��õĲ������ʵ���ϸ���ͺ�ʾ�����룺

### 1. �󶨣�Binding��
��C++�У���ͨ��ָ��һ�����������ݻ������������磬���ú�ָ��ĳ�ʼ�����Ա���Ϊһ�ְ󶨡�

**ʾ������**��
```c++
int x = 10;
int& ref = x;  // ������ref������x
int* ptr = &x; // ��ָ��ptr������x�ĵ�ַ
```

### 2. ���ɣ�Generating��
���ɲ����漰�����µ�����ֵ������⾭����ʹ�������������������������캯��ʱ������

**ʾ������**��
```c++
#include <random>

std::random_device rd;
std::mt19937 gen(rd()); // ����һ�������������
std::uniform_int_distribution<> dis(1, 100);

int randomValue = dis(gen); // ����һ�������
```

### 3. ��ֵ��Assignment��
��ֵ�ǽ�һ��ֵ��һ���������ݸ���һ�������Ĺ��̡���C++�У���ֵ������`=`������ʵ����һ�㡣

**ʾ������**��
```c++
int a = 10;
int b;
b = a; // ��a��ֵ����b
```

### 4. ���죨Constructing��
�����Ǵ������󲢳�ʼ����ֵ�Ĺ��̡�ÿ������һ���¶���ʱ���乹�캯�������á�

**ʾ������**��
```c++
class MyClass {
public:
    MyClass(int value) : data(value) {}
private:
    int data;
};

MyClass obj(123); // ����һ��MyClass����
```

### 5. ������Destructing��
�����Ƕ����������ڽ���ʱ�Զ������Ĺ��̣�����������Դ��ִ��������������

**ʾ������**��
```c++
class MyClass {
public:
    ~MyClass() {
        std::cout << "Object is being destroyed" << std::endl;
    }
};

MyClass obj; // ��obj����������ʱ�������������ᱻ����
```

### 6. ���ƣ�Copying��
�����Ǵ���һ������ľ�ȷ�����Ĺ��̡���ͨ��ͨ���������캯����ɡ�

**ʾ������**��
```c++
class MyClass {
public:
    MyClass(const MyClass& other) : data(other.data) {} // �������캯��
private:
    int data;
};

MyClass original(100);
MyClass copy = original; // ʹ�ÿ������캯������һ������
```

### 7. �ƶ���Moving��
�ƶ��ǽ���Դ��һ������ת�Ƶ���һ������Ĺ��̣�ͨ���漰�����ݴ�һ�������ƶ�����һ�����󣬲�ʹԭ��������Ч��δ�����״̬��

**ʾ������**��
```c++
class MyClass {
public:
    MyClass(MyClass&& other) : data(other.data) {
        other.data = 0; // ��ԭ�����������Ϊ0
    }
private:
    int data;
};

MyClass source(100);
MyClass destination = std::move(source); // ʹ���ƶ����캯��
```

### 8. ������Swapping��
������ָ��������������ݻ�������C++�У�`std::swap`���������������������ֵ��

**ʾ������**��
```c++
int a = 10, b = 20;
std::swap(a, b); // ����a��b��ֵ
```

### 9. ���루Inserting��
�����ǽ�Ԫ����ӵ����ݽṹ�����������еĲ�����

**ʾ������**��
```c++
#include <vector>
std::vector<int> vec;
vec.push_back(10); // ��vector��β������ֵ10
```

### 10. ɾ����Erasing/Removing��
ɾ���Ǵ����ݽṹ���Ƴ�Ԫ�صĲ�����

**ʾ������**��
```c++
#include <vector>
std::vector<int> vec = {1, 2, 3, 4, 5};
vec.erase(vec.begin() + 2); // ɾ��λ��Ϊ2��

Ԫ�أ���3��
```

### 11. ������Searching��
�����������ݽṹ�в���Ԫ�صĹ��̡�

**ʾ������**��
```c++
#include <algorithm>
#include <vector>
std::vector<int> vec = {1, 2, 3, 4, 5};
auto it = std::find(vec.begin(), vec.end(), 3); // ��vector������ֵΪ3��Ԫ��
```

### 12. ����Sorting��
�����ǰ���һ����˳����֯���ݽṹ�е�Ԫ�ء�

**ʾ������**��
```c++
#include <algorithm>
#include <vector>
std::vector<int> vec = {5, 3, 1, 4, 2};
std::sort(vec.begin(), vec.end()); // ��vector�е�Ԫ����������
```

### 13. ������Iterating��
�����Ǳ������ݽṹ�е�Ԫ�صĹ��̡�

**ʾ������**��
```c++
#include <vector>
std::vector<int> vec = {1, 2, 3, 4, 5};
for (int value : vec) {
    std::cout << value << " ";
}
```

��Щ������C++����г��õ�һЩ���������������ڹ��������������ڡ����ݽṹ�Ĳ����ͳ�����Ϊ�Ŀ��ơ�������и�������������Ҫ��һ���Ľ��ͣ�������ң�