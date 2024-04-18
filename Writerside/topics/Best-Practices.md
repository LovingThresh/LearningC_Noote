# Best Practices

在C++程序开发中，规范的目录结构可以帮助组织源代码、头文件、测试代码以及第三方依赖，提高项目的可维护性和协作效率。以下是一种常见的、包含您提到的目录和CMakeLists.txt文件布局的方案，同时也补充了一些其他常用的目录：

```
MyProject/
├── CMakeLists.txt          # 项目顶层CMakeLists.txt文件
├── build/                  # 构建目录（由用户创建，用于存放编译输出）
├── docs/                   # 文档目录，存放项目文档、API文档等
├── example/                # 示例代码目录，包含使用该项目库的示例程序
│   ├── CMakeLists.txt      # 示例代码子目录的CMakeLists.txt文件
│   └── ...
├── include/                # 头文件目录，存放公共的头文件
│   └── MyProject           # 项目命名空间对应的子目录，实际头文件放在此处
│       └── ...
├── src/                    # 源代码目录，存放项目的主要实现代码
│   ├── CMakeLists.txt      # 源代码子目录的CMakeLists.txt文件
│   ├── module1/            # 模块1源代码子目录（如有多个模块，可按需添加）
│   │   ├── CMakeLists.txt  # 模块1子目录的CMakeLists.txt文件
│   │   └── ...
│   ├── module2/            # 模块2源代码子目录
│   │   ├── CMakeLists.txt  # 模块2子目录的CMakeLists.txt文件
│   │   └── ...
│   └── ...
├── test/                   # 测试代码目录，存放项目测试用例和测试框架配置
│   ├── CMakeLists.txt      # 测试代码子目录的CMakeLists.txt文件
│   └── ...
├── third_party/            # 第三方依赖目录，存放项目依赖的外部库源码或预编译库
│   ├── CMakeLists.txt      # 第三方依赖子目录的CMakeLists.txt文件（可选，用于构建依赖库）
│   └── ...
└── tools/                  # 工具目录，存放项目相关的辅助工具、脚本等（可选）
    └── ...
```

对于每个包含`CMakeLists.txt`文件的目录，其内容应根据该目录的角色进行相应的配置。以下是一些基本的配置示例：

**顶层`CMakeLists.txt`**：

```cmake
cmake_minimum_required(VERSION 3.10)
project(MyProject VERSION 1.0 LANGUAGES CXX)

# 设置编译选项、搜索路径等全局配置
...

# 添加子目录
add_subdirectory(src)
add_subdirectory(example)
add_subdirectory(test)
add_subdirectory(third_party)

# 安装规则（可选）
install(TARGETS ...)
install(DIRECTORY ...)
```

**`src/CMakeLists.txt`**：

```cmake
# 添加库或可执行文件
add_library(MyProject ...)
add_executable(MyExecutable ...)

# 设置源文件、头文件、编译选项等
target_sources(MyProject ...)
target_include_directories(MyProject ...)
target_compile_options(MyProject ...)
...

# 添加依赖（如内部模块间依赖）
target_link_libraries(MyProject ...)
```

**`example/CMakeLists.txt`**：

```cmake
# 添加示例可执行文件
add_executable(Example1 example1.cpp)
add_executable(Example2 example2.cpp)

# 设置源文件、头文件、编译选项等
target_sources(Example1 ...)
target_sources(Example2 ...)
target_include_directories(Example1 ...)
target_include_directories(Example2 ...)
target_compile_options(Example1 ...)
target_compile_options(Example2 ...)

# 链接到项目库
target_link_libraries(Example1 MyProject)
target_link_libraries(Example2 MyProject)
```

**`test/CMakeLists.txt`**：

```cmake
# 添加测试框架（如Google Test）
find_package(GTest REQUIRED)
enable_testing()

# 添加测试用例
add_executable(MyProjectTests tests.cpp)
target_link_libraries(MyProjectTests GTest::GTest GTest::Main MyProject)

# 注册测试
add_test(NAME MyProjectTests COMMAND MyProjectTests)
```

**`third_party/CMakeLists.txt`**（如果需要在项目中构建第三方库）：

```cmake
# 添加外部库的CMake项目（如通过add_subdirectory()）
add_subdirectory(libA)
add_subdirectory(libB)

# 或者，直接添加预编译库的链接信息
include_directories(${THIRD_PARTY_DIR}/libC/include)
link_directories(${THIRD_PARTY_DIR}/libC/lib)
```

以上是一个相对规范的C++项目目录结构及CMakeLists.txt设置示例，涵盖了源代码、头文件、示例、测试和第三方依赖的管理。
实际项目可根据具体需求进行调整。记得在构建时创建一个独立的`build`目录，并在该目录下运行CMake生成构建系统，以保持源码目录的整洁。