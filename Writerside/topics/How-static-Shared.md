# How static &amp; Shared

#### ��⾲̬���붯̬��Ϳ�ִ���ļ��Ĺ�ϵ��

��̬��Ͷ�̬���ڱ������ӽ׶����������ɵĿ�ִ���ļ���.exe���Ĺ�ϵ���£�

**��̬�⣨Static Library��**��
��̬��ͨ���� `.a`����Unix-likeϵͳ�У��� `.lib`����Windowsϵͳ�У�Ϊ��չ�����ڱ������ӽ׶Σ�����ʹ�þ�̬�⹹����ִ���ļ�ʱ���������Ὣ
��̬���б����õ������ж����ļ���`.o` �� `.obj`��**ֱ�Ӻϲ�**���������ɵĿ�ִ���ļ��С�����ζ�ţ�

- **�ߴ�����**����̬���еĴ��뱻�����ظ��Ƶ���ִ���ļ��У����¿�ִ���ļ��Ĵ�С���ӡ�
- **��������**����������������붼�Ѿ�Ƕ�뵽��ִ���ļ��ڲ������ʹ�þ�̬�⹹���ĳ���������ʱ����Ҫ�����ⲿ�ļ�������ֱ�Ӷ������С�
- **��������**������̬���и��£���Ҫ���±�������ʹ�øÿ�����п�ִ���ļ��������޷���ø��º�Ĺ��ܻ��޸���

**��̬�⣨Dynamic Library��**��
��̬��ͨ���� `.so`����Unix-likeϵͳ�У���Shared Object����`.dylib`����macOS�У��� `.dll`
����Windowsϵͳ�У���Dynamic Link Library��Ϊ��չ�����ڱ������ӽ׶Σ�����ʹ�ö�̬�⹹����ִ���ļ�ʱ��
���������Ὣ��̬�������ֱ�Ӻϲ�����ִ���ļ��С��෴�������¼�¶Զ�̬���������ϵ�����ڿ�ִ���ļ�������һЩ��������붯̬�⽻������Ϣ������ű������ȣ�������ζ�ţ�

- **�ߴ��С**����ִ���ļ�ֻ������������ָ��̬���������Ϣ�����������ʵ�ʴ��룬���ͨ����ʹ�þ�̬�⹹���Ŀ�ִ���ļ�С��
- **����ʱ����**����������ʱ��Ҫ��̬���ļ�������ϵͳ�ض�������·������`LD_LIBRARY_PATH`��`PATH`��������ָ����·�����С�
- ���ȱ������Ķ�̬���ļ��������޷����С�
- **���ڸ���**�����¶�̬��ʱ��ֻ���滻ϵͳ�еĿ��ļ������������ÿ�Ŀ�ִ���ļ����´�����ʱ���Զ�ʹ���°汾�Ŀ⣬
- �������±�����Щ��ִ���ļ���

�ܽ���˵����̬���ڱ���ʱ�ᱻ�����������غϲ�����ִ���ļ��У�����̬���򲻻ᡣ��̬�������������ʱ�ű����ص��ڴ沢���ִ���ļ�������
��ʹ��ʹ�ö�̬��ĳ�����н�С����������ڸ��µ��ŵ㣬��ͬʱҲ����������ʱ���������⡣ѡ��ʹ�þ�̬�⻹�Ƕ�̬��ͨ��ȡ������Ŀ�ľ��������粿�𻷾������²��ԡ�����Ҫ������ء�

#### ��ι��������Ӿ�̬��
```cmake
cmake_minimum_required(VERSION 3.10)  # ָ�����CMake�汾Ҫ��
project(MyProject VERSION 1.0 LANGUAGES CXX)  # ������Ŀ���ƺͰ汾

# ���ñ���ѡ���C++��׼�����漶��ȣ�
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED True)
set(CMAKE_CXX_FLAGS "-Wall -Wextra")

# ���徲̬��
add_library(MyStaticLib STATIC
   src/library_source1.cpp
   src/library_source2.cpp
   # ����Դ�ļ�...
)

# ���ͷ�ļ�Ŀ¼���Ա�����Ŀ�������ȷ�ҵ����ͷ�ļ�
target_include_directories(MyStaticLib
   PUBLIC
       ${CMAKE_CURRENT_SOURCE_DIR}/include
)

# ��������������������̬���������⣩������ʹ��target_link_libraries���
# target_link_libraries(MyStaticLib PRIVATE DependencyLib)

# �����ִ���ļ���������������������̬�⣩
add_executable(MyExecutable
   src/main.cpp
)

# ���Ӿ�̬�⵽��ִ���ļ�
target_link_libraries(MyExecutable
   PRIVATE MyStaticLib
)
```

����CMakeLists.txt�ļ��У�

- `add_library()`�������ڶ��徲̬�⣬`STATIC`�ؼ���ָ���˿�����Ϊ��̬�⡣Դ�ļ��б��ǿ���������Դ�����ļ�·����
- `target_include_directories()`���������˿�Ĺ���ͷ�ļ�Ŀ¼��ʹ�����������˿��Ŀ�꣨���ִ���ļ����ܹ��ҵ�����ȷ�������ͷ�ļ���
- `add_executable()`�����һ����ִ���ļ����г���Դ�����ļ���
- `target_link_libraries()`�����̬��`MyStaticLib`���ӵ���ִ���ļ�`MyExecutable`�������`PRIVATE`�ؼ��ֱ���������˽�еģ������������ϵ����`MyExecutable`��Ч�����ᴫ�ݸ�`MyExecutable`������������

#### ���ͨ��CMAKE���������Ӷ�̬��



���ڶ�̬�⣨Ҳ��Ϊ����⣩�����������뾲̬�����ƣ�����CMake��������Ҫ����Ӧ�ĵ�����������ʹ��CMake���������Ӷ�̬�����Ҫ���裺

```cmake
cmake_minimum_required(VERSION 3.10)
project(MyProject VERSION 1.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED True)
set(CMAKE_CXX_FLAGS "-Wall -Wextra")

# ���嶯̬��
add_library(MyDynamicLib SHARED
   src/library_source1.cpp
   src/library_source2.cpp
   # ����Դ�ļ�...
)

target_include_directories(MyDynamicLib
   PUBLIC
       ${CMAKE_CURRENT_SOURCE_DIR}/include
)

# ����������������ʹ��target_link_libraries���
# target_link_libraries(MyDynamicLib PRIVATE DependencyLib)

# �����ִ���ļ���������������������̬�⣩
add_executable(MyExecutable
   src/main.cpp
)

# ���Ӷ�̬�⵽��ִ���ļ�
target_link_libraries(MyExecutable
   PRIVATE MyDynamicLib
)
```

��Ҫ�仯���ڣ�

- ʹ��`add_library()`����ʱ����`STATIC`��Ϊ`SHARED`��ָ��Ҫ�������Ƕ�̬�⡣
- �������֣�������ͷ�ļ�Ŀ¼�����ӿ⵽��ִ���ļ����뾲̬�⹹����ͬ��

































