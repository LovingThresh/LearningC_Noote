# How include

## ���ֱ�ӵ���ͷ�ļ�
��CMake�й���ͷ�ļ���������Ҫ�漰�������棺

1. **ָ��ͷ�ļ�����·��**��ȷ���������ڱ���Դ�ļ�ʱ�ܹ��ҵ������ͷ�ļ���
2. **��Դ������ʹ��`#include`ָ��**����дԴ����ʱ��ȷ����ͷ�ļ���

**1. ָ��ͷ�ļ�����·��**

CMake�ṩ��`include_directories()`������ָ��������Ӧ������ͷ�ļ���Ŀ¼��������CMakeLists.txt��ʹ���������ʱ����ָ����Ŀ¼�ᱻ��ӵ���������� `-I` �����У�ʹ�ñ�����֪��ȥ�������`#include`ָ���е�ͷ�ļ���

```cmake
include_directories(include)  # ����ͷ�ļ�λ����Ŀ�ġ�include����Ŀ¼
```

���ߣ��������Ҫָ�����ͷ�ļ�Ŀ¼������ͬʱ�г���

```cmake
include_directories(
    include
    third_party/libA/include
    third_party/libB/include
)
```

������ʹ��`PUBLIC`��`PRIVATE`��`INTERFACE`�ؼ�����ָ����ЩĿ¼����Ե�ǰĿ�ꡢ������������Ŀ�껹�ǽ���ʹ������Ŀ��ɼ���

```cmake
target_include_directories(MyTarget
    PUBLIC include  # ����������MyTarget��Ŀ�깫����Ŀ¼
    PRIVATE src/private_headers  # ֻ��MyTarget����ɼ�
)
```

**2. ��Դ������ʹ��`#include`ָ��**

��Դ�����У�ʹ�ñ�׼��C/C++ `#include` ָ��������ͷ�ļ���������ʹ�����·�������·�����������Ŀ��Ŀ¼�����ϵͳ�Ĺ���Ŀ¼�����Ƽ�ʹ�����·����������ѭ��Ŀ���õ���֯�ṹ��

```c++
#include "myheader.h"  // ������ǰԴ�ļ�����Ŀ¼���丸Ŀ¼�µ�myheader.h
#include "subdir/moreheaders.h"  // ������ǰĿ¼��subdir��Ŀ¼�е�moreheaders.h

// ���ߣ�����ָ����ͷ�ļ�����·���µ�ͷ�ļ�
#include <external_lib/header.h>  // ����ͨ��include_directories��ӵ��ⲿ��ͷ�ļ�
```

**���ʵ��**��

- ������ʹ��`target_include_directories()`����ȫ�ֵ�`include_directories()`����Ϊ���ܸ��õ�ά��Ŀ����������ϵ�������ڹ���ϵͳ��ģ�黯�����á�
- ����Ŀ�����������ִ���ļ�ʱ��Ϊÿ��Ŀ�굥��ָ����������ͷ�ļ�Ŀ¼���������Ա��ⲻ��Ҫ��ͷ�ļ�����·����Ⱦ��
- ʹ��`PUBLIC`��`PRIVATE`��`INTERFACE`����ͷ�ļ��Ŀɼ��ԣ������ڹ����ӿ������Ŀ�͹����ӵ�������ϵ��

ͨ���������CMake���ú�Դ�����е�`#include`ָ���������CMake��Ŀ��˳�������úͱ���ͷ�ļ��ˡ�