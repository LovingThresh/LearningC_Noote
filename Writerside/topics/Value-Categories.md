# Value Categories

## 引子
```C++
void emplace_back(std::thread&& t)
{
	m_oPool.emplace_back(std::move(t));
}
```

这段代码定义了一个名为 `emplace_back` 的函数，用于将一个 `std::thread` 对象添加到一个线程池（或某个容器）中。它的具体作用可以分解如下：

### 1. `void emplace_back(std::thread&& t)`
- **`void`**：表示该函数没有返回值。
- **`emplace_back`**：这是函数名，表示将元素放置在某个容器的末尾。
- **`std::thread&& t`**：表示一个右值引用参数，这意味着该函数期望一个临时的 `std::thread` 对象（右值），或者通过 `std::move` 转换后的 `std::thread`。

### 2. `m_oPool.emplace_back(std::move(t));`
- **`m_oPool`**：这是一个类成员变量，通常是某种容器（例如 `std::vector<std::thread>`），它用于存储线程对象。
- **`emplace_back`**：这是 `std::vector` 的成员函数，它的作用是在容器的末尾直接构造对象。与 `push_back` 不同，`emplace_back` 允许在容器内部直接构造对象，避免不必要的拷贝或移动操作。
- **`std::move(t)`**：将传入的线程对象 `t` 转换为一个右值，以便将其移动到 `m_oPool` 容器中。移动语义可以避免线程对象的拷贝，确保高效地转移资源（例如线程的控制权）。

### 作用总结
这个函数的作用是：将一个右值引用形式的 `std::thread` 对象添加到 `m_oPool` 容器中，并且通过移动语义（`std::move`）来避免不必要的拷贝。这通常用于线程池的实现中，将新创建的线程存储在池中以便后续管理和使用。

