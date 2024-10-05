# Signal Slot Mechanism

## Ŀ¼
1. [�ź���۵Ļ�������ԭ��](#Basic-Signal-Slot-Mechanism)
2. [�źŴ���ʱ��������Ϊ](#Blocking-Behavior)
3. [ͬ�����첽���ӵ�����](#Synchronous-Asynchronous)
4. [�Զ����ź���۵�ʵ��](#Custom-Signal-Slot)
5. [�ܽ�](#Summary)

---

## �ź���۵Ļ�������ԭ�� {#Basic-Signal-Slot-Mechanism}

### ����������������
Qt ���ź���ۻ��������¼�����ģ�͵ĺ��ġ��ź�������ĳЩ�¼�����ʱ֪ͨϵͳ���ۺ�����������¼���صĴ��������û���������ģ����źŴ��������òۺ���ʱ���źŴ����ĵط��Ƿ�ᱻ�������ȴ��ۺ���ִ����ɺ��ټ���ִ�С�

### �ؼ��㣺
- �źſ��Ա��κζ����䣬������Ӧ�źŵĺ�����
- Qt ʹ�� **MOC��Meta-Object Compiler��** ��֧���ź���۵Ļ��ơ�
- Ĭ������£��ź����첽���ӵġ�

### ����ʾ����

```C++
// �����ź����
class MyObject : public QObject {
    Q_OBJECT
public:
    MyObject() {
        connect(this, &MyObject::mySignal, this, &MyObject::mySlot);
    }

signals:
    void mySignal();  // �ź�

public slots:
    void mySlot() {   // �ۺ���
        qDebug() << "�ۺ���������";
    }
};

// �����ź�
int main() {
    MyObject obj;
    emit obj.mySignal();  // �����ź�
    return 0;
}
```

����������У����ź� `mySignal` ������ʱ���ۺ��� `mySlot` �ᱻִ�С�Ĭ������£�Qt ����ݶ��������̵߳Ĳ�ͬ�������ź����֮������ӷ�ʽ��

---

## �źŴ���ʱ��������Ϊ {#Blocking-Behavior}

### ����������������
�û������źŴ���ʱ���Ƿ�������źŷ���ĵط����ȴ��ۺ���ִ������ټ���ִ�С�

### �ؼ��㣺
- **Qt Ĭ�ϵ���Ϊ**�����źźͲ���ͬһ�߳�������ʱ���źŵĴ�����ͬ���ģ��������źŵĵط���ȴ��ۺ���ִ����ɡ�
- **���߳�ʱ**������ź����λ�ڲ�ͬ�̣߳���Ĭ�����첽�ģ��źŷ������������أ������źŵĵط����ᱻ������

### ����ʾ����

```C++
class MyObject : public QObject {
    Q_OBJECT
public slots:
    void slowSlot() {
        QThread::sleep(2);  // ģ���ʱ����
        qDebug() << "�ۺ���ִ�����";
    }
};

// ͬһ�߳��е�ͬ���ź�
int main() {
    MyObject obj;
    QElapsedTimer timer;
    timer.start();
    
    connect(&obj, &MyObject::mySignal, &obj, &MyObject::slowSlot);
    emit obj.mySignal();  // �����źţ����̻߳�ȴ� slowSlot ִ�����
    
    qDebug() << "�źŷ�����ʱ�䣺" << timer.elapsed() << "����";
    return 0;
}
```

����������ʾ���źŷ���ĵط�ȷʵ�ȴ��� `slowSlot` ִ����ϲż�����

---

## ͬ�����첽���ӵ����� {#Synchronous-Asynchronous}

### ����������������
Qt �ṩ��ͬ����ֱ�ӣ����첽�����У����ӷ�ʽ���û���Ҫ���ݾ��������ѡ����ʵ��������͡�

### �ؼ��㣺
- **ͬ�����ӣ�Qt::DirectConnection��**���źŷ���ʱ��ֱ�ӵ��òۺ����������źŵĵط��ᱻ������
- **�첽���ӣ�Qt::QueuedConnection��**���źŷ���󣬲ۺ��������¼�ѭ������һ������ʱ��ִ�У��źŷ��䷽���ᱻ������

### ����ʾ����

```C++
connect(sender, &Sender::signalName, receiver, &Receiver::slotName, Qt::QueuedConnection);
```

�������ú󣬼����ź����λ��ͬһ�̣߳��źŷ��䷽Ҳ����ȴ��ۺ���ִ����ϡ�

---

## �Զ����ź���۵�ʵ�� {#Custom-Signal-Slot}

### ����������������
Qt �����û��Զ����ź���ۣ��Ա��ڳ����д����������¼�������ơ�

### �ؼ��㣺
- �ź�ֻ��Ҫ������ʹ�� `signals:` �ؼ�������������Ҫʵ�֡�
- �ۺ�������ͨ�ĳ�Ա����������Ҫ�� `slots:` �ؼ��ֱ�ǡ�

### ����ʾ����

```C++
class CustomObject : public QObject {
    Q_OBJECT
public:
    CustomObject() {
        connect(this, &CustomObject::customSignal, this, &CustomObject::customSlot);
    }

signals:
    void customSignal(int value);  // �Զ����ź�

public slots:
    void customSlot(int value) {   // �Զ���ۺ���
        qDebug() << "�յ��źţ�ֵΪ��" << value;
    }
};

// ʹ���ź����
int main() {
    CustomObject obj;
    emit obj.customSignal(42);  // �����źţ����ݲ���
    return 0;
}
```

---

## �ܽ� {#Summary}

- ���ź������ͬһ�߳���ʱ���źŴ��������������źŵĵط���ֱ���ۺ���ִ����ϡ�
- ���ڿ��̵߳��ź�������ӣ�Ĭ�����첽�ģ��źŷ���󲻻�������
- ����ͨ��ָ���������ͣ�`Qt::DirectConnection` �� `Qt::QueuedConnection`��������ͬ�����첽��Ϊ��
- �Զ����ź����ʱ���źŲ���Ҫʵ�֣��ۺ���ֻ�Ǵ��� `slots` �ؼ��ֵ���ͨ��Ա������

ͨ����������ź���۵����ӷ�ʽ������ʵ�ָ�Ч���¼�������̡�