# Operator * &amp;

- 在C++中，`*`和`&`具有多种含义和用法，它们的作用取决于它们所处的上下文。


## `&` -> 地址运算符/引用符号

### 作为地址运算符

当`&`用作一元运算符时，它会返回变量的内存地址。

```C++
int var = 10;
int* ptr = &var; // ptr现在包含var的地址
```

### 作为引用声明符号

当`&`在类型声明中使用时，它创建了一个引用。

```C++
int var = 10;
int& ref = var; // ref是var的引用
```

引用是某个已存在对象的另一个名字，它必须在声明时被初始化，并且一旦与某个对象绑定，就不能再改变绑定到另一个对象。

## `*` -> 指针运算符/解引用符号

### 作为指针声明符号

当`*`用在类型声明中时，它表示一个指针类型。

```C++
int var = 10;
int* ptr = &var; // ptr是指向int类型的指针
```

### 作为解引用运算符

当`*`作为一元运算符使用时，它会解引用一个指针，也就是返回指针所指向地址的值。

```C++
int var = 10;
int* ptr = &var;
int value = *ptr; // value现在是10，是ptr指向的内存位置的值
```

### 使用场景比较

- **变量地址**：获取变量的内存地址。
- **创建引用**：创建一个别名，即引用另一个变量。
- **声明指针**：声明一个变量，它可以存储内存地址。
- **解引用**：通过指针访问它所指向的值。

### 示例代码

下面是一个使用这些运算符的示例代码：

```C++
#include <iostream>

int main() {
    int var = 10; // 普通变量
    int* ptr = &var; // 指针，存储var的地址
    int& ref = var; // 引用，别名为var

    std::cout << "var: " << var << "\n";
    std::cout << "ptr (address): " << ptr << "\n";
    std::cout << "*ptr (dereferenced value): " << *ptr << "\n";
    std::cout << "ref: " << ref << "\n";

    // 修改引用和指针指向的值
    *ptr = 20;
    std::cout << "var (after *ptr = 20): " << var << "\n";

    ref = 30;
    std::cout << "var (after ref = 30): " << var << "\n";

    return 0;
}
```

在这个示例中，`var`是一个普通的整数变量，`ptr`是一个指向`var`的指针，`ref`是`var`的引用。通过`*ptr`可以修改`var`的值，因为`*ptr`解引用了指向`var`的指针。同样，通过修改`ref`也会改变`var`的值，因为`ref`是`var`的一个引用。

## `&` -> 引用参数/返回类型

当`&`用在函数参数类型前时，它表示参数将以引用的方式传递，这允许函数直接修改传入的参数。

```C++
void increment(int& value) {
    value++; // 直接修改传入的参数
}
```

当`&`用在函数返回类型前时，它表示函数返回一个引用。

```C++
int& getElement(int* array, int index) {
    return array[index]; // 返回数组元素的引用
}
```

返回引用时要特别注意，不要返回局部变量的引用，因为局部变量在函数返回后会被销毁，这样会导致未定义的行为。

## `*` -> 指针参数/返回类型

当`*`用在函数参数类型前时，它表示参数将以指针的方式传递。

```C++
void setToNull(int* ptr) {
    ptr = nullptr; // 将传入的指针设置为nullptr
}
```

当`*`用在函数返回类型前时，它表示函数返回一个指针。

```C++
int* getPointer(int& value) {
    return &value; // 返回指向value的指针
}
```

与返回引用类似，返回指针时也要确保你没有返回指向局部变量的指针。

### 使用场景比较

- **函数参数传递**：
    - 使用`&`：按引用传递，允许函数修改外部变量。
    - 使用`*`：按指针传递，也可以修改外部变量，但是使用方式不同，需要解引用。

- **函数返回值**：
    - 使用`&`：返回引用，通常用于返回参数传递的对象或者全局/静态对象。
    - 使用`*`：返回指针，可以是新分配的内存，也可以是参数传递的对象的地址。

下面是一个包含这些概念的示例代码：

```C++
#include <iostream>

void modifyByReference(int& ref) {
    ref = 100; // 修改引用的值，会影响原始变量
}

int* allocateMemory() {
    int* ptr = new int(200); // 分配内存并返回其地址
    return ptr;
}

int main() {
    int a = 1;
    modifyByReference(a); // a现在是100
    std::cout << "a after modifyByReference: " << a << "\n";

    int* b = allocateMemory();
    std::cout << "*b after allocateMemory: " << *b << "\n";
    delete b; // 记得释放内存

    return 0;
}
```

在这段代码中，`modifyByReference`函数接收一个整数引用作为参数并修改它。`allocateMemory`函数分配了一个整数的内存并返回指向它的指针。注意，我们使用了`new`来分配内存，因此需要用`delete`来释放它，防止内存泄漏。

这些示例说明了`*`和`&`在函数参数和返回类型中的不同用途，它们都是C++中非常强大的特性，允许函数更灵活地操作数据。