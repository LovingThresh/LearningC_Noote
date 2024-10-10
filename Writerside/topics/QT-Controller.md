# QT Controller

## Ŀ¼ {#index}

- [��������������](#background)
- [������ģʽ������](#controller-advantages)
- [���ͨ�ŵĿ�����ʵ��](#controller-implementation)
- [ʾ������](#example-code)
- [�ܽ�](#summary)

---

## �������������� {#background}

### ��������

�� Qt �����У��������������������ʱ����Щ������ֱ��ͨ�ţ������źŲۣ����ܵ��´�����϶����ߣ�����ά�������ԣ���Ҫ���������

1. **�ź���·����**����������Ϊ�н��������źŻᵼ���ź���·���ӻ��������ڹ���
2. **ѭ������**�������֮��ֱ�ӻ�����ÿ��ܻ����ѭ���������Ӷ��������������������⡣

Ϊ�˽���������⣬����ͨ������һ��������**��������**�����为������Э��������Ľ�����

---

## ������ģʽ������ {#controller-advantages}

������ģʽ����������������ͨ�ź�����������Ҫ�ŵ������

1. **�����**�������ͨ������������ͨ�ţ������໥������������Ҳ����ֱ�Ӵ���������Ľ�����
2. **���������**���źŲ����Ӽ����ڿ������У�����������ֻ��Ϳ�����ͨ�ż��ɣ��Ӷ����˴���ṹ��
3. **ѭ��������ֹ**����������Ч��ֹ������ѭ�����ã��������߼�ת�Ƶ��������д���

---

## ���ͨ�ŵĿ�����ʵ�� {#controller-implementation}

### �ؼ���

1. **�����������**�����������������������ָ�룬����������ǵ��źŲ����ӡ�
2. **������Կ�������͸����**�����������Ҫ֪���˴˵Ĵ��ڣ�����ͨ��������������Ϣ���ݡ�
3. **�������еĳ�Աָ��**���������н����п�����ָ�룬���������ͨ����ȫ���ɿ���������

---

## ʾ������ {#example-code}

�����ǿ��������ʾ�����룬չʾ�����ʵ���������Ķ���ͨ�š�

```C++
#include <QWidget>
#include <QObject>
#include <QPushButton>
#include <QLabel>

// ����� A
class ComponentA : public QWidget {
    Q_OBJECT
public:
    explicit ComponentA(QWidget* pParent = nullptr) : QWidget(pParent) {
        m_pButton = new QPushButton(tr("Click Me"), this);
        connect(m_pButton, &QPushButton::clicked, this, &ComponentA::buttonClicked);
    }

signals:
    void buttonClicked();

private:
    QPushButton* m_pButton;
};

// ����� B
class ComponentB : public QWidget {
    Q_OBJECT
public:
    explicit ComponentB(QWidget* pParent = nullptr) : QWidget(pParent) {
        m_pLabel = new QLabel(tr("Waiting for Click"), this);
    }

public slots:
    void updateLabel() {
        m_pLabel->setText(tr("Button in Component A Clicked!"));
    }

private:
    QLabel* m_pLabel;
};

// ��������
class Controller : public QObject {
    Q_OBJECT
public:
    Controller(ComponentA* pComponentA, ComponentB* pComponentB, QObject* pParent = nullptr)
        : QObject(pParent), m_pComponentA(pComponentA), m_pComponentB(pComponentB) {
        connect(m_pComponentA, &ComponentA::buttonClicked, m_pComponentB, &ComponentB::updateLabel);
    }

private:
    ComponentA* m_pComponentA;
    ComponentB* m_pComponentB;
};

// ������
class MainWindow : public QWidget {
    Q_OBJECT
public:
    MainWindow(QWidget* pParent = nullptr) : QWidget(pParent) {
        m_pComponentA = new ComponentA(this);
        m_pComponentB = new ComponentB(this);
        m_pController = new Controller(m_pComponentA, m_pComponentB, this);  // ����������������ͨ��
    }

private:
    ComponentA* m_pComponentA;
    ComponentB* m_pComponentB;
    Controller* m_pController;
};
```

---

## �ܽ� {#summary}

ͨ�������������������źŲ����ӣ�������Ч�����������������룬ʹ����ṹ����������������Ķ����ԡ�ͬʱ�����ַ�����������������ֱ�ӵ��ã�����ѭ���������⡣

### �ؼ���ع�

- **�����**�������������֮����߼����룬����������Ĵ���ṹ��
- **���й����źŲ�**�����������й�����������źŲۣ�ʹ������׶���ά����
- **��ֹѭ������**��������������������ѭ���������⣬����˴�����ȶ��ԺͿ�ά���ԡ�