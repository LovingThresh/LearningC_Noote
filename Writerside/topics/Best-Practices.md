# Best Practices

��C++���򿪷��У��淶��Ŀ¼�ṹ���԰�����֯Դ���롢ͷ�ļ������Դ����Լ������������������Ŀ�Ŀ�ά���Ժ�Э��Ч�ʡ�������һ�ֳ����ġ��������ᵽ��Ŀ¼��CMakeLists.txt�ļ����ֵķ�����ͬʱҲ������һЩ�������õ�Ŀ¼��

```
MyProject/
������ CMakeLists.txt          # ��Ŀ����CMakeLists.txt�ļ�
������ build/                  # ����Ŀ¼�����û����������ڴ�ű��������
������ docs/                   # �ĵ�Ŀ¼�������Ŀ�ĵ���API�ĵ���
������ example/                # ʾ������Ŀ¼������ʹ�ø���Ŀ���ʾ������
��   ������ CMakeLists.txt      # ʾ��������Ŀ¼��CMakeLists.txt�ļ�
��   ������ ...
������ include/                # ͷ�ļ�Ŀ¼����Ź�����ͷ�ļ�
��   ������ MyProject           # ��Ŀ�����ռ��Ӧ����Ŀ¼��ʵ��ͷ�ļ����ڴ˴�
��       ������ ...
������ src/                    # Դ����Ŀ¼�������Ŀ����Ҫʵ�ִ���
��   ������ CMakeLists.txt      # Դ������Ŀ¼��CMakeLists.txt�ļ�
��   ������ module1/            # ģ��1Դ������Ŀ¼�����ж��ģ�飬�ɰ�����ӣ�
��   ��   ������ CMakeLists.txt  # ģ��1��Ŀ¼��CMakeLists.txt�ļ�
��   ��   ������ ...
��   ������ module2/            # ģ��2Դ������Ŀ¼
��   ��   ������ CMakeLists.txt  # ģ��2��Ŀ¼��CMakeLists.txt�ļ�
��   ��   ������ ...
��   ������ ...
������ test/                   # ���Դ���Ŀ¼�������Ŀ���������Ͳ��Կ������
��   ������ CMakeLists.txt      # ���Դ�����Ŀ¼��CMakeLists.txt�ļ�
��   ������ ...
������ third_party/            # ����������Ŀ¼�������Ŀ�������ⲿ��Դ���Ԥ�����
��   ������ CMakeLists.txt      # ������������Ŀ¼��CMakeLists.txt�ļ�����ѡ�����ڹ��������⣩
��   ������ ...
������ tools/                  # ����Ŀ¼�������Ŀ��صĸ������ߡ��ű��ȣ���ѡ��
    ������ ...
```

����ÿ������`CMakeLists.txt`�ļ���Ŀ¼��������Ӧ���ݸ�Ŀ¼�Ľ�ɫ������Ӧ�����á�������һЩ����������ʾ����

**����`CMakeLists.txt`**��

```cmake
cmake_minimum_required(VERSION 3.10)
project(MyProject VERSION 1.0 LANGUAGES CXX)

# ���ñ���ѡ�����·����ȫ������
...

# �����Ŀ¼
add_subdirectory(src)
add_subdirectory(example)
add_subdirectory(test)
add_subdirectory(third_party)

# ��װ���򣨿�ѡ��
install(TARGETS ...)
install(DIRECTORY ...)
```

**`src/CMakeLists.txt`**��

```cmake
# ��ӿ���ִ���ļ�
add_library(MyProject ...)
add_executable(MyExecutable ...)

# ����Դ�ļ���ͷ�ļ�������ѡ���
target_sources(MyProject ...)
target_include_directories(MyProject ...)
target_compile_options(MyProject ...)
...

# ������������ڲ�ģ���������
target_link_libraries(MyProject ...)
```

**`example/CMakeLists.txt`**��

```cmake
# ���ʾ����ִ���ļ�
add_executable(Example1 example1.cpp)
add_executable(Example2 example2.cpp)

# ����Դ�ļ���ͷ�ļ�������ѡ���
target_sources(Example1 ...)
target_sources(Example2 ...)
target_include_directories(Example1 ...)
target_include_directories(Example2 ...)
target_compile_options(Example1 ...)
target_compile_options(Example2 ...)

# ���ӵ���Ŀ��
target_link_libraries(Example1 MyProject)
target_link_libraries(Example2 MyProject)
```

**`test/CMakeLists.txt`**��

```cmake
# ��Ӳ��Կ�ܣ���Google Test��
find_package(GTest REQUIRED)
enable_testing()

# ��Ӳ�������
add_executable(MyProjectTests tests.cpp)
target_link_libraries(MyProjectTests GTest::GTest GTest::Main MyProject)

# ע�����
add_test(NAME MyProjectTests COMMAND MyProjectTests)
```

**`third_party/CMakeLists.txt`**�������Ҫ����Ŀ�й����������⣩��

```cmake
# ����ⲿ���CMake��Ŀ����ͨ��add_subdirectory()��
add_subdirectory(libA)
add_subdirectory(libB)

# ���ߣ�ֱ�����Ԥ������������Ϣ
include_directories(${THIRD_PARTY_DIR}/libC/include)
link_directories(${THIRD_PARTY_DIR}/libC/lib)
```

������һ����Թ淶��C++��ĿĿ¼�ṹ��CMakeLists.txt����ʾ����������Դ���롢ͷ�ļ���ʾ�������Ժ͵����������Ĺ���
ʵ����Ŀ�ɸ��ݾ���������е������ǵ��ڹ���ʱ����һ��������`build`Ŀ¼�����ڸ�Ŀ¼������CMake���ɹ���ϵͳ���Ա���Դ��Ŀ¼�����ࡣ