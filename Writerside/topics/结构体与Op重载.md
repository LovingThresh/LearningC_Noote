# 结构体与Op重载

## 应用方式
以下是关于C++中结构体（类）重载操作符以及其在排序、比较等场景中应用的整理笔记：

### 结构体与操作符重载简介

**结构体与类**：在C++中，结构体和类非常相似，主要区别在于默认的访问权限不同（结构体默认为公有，类默认为私有）。它们都可以包含数据成员、成员函数以及重载操作符。

**操作符重载**：操作符重载允许用户自定义操作符的行为，当操作符应用于自定义类型时，可以执行特定的操作。这使得代码更加自然、易于理解，同时保持了操作符的原有语义。

### 示例：重载操作符用于比较

假设有一个`Person`类，我们想比较其对象的`firstName`来决定排序顺序。

```c++
class Person {
public:
    std::string firstName;
    std::string lastName;
    // 构造函数等其他成员省略...
};

// 自定义比较结构体，重载<操作符
struct FirstNameCompare {
    bool operator()(const Person& a, const Person& b) const {
        return a.firstName < b.firstName; // 比较firstName
    }
};
```

### 应用场景：排序

在`std::ranges::sort`或`std::sort`中使用自定义比较逻辑，比如上面定义的`FirstNameCompare`。

```c++
std::vector<Person> people; // 假设people已经初始化
std::ranges::sort(people, FirstNameCompare{}); // struct{} -> 实例
```

### 操作符重载与其他场景

- **==, !=, >, >=, <, <=**：除了`<`之外，其他比较操作符也可以重载，用于实现等值判断、大于、小于等于等逻辑。
- **+, -, *, /, %**：重载算术操作符，用于自定义类型间的数学运算。
- **<<, >>**：重载流插入和提取操作符，便于自定义类型与IO流的交互。

### 优势与注意事项

**优势**：
- 提高代码的可读性和自解释性，使得代码更接近自然语言表述。
- 提供了一致的接口，便于维护和扩展。
- 增强了泛型编程的能力，使得自定义类型能更好地融入标准库和现有的代码框架中。

**注意事项**：
- 保持操作符原有的语义，避免造成混淆。
- 注意操作符的优先级和结合性，确保符合预期。
- 确保重载操作符的实现是高效的，避免不必要的开销。

### 总结

结构体（或类）通过操作符重载，可以为自定义类型提供强大的比较、运算能力，使得这些类型能够无缝融入到C++的标准库函数和算法中，如排序、查找等。正确且合理地使用操作符重载，可以显著提升代码的表达能力和可维护性，但同时需要谨慎设计，确保操作符的行为符合预期，不会引起误解或误用。