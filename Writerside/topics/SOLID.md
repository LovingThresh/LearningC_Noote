# SOLID
---

## Ŀ¼

1. [SOLID ԭ����](#introduction)
2. [��һְ��ԭ�� (Single Responsibility Principle, SRP)](#srp)
3. [���ŷ��ԭ�� (Open/Closed Principle, OCP)](#ocp)
4. [�����滻ԭ�� (Liskov Substitution Principle, LSP)](#lsp)
5. [�ӿڸ���ԭ�� (Interface Segregation Principle, ISP)](#isp)
6. [��������ԭ�� (Dependency Inversion Principle, DIP)](#dip)
7. [�ܽ�](#conclusion)

---

## 1. SOLID ԭ���� {#introduction}

SOLID ���������������ԭ�������ĸ���Դʣ�

- **S** - ��һְ��ԭ�� (Single Responsibility Principle, SRP)
- **O** - ���ŷ��ԭ�� (Open/Closed Principle, OCP)
- **L** - �����滻ԭ�� (Liskov Substitution Principle, LSP)
- **I** - �ӿڸ���ԭ�� (Interface Segregation Principle, ISP)
- **D** - ��������ԭ�� (Dependency Inversion Principle, DIP)

��Щԭ��ּ����ߴ���Ŀ�ά���ԡ�����չ�ԺͿɸ����ԣ�ʹ���ϵͳ���ߵ��ԣ��ܹ���Ӧ���ϱ仯������

---

## 2. ��һְ��ԭ�� (Single Responsibility Principle, SRP) {#srp}

### **����**
һ����Ӧ��ֻ��һ���������仯��ԭ��Ҳ����˵��һ����Ӧ�ý���һ��ְ��

### **�������**
��һְ��ԭ��ǿ��ÿ�����ģ��Ӧרע��һ���ض��Ĺ��ܻ�ְ�����ַ���ȷ������ĸ��ھۺ͵���ϣ�ʹ�ô������������ά����

### **�м�ʾ��**

������һ�� `User` �࣬ͬʱ�����û����ݺ���־��¼��

```C++
class User {
public:
    void setName(const std::string& name) { this->name = name; }
    std::string getName() const { return name; }

    void logUserAction(const std::string& action) {
        // ��־��¼�߼�
    }

private:
    std::string name;
};
```

**Υ�� SRP��**
`User` ��ȹ����û����ݣ��ָ�����־��¼��������ְ��Ӧ���롣����Ҫ�޸���־��¼�߼������ܻ�Ӱ���û����ݹ���

### **�ع����ʾ����ѭ SRP** {id="srp_function"}

```C++
class User {
public:
    void setName(const std::string& name) { this->name = name; }
    std::string getName() const { return name; }

private:
    std::string name;
};

class UserLogger {
public:
    void logAction(const User& user, const std::string& action) {
        // �ض����û���������־��¼�߼�
    }
};
```

**��ѭ SRP��**
���ڣ�`User` ��������û����ݣ��� `UserLogger` ��ר�Ŵ�����־��¼�������޸���־�߼�����Ӱ���û����ݹ���

### **�߼�����**

ʵʩ SRP ���ܻᵼ�����������ӣ��⿴�Ʒ�������ͨ�����ģʽ�� **�۲���ģʽ��Observer Pattern��** �� **�н���ģʽ��Mediator Pattern��**��������Ч������֮��Ľ��������⣬ģ�黯��ƺ��������ܹ���Service-Oriented Architecture, SOA��Ҳ�ܽ�һ��ǿ�� SRP��ͨ������ͬ���ܻ���Ϊ������ģ��������ʵ��ְ��ķ��롣

---

## 3. ���ŷ��ԭ�� (Open/Closed Principle, OCP) {#ocp}

### **����**
���ʵ�壨�ࡢģ�顢�����ȣ�Ӧ��**����չ����**��**���޸ķ��**��

### **�������**
���ŷ��ԭ������ڲ��޸����д��������£�ͨ����չ������¹��ܡ���ͨ��ͨ�����󡢼̳кͶ�̬����ʵ�֡�

### **�м�ʾ��**

������һ�� `Shape` ���νṹ��

```C++
enum class ShapeType { Circle, Rectangle };

class Shape {
public:
    virtual ShapeType getType() const = 0;
};

class Circle : public Shape {
public:
    ShapeType getType() const override { return ShapeType::Circle; }
    double getRadius() const { return radius; }

private:
    double radius;
};

class Rectangle : public Shape {
public:
    ShapeType getType() const override { return ShapeType::Rectangle; }
    double getWidth() const { return width; }
    double getHeight() const { return height; }

private:
    double width;
    double height;
};

// ��������ĺ���
double calculateArea(const Shape& shape) {
    switch (shape.getType()) {
        case ShapeType::Circle:
            // ǿ������ת��Ϊ Circle ���������
            break;
        case ShapeType::Rectangle:
            // ǿ������ת��Ϊ Rectangle ���������
            break;
    }
    return 0.0;
}
```

**Υ�� OCP��**
����µ���״���� `Triangle`����Ҫ�޸� `calculateArea` ������Υ���˿��ŷ��ԭ��

### **�ع����ʾ����ѭ OCP** {id="ocp_function"}

```C++
class Shape {
public:
    virtual double calculateArea() const = 0;
};

class Circle : public Shape {
public:
    double calculateArea() const override { return 3.14159 * radius * radius; }

private:
    double radius;
};

class Rectangle : public Shape {
public:
    double calculateArea() const override { return width * height; }

private:
    double width;
    double height;
};

// ����������ĺ���
double calculateTotalArea(const std::vector<std::shared_ptr<Shape>>& shapes) {
    double total = 0.0;
    for (const auto& shape : shapes) {
        total += shape->calculateArea();
    }
    return total;
}
```

**��ѭ OCP��**
���ڣ�����µ���״�ࣨ�� `Triangle`��ֻ��̳��� `Shape` ��ʵ�� `calculateArea` �������������޸� `calculateTotalArea` ������

### **�߼�����**

�ڸ���ϵͳ�У�OCP ��ʵ�ֳ���� **����ע�루Dependency Injection��** �� **���󹤳�ģʽ��Abstract Factory Pattern��**���Զ�̬��������Ϊ�����޸����д��롣���� **����ʽ��̷�ʽ** �� **����ģʽ��Strategy Pattern��**�����Խ�һ������ OCP ��Ӧ�ã�ͨ����װ�ɱ���Ϊ����ǿ����Ŀ���չ�ԡ�

---

## 4. �����滻ԭ�� (Liskov Substitution Principle, LSP) {#lsp}

### **����**
�����ͱ����ܹ��滻���ǵĻ����Ͷ����ᵼ�³������

### **�������**
�����滻ԭ��ȷ���̳й�ϵ�е������ܹ��޷��滻���࣬���ֳ������ȷ�Ժ͹��ܵ�һ���ԡ�

### **�м�ʾ��**

����һ�� `Bird` ��������� `Penguin`��

```C++
class Bird {
public:
    virtual void fly() = 0;
};

class Sparrow : public Bird {
public:
    void fly() override {
        // ��ȸ���е��߼�
    }
};

class Penguin : public Bird {
public:
    void fly() override {
        // ����޷����У������׳��쳣��ʲô������
    }
};
```

**Υ�� LSP��**
����޷����У��������Ҫ������Ϊ�����������滻 `Bird` Ϊ `Penguin` �ᵼ���߼�����Υ���������滻ԭ��

### **�ع����ʾ����ѭ LSP** {id="lsp_function"}

```C++
class Bird {
public:
    virtual void move() = 0;
};

class FlyingBird : public Bird {
public:
    void move() override { fly(); }

    virtual void fly() = 0;
};

class NonFlyingBird : public Bird {
public:
    void move() override { swim(); }

    virtual void swim() = 0;
};

class Sparrow : public FlyingBird {
public:
    void fly() override {
        // ��ȸ���е��߼�
    }
};

class Penguin : public NonFlyingBird {
public:
    void swim() override {
        // �����Ӿ���߼�
    }
};
```

**��ѭ LSP��**
���ڣ�`Sparrow` �� `Penguin` �ֱ�̳����ʵ��Ļ��ࣨ`FlyingBird` �� `NonFlyingBird`����ȷ�����ǿ����ڸ��Ե�����������ȷ�滻�����������߼�����

### **�߼�����**

LSP �� **�����Լ��Design by Contract��** ������أ�Ҫ��������ʵ��ʱ������Ȼ�����ϸ��ǰ������������ɵĺ����������߼�Ӧ���У���� **�ӿڸ���ԭ��ISP��** �� **��������ԭ��DIP��**������ͨ����ϸ���ȵĽӿںͳ����Σ�ȷ��������滻�Ժ�ϵͳ�������ȶ��ԡ�

---

## 5. �ӿڸ���ԭ�� (Interface Segregation Principle, ISP) {#isp}

### **����**
�ͻ��˲�Ӧ�ñ����������ǲ�ʹ�õĽӿڡ�Ӧ���Ӵ�Ľӿڲ��Ϊ�������С�ӿڡ�

### **�������**
�ӿڸ���ԭ��������ϸ���ȵĽӿڣ�ÿ���ӿ�ֻ�����ض��Ĺ��ܣ����ⵥһ�ӿڳе�����ְ������ϵͳ������ԺͿ�ά���ԡ�

### **�м�ʾ��**

����һ�� `IWorker` �ӿڣ�

```C++
class IWorker {
public:
    virtual void work() = 0;
    virtual void eat() = 0;
};
```

**Υ�� ISP��**
��������ʵ�� `IWorker` �ӿ�ʱ������Ҫ `eat()` ���������½ӿ��а�������Ҫ�ķ�����

### **�ع����ʾ����ѭ ISP** {id="isp_function"}

```C++
class IWorkable {
public:
    virtual void work() = 0;
};

class IFeedable {
public:
    virtual void eat() = 0;
};

class HumanWorker : public IWorkable, public IFeedable {
public:
    void work() override {
        // ���๤���߼�
    }

    void eat() override {
        // �����ʳ�߼�
    }
};

class RobotWorker : public IWorkable {
public:
    void work() override {
        // �����˹����߼�
    }
};
```

**��ѭ ISP��**
���ڣ�`RobotWorker` ֻʵ�� `IWorkable` �ӿڣ������˲���Ҫ�� `eat()` �����������˽ӿڵ�רһ�Ժ�����ԡ�

### **�߼�����**

�ڸ���ϵͳ�У�ISP ����ͨ�� **��ɫ�ӿڣ�Role Interfaces��** ʵ�֣����ӿڰ���ɫ���ܲ�֡����⣬��� **��ϸ��ã�Composition Over Inheritance��**��ͨ����϶��ϸ���Ƚӿ����������Ӷ��󣬱��ⲻ��Ҫ�ļ̳й�ϵ��ʹ�� **�������** �� **ģ����** Ҳ����Чʵ�ֽӿڸ��룬����ϵͳ�Ŀ���չ�Ժ�����ԡ�

---

## 6. ��������ԭ�� (Dependency Inversion Principle, DIP) {#dip}

### **����**
�߲�ģ�鲻Ӧ�������Ͳ�ģ�飬���߶�Ӧ�������ڳ��󡣳���Ӧ��������ϸ�ڣ�ϸ��Ӧ�������ڳ���

### **�������**
��������ԭ��ּ�ڼ���ģ��֮�����ϣ�ͨ������������ӿڻ�����ࣩ���Ǿ���ʵ�֣�����ϵͳ������ԺͿɲ����ԡ�

### **�м�ʾ��**

����һ�� `LightBulb` ���һ�� `Switch` �ࣺ

```C++
class LightBulb {
public:
    void turnOn() { /* �򿪵��� */ }
    void turnOff() { /* �رյ��� */ }
};

class Switch {
public:
    void operate() {
        bulb.turnOn();
    }

private:
    LightBulb bulb;
};
```

**Υ�� DIP��**
`Switch` ��ֱ�������ھ���� `LightBulb` �࣬���� `Switch` ������Ҫ�滻 `LightBulb` ʱ�����޸ġ�

### **�ع����ʾ����ѭ DIP** {id="dip_function"}

```C++
class IDevice {
public:
    virtual void turnOn() = 0;
    virtual void turnOff() = 0;
};

class LightBulb : public IDevice {
public:
    void turnOn() override { /* �򿪵��� */ }
    void turnOff() override { /* �رյ��� */ }
};

class Fan : public IDevice {
public:
    void turnOn() override { /* �򿪷��� */ }
    void turnOff() override { /* �رշ��� */ }
};

class Switch {
public:
    Switch(IDevice& device) : device(device) {}

    void operate() {
        device.turnOn();
    }

private:
    IDevice& device;
};
```

**��ѭ DIP��**
`Switch` ������������ `IDevice` ����ӿڣ��������κ�ʵ���� `IDevice` ���豸���� `LightBulb` �� `Fan`�����ʹ�ã�������ϵͳ������ԺͿ���չ�ԡ�

### **�߼�����**

ʵʩ DIP ͨ����� **����ע�루Dependency Injection��** �������繹�캯��ע�롢Setter ע���ӿ�ע�롣���� **���Ʒ�ת��Inversion of Control, IoC��** ���������Ը���Ч�ع���������ϵ�Ͷ����������ڡ��߼�Ӧ���У���� **����ӿڱ��** �� **����λ��ģʽ��Service Locator Pattern��**�����Խ�һ������ϵͳģ�飬��������Ŀɲ����Ժ�ά���ԡ�

---

## 7. �ܽ� {#conclusion}

SOLID ԭ��Ϊ�������ṩ�˼�ʵ�Ļ�����ͨ����ѭ��Щԭ�򣬿������ܹ�����������ά������չ������ϵͳ��������ÿ��ԭ��Ĺؼ�Ҫ�㣺

- **��һְ��ԭ�� (SRP)**��ȷ��ÿ����ֻ��һ��ְ����������ھ��ԣ�������϶ȡ�
- **���ŷ��ԭ�� (OCP)**�����ʵ��Ӧ����չ���ţ����޸ķ�գ�ͨ������Ͷ�̬��ʵ�ֹ�����չ��
- **�����滻ԭ�� (LSP)**������Ӧ������������Ӱ��������ȷ�ԣ�ȷ���̳в�ε���ȷ�Ժ��ȶ��ԡ�
- **�ӿڸ���ԭ�� (ISP)**�����Ӵ�Ľӿڲ��Ϊ��ϸ���ȵĽӿڣ�ȷ����ֻ������������Ҫ�Ľӿڡ�
- **��������ԭ�� (DIP)**���߲�ģ�鲻�����ڵͲ�ģ�飬���߶������ڳ�������ϵͳ������ԺͿɲ����ԡ�

���ղ�Ӧ�� SOLID ԭ�򣬿�������������������������ά���ɱ���ʹ���ϵͳ�߱����õ���Ӧ�ԺͿ���չ�ԡ�