# QT proxy-vs-delegate

## Ŀ¼ {#index}

- [������ί�и���](#proxy-vs-delegate)
- [��״��ͼ������ί��ʵ������](#tree-view-proxy-delegate-example)
- [ʾ������](#example-code)
- [�ܽ�](#summary)

---

## ������ί�и��� {#proxy-vs-delegate}

### ��������������

�ڿ����У������ί���ǳ��������ģʽ���ر����ڸ��� UI �ṹ�У�����״��ͼ������������ί�е��������ȷʹ�������ģ��������Ҫ��һ����˵��

- **����Proxy��**�������ڴ�������Դ����ͼ���߼�������ģ�͡�����ֱ�ӹ������ݣ����Ƕ�ԭʼ���ݽ���ɸѡ�����������������
- **ί�У�Delegate��**������������ͼ�������ʾϸ�ڵĽӿڣ��������Ԫ�صĻ��ƺͽ�����

����״��ͼ�ṹ�У������߷ֱ�ͨ�� `TreeSearchModel`������ģ�ͣ��� `TreeDelegate`��ί���ࣩʵ�ֲ�ͬ�ķֹ��͹��ܡ�

### �ؼ���

- **����Proxy��**�����������ݺ���ͼ�䴦���м��߼���ͨ������ɸѡ������ȣ��������ݹ����֡�
- **ί�У�Delegate��**�����������ͼϸ�ڣ���Ԫ�صĻ��ơ�������������ͼ�����֡�

---

## ��״��ͼ������ί��ʵ������ {#tree-view-proxy-delegate-example}

### ��������������

����״��ͼ�У�ͨ�������¹ؼ������

1. **DataModel**���ṩԭʼ����ģ�ͣ�ֱ�Ӵ�����Դ��ȡ���ݣ�����������ͼ��Ρ�
2. **TreeView**������չʾ��״�ṹ�Ŀؼ���������ģ�ͻ����ģ�͹�������ʾ���ݡ�
3. **TreeDelegate**������Ԫ�صĻ��ƺ��¼������� `TreeView` �е�ÿ���ڵ�Ļ��ƺͽ����߼����������������Զ��塣
4. **TreeSearchModel**���䵱����ģ�ͣ����� `DataModel` �ṩɸѡ�������ܡ�`TreeSearchModel` �ɰ������������� `DataModel` �е����ݣ�ʹ�� `TreeView` ����ʾ���������Ľڵ㡣

### �ؼ���

- **TreeSearchModel������ģ�ͣ�**����Ϊ������� `DataModel` ��ɸѡ���ݣ������������ṩ�������������ݡ�
- **TreeDelegate��ί���ࣩ**������ڵ�Ļ��ƺͽ���ϸ�ڣ�����ͼ�����ݷ��룬���ڸ��Ի���ʾ��

---

## ʾ������ {#example-code}

```C++
#include <QAbstractItemModel>
#include <QSortFilterProxyModel>
#include <QStyledItemDelegate>
#include <QTreeView>

// DataModel: �ṩԭʼ����
class DataModel : public QAbstractItemModel {
    Q_OBJECT
public:
    // ���ݴ����߼�ʡ��...
};

// TreeSearchModel: ����ɸѡ����ģ��
class TreeSearchModel : public QSortFilterProxyModel {
    Q_OBJECT
public:
    explicit TreeSearchModel(QObject* pParent = nullptr) 
        : QSortFilterProxyModel(pParent) {
        setDynamicSortFilter(true);
    }

protected:
    // ��д���˹���
    bool filterAcceptsRow(int nSourceRow, const QModelIndex &oSourceParent) const override {
        // ��������ɸѡ����
        return true; // ʾ����Ĭ��ͨ��
    }
};

// TreeDelegate: �Զ�����ƺͽ���ί��
class TreeDelegate : public QStyledItemDelegate {
    Q_OBJECT
public:
    explicit TreeDelegate(QObject* pParent = nullptr) : QStyledItemDelegate(pParent) {}

    void paint(QPainter* pPainter, const QStyleOptionViewItem &oOption, const QModelIndex &oIndex) const override {
        // ���ƻ��ڵ����
        QStyledItemDelegate::paint(pPainter, oOption, oIndex);
    }
};

// TreeView: ��״��ͼ
QTreeView* createTreeView(DataModel* pDataModel, TreeSearchModel* pProxyModel, TreeDelegate* pDelegate) {
    auto* pTreeView = new QTreeView;
    pProxyModel->setSourceModel(pDataModel); // ���ô�������Դ
    pTreeView->setModel(pProxyModel);        // ������״��ͼ������ģ��Ϊ����ģ��
    pTreeView->setItemDelegate(pDelegate);    // ����ί��
    return pTreeView;
}
```

---

## �ܽ� {#summary}

- **����Proxy��** ��Ҫ�������ݵ�ɸѡ������ʹ�� `TreeView` չʾ�����ݷ��Ϲ����������������ݹ���
- **ί�У�Delegate��** ��Ҫ���ڶ��ƻ���ͼ�е�Ԫ��չʾ�ͽ���������ÿ���ڵ�Ļ��ƺ��û�������