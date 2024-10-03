# Knowledge

1. [MOC Working Mechanism Overview](#MOC-Working-Mechanism-Overview)
2. [Signal Emission Process](#Signal-Emission-Process)

---

## MOC Working Mechanism Overview {#MOC-Working-Mechanism-Overview}

### ����������
�� Qt �У�MOC��Meta-Object Compiler�������ڱ���ʱ���Զ����� `QObject` ����ص��ź���ۻ��ơ�MOC ͨ��ɨ��Դ�����е� `Q_OBJECT` �겢���� `signals` �� `slots` ���������ɴ��롣��Щ������Ӧ�ó�������ʱ�����źŲ۵����Ӻͼ����� MOC �Ĺ������������ڿ����߸��õ��Ż���ʹ���źŲۻ��ơ�

### �ؼ��㣺
1. **MOC �ڹ���ʱ�Ĺ���**��
	- ɨ������е� `Q_OBJECT` �꣬ʶ����Щ����Ҫ�źŲ�֧�֡�
	- �����źš��ۺ����������������ɴ����Թ�����ʱʹ�á�

2. **���ɵĴ���**��
	- ����ֻ����Ԫ���ݣ�������������źš��ۺ����ԡ�
	- �����źŵĴ�����룬�����źŵļ��
	- ���� `qt_static_metacall` ��̬���������������źŲ۵ĺ������÷ַ���

---

## Signal Emission Process {#Signal-Emission-Process}

### ����������
�źŵķ����漰�ಽ������������ MOC ���ɵĴ����������������������źŵȡ������Щ���������ڿ����������˽��źŲۻ��Ƶ����й��̣���������θ�Ч�����źźͲ۵���������á�

### �ؼ��㣺
1. **���ô������**��
	- �źŷ���ʱ������ MOC ���ɵĴ����������Щ�����ᴦ���źŵĲ����ͼ��

2. **��������**��
	- ���д��ݸ��źŵĲ��������洢��һ�� `void*` �����У���ͨ��ָ�����ʽ���ݡ�

3. **���� `QMetaObject::activate`**��
	- �����źŲ��ҵ��������ӵĲۺ�����

4. **���� `qt_static_metacall`**��
	- ����ÿ���۶��󣬵����� `qt_static_metacall` �����Էַ���ִ�вۺ�����

5. **����ת����۵���**��
	- �� `void*` ���͵Ĳ���ת���ؾ������ͣ��������û�����Ĳۺ�����

### ����ʾ����

```C++
// Emitter ��ͨ���źŷ����ַ���
#include <QObject>
#include <QString>
#include <iostream>

class Emitter : public QObject {
    Q_OBJECT  // MOC ��ɨ�赽����겢������ش���

public:
    signals:
    void stringEmitted(const QString &str);  // �ź��������� MOC ���ɴ������

    void sendString(const QString &str) {
        emit stringEmitted(str);  // �����źţ������ۺ���
    }
};

class Receiver : public QObject {
    Q_OBJECT

public slots:
    void onStringEmitted(const QString &str) {
        std::cout << "Received string: " << str.toStdString() << std::endl;
    }
};

int main() {
    Emitter emitter;
    Receiver receiver;

    // �����źźͲ�
    QObject::connect(&emitter, &Emitter::stringEmitted, &receiver, &Receiver::onStringEmitted);

    // �����ź�
    emitter.sendString("Hello, Qt!");

    return 0;
}
```

### �ܽ᣺
MOC �Ĺ���ԭ����� Qt �źŲۻ���������Ҫ��ͨ�������źŲ���صĴ��������Ԫ���ݣ�MOC ȷ�����źŷ���ʱ���Զ�̬���Ҳ�������֮���ӵĲۺ����������Щ�ڲ����������������Ż�Ӧ�ó��������Ч�ʲ�����Ǳ�����⡣