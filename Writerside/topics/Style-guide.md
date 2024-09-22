# Style guide

### 鸿鹄云图编码规范及代码案例

#### 1. 【文件规范】
- **字符编码**：UTF-8

- **文件命名**：
	- 规则：以项目大写前缀开头，之后每个单词首字母大写，禁止下划线和空格。`main.cpp`除外。
	- 案例：
    ```C++
    // 文件名：ProjectNameMain.cpp
    ```

- **目录命名**：
	- 规则：每个单词首字母大写，禁止空格。
	- 案例：
    ```
    ProjectFolder/
        Source/
        Include/
    ```

- **解决方案命名**：
	- 规则：每个单词首字母大写，禁止空格。
	- 案例：
    ```C++
    SolutionName.sln
    ```

- **头文件命名**：
	- 规则：以.h为后缀，以项目或模块的大写前缀开头，之后每个单词首字母大写。头文件首行必须是`#pragma once`。
	- 案例：
    ```C++
    // 文件名：ProjectModuleName.h
    #pragma once
    ```

- **单元文件**：
	- 规则：以.cpp为后缀，与对应头文件保持相同名称。
	- 案例：
    ```C++
    // 文件名：ProjectModuleName.cpp
    ```

#### 2. 【排版风格】
- **换行**：
	- 规则：一行代码只定义一个变量，写一条语句。控制结构如`if`、`for`等必须使用`{}`包裹，即使执行语句少于两条。
	- 案例：
    ```C++
    int a = 10;
    int b = 20;
	
    if (a > b) 
    {
        std::cout << "a is greater";
    }
    ```

- **缩进**：
	- 规则：使用4个空格，`{`和`}`独占一行，且与引用它们的语句对齐。
	- 案例：
    ```C++
    void ExampleFunction() 
    {
        for (int i = 0; i < 10; ++i) 
        {
            std::cout << i;
        }
    }
    ```

- **空行**：
	- 规则：函数、枚举、类、宏声明后应加空行。
	- 案例：
    ```C++
    class MyClass 
    {
    public:
        void Function1();

        void Function2();
    };
    ```

- **空格**：
	- 规则：符号前后应加空格，以下情况除外：
		- `.`、`->`、`[]`、`::`
		- 函数调用的`()`、引号前后
	- 案例：
    ```C++
    int* ptr = &a; // 变量类型前的 * 不加空格
    if (a == b) {
        std::cout << "Equal";
    }
    ```

#### 3. 【命名规范】
- **缩写**：
	- 规则：避免缩写，使用通用缩写。
	- 案例：
    ```C++
    int numEvents;
    double avgSpeed;
    ```

- **类命名**：
	- 规则：以项目或模块的大写前缀开头，之后每个单词首字母大写。
	- 案例：
    ```C++
    class ProjectUserManager;
    ```

- **接口命名**：
	- 规则：类名前加`I`，必须包含虚析构函数。
	- 案例：
    ```C++
    class IUserManager 
    {
    public:
        virtual ~IUserManager() = default;
    };
    ```

- **方法命名**：
	- 规则：方法名以小写开头，之后每个单词首字母大写。
	- 案例：
    ```C++
    void setName(const std::string& name);
    std::string getName() const;
    ```

- **变量和常量命名**：
	- 规则：变量名以类型前缀开头，范围前缀适用于全局常量、静态变量等。
	- 案例：
    ```C++
    int nEventFlag;
    bool bIsRunning;
    static int s_instanceCount;
    ```

#### 4. 【过程控制】
- **禁止使用`goto`**。
	- 案例：
    ```C++
    // 不推荐
    // goto label;
    ```

#### 5. 【类型转换】
- **规则**：基础类型转换可使用强制转换，其他类型转换需使用`static_cast`或`dynamic_cast`。
	- 案例：
    ```C++
    int a = 10;
    double d = static_cast<double>(a);
    ```

#### 6. 【注释规范】
- **单元注释模板**：
    ```C++
    /*!
     * @brief 简述功能
     *
     * @author
     * @date 2024-09-22
     * @copyright (c) 2013-2024 Honghu Yuntu Corporation
     */
    ```

- **函数注释模板**：
    ```C++
    /*!
     * @brief 获取用户名称
     * @param userId 用户ID
     * @return 用户名称字符串
     * @author
     * @date 2024-09-22
     */
    std::string getUserName(int userId);
    ```