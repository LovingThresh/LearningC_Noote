# How include

## 如何直接导入头文件
在CMake中管理头文件的引用主要涉及两个方面：

1. **指定头文件搜索路径**：确保编译器在编译源文件时能够找到所需的头文件。
2. **在源代码中使用`#include`指令**：编写源代码时正确引用头文件。

**1. 指定头文件搜索路径**

CMake提供了`include_directories()`命令来指定编译器应该搜索头文件的目录。当您在CMakeLists.txt中使用这个命令时，所指定的目录会被添加到编译命令的 `-I` 参数中，使得编译器知道去哪里查找`#include`指令中的头文件。

```cmake
include_directories(include)  # 假设头文件位于项目的“include”子目录
```

或者，如果您需要指定多个头文件目录，可以同时列出：

```cmake
include_directories(
    include
    third_party/libA/include
    third_party/libB/include
)
```

还可以使用`PUBLIC`、`PRIVATE`或`INTERFACE`关键字来指定这些目录是针对当前目标、仅对依赖它的目标还是仅对使用它的目标可见：

```cmake
target_include_directories(MyTarget
    PUBLIC include  # 对所有依赖MyTarget的目标公开此目录
    PRIVATE src/private_headers  # 只对MyTarget自身可见
)
```

**2. 在源代码中使用`#include`指令**

在源代码中，使用标准的C/C++ `#include` 指令来引入头文件。您可以使用相对路径或绝对路径（相对于项目根目录或编译系统的工作目录）。推荐使用相对路径，并且遵循项目良好的组织结构。

```c++
#include "myheader.h"  // 包含当前源文件所在目录或其父目录下的myheader.h
#include "subdir/moreheaders.h"  // 包含当前目录下subdir子目录中的moreheaders.h

// 或者，包含指定的头文件搜索路径下的头文件
#include <external_lib/header.h>  // 包含通过include_directories添加的外部库头文件
```

**最佳实践**：

- 尽可能使用`target_include_directories()`而非全局的`include_directories()`，因为它能更好地维护目标间的依赖关系，有助于构建系统的模块化和重用。
- 当项目包含多个库或可执行文件时，为每个目标单独指定其依赖的头文件目录，这样可以避免不必要的头文件搜索路径污染。
- 使用`PUBLIC`、`PRIVATE`和`INTERFACE`区分头文件的可见性，有助于构建接口清晰的库和管理复杂的依赖关系。

通过结合上述CMake配置和源代码中的`#include`指令，您就能在CMake项目中顺利地引用和编译头文件了。