# How static &amp; Shared

#### 理解静态库与动态库和可执行文件的关系：

静态库和动态库在编译链接阶段与最终生成的可执行文件（.exe）的关系如下：

**静态库（Static Library）**：
静态库通常以 `.a`（在Unix-like系统中）或 `.lib`（在Windows系统中）为扩展名。在编译链接阶段，当您使用静态库构建可执行文件时，链接器会将
静态库中被引用到的所有对象文件（`.o` 或 `.obj`）**直接合并**到最终生成的可执行文件中。这意味着：

- **尺寸增大**：静态库中的代码被完整地复制到可执行文件中，导致可执行文件的大小增加。
- **独立运行**：由于所有所需代码都已经嵌入到可执行文件内部，因此使用静态库构建的程序在运行时不需要依赖外部文件，可以直接独立运行。
- **更新困难**：若静态库有更新，需要重新编译链接使用该库的所有可执行文件，否则无法获得更新后的功能或修复。

**动态库（Dynamic Library）**：
动态库通常以 `.so`（在Unix-like系统中，即Shared Object）、`.dylib`（在macOS中）或 `.dll`
（在Windows系统中，即Dynamic Link Library）为扩展名。在编译链接阶段，当您使用动态库构建可执行文件时，
链接器不会将动态库的内容直接合并到可执行文件中。相反，它会记录下对动态库的依赖关系，并在可执行文件中留下一些描述如何与动态库交互的信息（如符号表、导入表等）。这意味着：

- **尺寸较小**：可执行文件只包含自身代码和指向动态库的引用信息，不包含库的实际代码，因此通常比使用静态库构建的可执行文件小。
- **运行时依赖**：程序运行时需要动态库文件存在于系统特定的搜索路径（如`LD_LIBRARY_PATH`或`PATH`环境变量指定的路径）中。
- 如果缺少所需的动态库文件，程序将无法运行。
- **易于更新**：更新动态库时，只需替换系统中的库文件，所有依赖该库的可执行文件在下次运行时会自动使用新版本的库，
- 无需重新编译这些可执行文件。

总结来说，静态库在编译时会被链接器完整地合并到可执行文件中，而动态库则不会。动态库的内容在运行时才被加载到内存并与可执行文件交互，
这使得使用动态库的程序具有较小的体积、易于更新的优点，但同时也带来了运行时依赖的问题。选择使用静态库还是动态库通常取决于项目的具体需求，如部署环境、更新策略、性能要求等因素。

#### 如何构建并链接静态库
```cmake
cmake_minimum_required(VERSION 3.10)  # 指定最低CMake版本要求
project(MyProject VERSION 1.0 LANGUAGES CXX)  # 定义项目名称和版本

# 设置编译选项（如C++标准、警告级别等）
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED True)
set(CMAKE_CXX_FLAGS "-Wall -Wextra")

# 定义静态库
add_library(MyStaticLib STATIC
   src/library_source1.cpp
   src/library_source2.cpp
   # 更多源文件...
)

# 添加头文件目录，以便其他目标可以正确找到库的头文件
target_include_directories(MyStaticLib
   PUBLIC
       ${CMAKE_CURRENT_SOURCE_DIR}/include
)

# 如果库有依赖项（如其他静态库或第三方库），可以使用target_link_libraries添加
# target_link_libraries(MyStaticLib PRIVATE DependencyLib)

# 定义可执行文件（假设它依赖于上述静态库）
add_executable(MyExecutable
   src/main.cpp
)

# 链接静态库到可执行文件
target_link_libraries(MyExecutable
   PRIVATE MyStaticLib
)
```

上述CMakeLists.txt文件中：

- `add_library()`命令用于定义静态库，`STATIC`关键字指定了库类型为静态库。源文件列表是库编译所需的源代码文件路径。
- `target_include_directories()`命令设置了库的公共头文件目录，使得其他依赖此库的目标（如可执行文件）能够找到并正确包含库的头文件。
- `add_executable()`命令创建一个可执行文件，列出其源代码文件。
- `target_link_libraries()`命令将静态库`MyStaticLib`链接到可执行文件`MyExecutable`。这里的`PRIVATE`关键字表明链接是私有的，即库的依赖关系仅对`MyExecutable`有效，不会传递给`MyExecutable`的下游依赖。

#### 如何通过CMAKE构建并链接动态库



对于动态库（也称为共享库），构建过程与静态库类似，但在CMake配置中需要做相应的调整。以下是使用CMake构建并链接动态库的主要步骤：

```cmake
cmake_minimum_required(VERSION 3.10)
project(MyProject VERSION 1.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED True)
set(CMAKE_CXX_FLAGS "-Wall -Wextra")

# 定义动态库
add_library(MyDynamicLib SHARED
   src/library_source1.cpp
   src/library_source2.cpp
   # 更多源文件...
)

target_include_directories(MyDynamicLib
   PUBLIC
       ${CMAKE_CURRENT_SOURCE_DIR}/include
)

# 如果库有依赖项，可以使用target_link_libraries添加
# target_link_libraries(MyDynamicLib PRIVATE DependencyLib)

# 定义可执行文件（假设它依赖于上述动态库）
add_executable(MyExecutable
   src/main.cpp
)

# 链接动态库到可执行文件
target_link_libraries(MyExecutable
   PRIVATE MyDynamicLib
)
```

主要变化在于：

- 使用`add_library()`命令时，将`STATIC`改为`SHARED`，指明要创建的是动态库。
- 其他部分（如设置头文件目录和链接库到可执行文件）与静态库构建相同。

































