# Signal Slot Mechanism

## 目录
1. [信号与槽的基本工作原理](#Basic-Signal-Slot-Mechanism)
2. [信号触发时的阻塞行为](#Blocking-Behavior)
3. [同步与异步连接的区别](#Synchronous-Asynchronous)
4. [自定义信号与槽的实现](#Custom-Signal-Slot)
5. [总结](#Summary)

---

## 信号与槽的基本工作原理 {#Basic-Signal-Slot-Mechanism}

### 背景与问题描述：
Qt 的信号与槽机制是其事件处理模型的核心。信号用于在某些事件发生时通知系统，槽函数则是与该事件相关的处理函数。用户常常会关心，当信号触发并调用槽函数时，信号触发的地方是否会被阻塞，等待槽函数执行完成后再继续执行。

### 关键点：
- 信号可以被任何对象发射，槽是响应信号的函数。
- Qt 使用 **MOC（Meta-Object Compiler）** 来支持信号与槽的机制。
- 默认情况下，信号是异步连接的。

### 代码示例：

```C++
// 定义信号与槽
class MyObject : public QObject {
    Q_OBJECT
public:
    MyObject() {
        connect(this, &MyObject::mySignal, this, &MyObject::mySlot);
    }

signals:
    void mySignal();  // 信号

public slots:
    void mySlot() {   // 槽函数
        qDebug() << "槽函数被调用";
    }
};

// 发射信号
int main() {
    MyObject obj;
    emit obj.mySignal();  // 触发信号
    return 0;
}
```

在这个例子中，当信号 `mySignal` 被发射时，槽函数 `mySlot` 会被执行。默认情况下，Qt 会根据对象所在线程的不同，决定信号与槽之间的连接方式。

---

## 信号触发时的阻塞行为 {#Blocking-Behavior}

### 背景与问题描述：
用户关心信号触发时，是否会阻塞信号发射的地方，等待槽函数执行完毕再继续执行。

### 关键点：
- **Qt 默认的行为**：当信号和槽在同一线程中运行时，信号的触发是同步的，即发射信号的地方会等待槽函数执行完成。
- **跨线程时**：如果信号与槽位于不同线程，则默认是异步的，信号发射后会立即返回，发射信号的地方不会被阻塞。

### 代码示例：

```C++
class MyObject : public QObject {
    Q_OBJECT
public slots:
    void slowSlot() {
        QThread::sleep(2);  // 模拟耗时操作
        qDebug() << "槽函数执行完毕";
    }
};

// 同一线程中的同步信号
int main() {
    MyObject obj;
    QElapsedTimer timer;
    timer.start();
    
    connect(&obj, &MyObject::mySignal, &obj, &MyObject::slowSlot);
    emit obj.mySignal();  // 触发信号，主线程会等待 slowSlot 执行完毕
    
    qDebug() << "信号发射后的时间：" << timer.elapsed() << "毫秒";
    return 0;
}
```

输出结果将显示，信号发射的地方确实等待了 `slowSlot` 执行完毕才继续。

---

## 同步与异步连接的区别 {#Synchronous-Asynchronous}

### 背景与问题描述：
Qt 提供了同步（直接）和异步（队列）连接方式。用户需要根据具体情况，选择合适的连接类型。

### 关键点：
- **同步连接（Qt::DirectConnection）**：信号发射时，直接调用槽函数，发射信号的地方会被阻塞。
- **异步连接（Qt::QueuedConnection）**：信号发射后，槽函数会在事件循环的下一个空闲时间执行，信号发射方不会被阻塞。

### 代码示例：

```C++
connect(sender, &Sender::signalName, receiver, &Receiver::slotName, Qt::QueuedConnection);
```

这样设置后，即便信号与槽位于同一线程，信号发射方也不会等待槽函数执行完毕。

---

## 自定义信号与槽的实现 {#Custom-Signal-Slot}

### 背景与问题描述：
Qt 允许用户自定义信号与槽，以便在程序中创建更灵活的事件处理机制。

### 关键点：
- 信号只需要在类中使用 `signals:` 关键字声明，不需要实现。
- 槽函数是普通的成员函数，但需要用 `slots:` 关键字标记。

### 代码示例：

```C++
class CustomObject : public QObject {
    Q_OBJECT
public:
    CustomObject() {
        connect(this, &CustomObject::customSignal, this, &CustomObject::customSlot);
    }

signals:
    void customSignal(int value);  // 自定义信号

public slots:
    void customSlot(int value) {   // 自定义槽函数
        qDebug() << "收到信号，值为：" << value;
    }
};

// 使用信号与槽
int main() {
    CustomObject obj;
    emit obj.customSignal(42);  // 触发信号，传递参数
    return 0;
}
```

---

## 总结 {#Summary}

- 当信号与槽在同一线程中时，信号触发会阻塞发射信号的地方，直到槽函数执行完毕。
- 对于跨线程的信号与槽连接，默认是异步的，信号发射后不会阻塞。
- 可以通过指定连接类型（`Qt::DirectConnection` 或 `Qt::QueuedConnection`）来控制同步或异步行为。
- 自定义信号与槽时，信号不需要实现，槽函数只是带有 `slots` 关键字的普通成员函数。

通过合理设计信号与槽的连接方式，可以实现高效的事件驱动编程。