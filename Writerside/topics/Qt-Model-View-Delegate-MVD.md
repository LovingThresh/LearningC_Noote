# Qt Model-View-Delegate (MVD)

## Ŀ¼
1. [MVD�ܹ�����](#MVD-Overview)
2. [QTreeWidget vs QTreeView - ʹ��ģ�͵�����](#TreeWidget-vs-TreeView)
3. [�Զ���ģ����ί�е�Ӧ��](#Custom-Model-Delegate)
4. [�߼����ԣ�֧�ָ������ݵ�QTreeView](#Advanced-TreeView-Features)
5. [�ܽ�](#Summary)

---

## MVD�ܹ����� {#MVD-Overview}

### ����������������
Qt �� **Model-View-Delegate (MVD)** �ܹ���һ�ַ������ݡ���ʾ���û����������ģʽ�������ڸ��ӵ��û�����������ͨ������ģ�͡���ͼ������ί�У������˴���Ŀ�ά���ԺͿ���չ�ԡ����ڸ��ӵ���״�ṹ�������ؼ��� `QTreeWidget` �� `QTreeView` ����չʾ���νṹ�����ݡ��� `QTreeWidget` �Ǽ򻯰棬�ʺϼ�����չʾ���� `QTreeView` �ǻ��� `MVD` �ܹ����ʺϴ����ӵ����ݽṹ����Ϊ��

### �ؼ��㣺
- **Model (ģ��)**������������ݣ����������ݵ���ʾ��ʽ��
- **View (��ͼ)**������ģ�͵�����չʾ���û�����ֱ�Ӵ��������߼���
- **Delegate (ί��)**�������û��������ӵ�������Ⱦ��ʵ���Զ��嵥Ԫ��༭��

��ʹ�� `QTreeWidget` ʱ��ģ�ͺ���ͼ�ǽ��ܽ�ϵģ��� `QTreeView` ���������ݺ���ʾ��������߼����Զ�������չ��

---

## QTreeWidget vs QTreeView - ʹ��ģ�͵����� {#TreeWidget-vs-TreeView}

### ����������������
`QTreeWidget` �� `QTreeView` �ļ򻯰棬������һ�� `QTreeWidgetItem` ��ģ�͡���ˣ�ʹ�� `QTreeWidget` ʱ��������ֻ����� `QTreeWidgetItem`��������ʽ����ģ�͡��� `QTreeView` �����ⲿ�� `QAbstractItemModel`����ʹ���ڴ�������ӵ�����ʱ������

### �ؼ��㣺
- **QTreeWidget**���ʺϼ򵥵Ĳ�νṹ�����ó���������̬�������ݽṹ�����ļ�Ŀ¼չʾ��
- **QTreeView + QAbstractItemModel**�������ڶ�̬�����ӵ����νṹ���ܸ��õش���������ݺͶ������Ĳ�����

### ����ʾ����ʹ��QTreeWidget����

```C++
QTreeWidget *treeWidget = new QTreeWidget(this);
treeWidget->setColumnCount(2);
treeWidget->setHeaderLabels(QStringList() << "Name" << "Value");

QTreeWidgetItem *rootItem = new QTreeWidgetItem(treeWidget);
rootItem->setText(0, "Root Item");

QTreeWidgetItem *childItem = new QTreeWidgetItem(rootItem);
childItem->setText(0, "Child Item");
childItem->setText(1, "Value 1");

treeWidget->addTopLevelItem(rootItem);
```

### ����ʾ����ʹ��QTreeView + �Զ���ģ�ͣ���

```C++
class TreeModel : public QAbstractItemModel {
    // ʵ�� QAbstractItemModel �ı�Ҫ�������� rowCount(), columnCount(), data(), etc.
};

QTreeView *treeView = new QTreeView(this);
TreeModel *model = new TreeModel();
treeView->setModel(model);
```

ʹ�� `QTreeView` ʱ����������ͨ�� `TreeModel` ������Ϊ��̬�������ݡ�֧�ֶ��̵߳ȳ����ṩ�˸���Ŀ���������

---

## �Զ���ģ����ί�е�Ӧ�� {#Custom-Model-Delegate}

### ����������������
�ڸ��ӵ�Ӧ�ó����У������߿�����Ҫ�Զ���ģ����֧�ָ��Ӹ��ӵ�ҵ���߼������ݽṹ��ͬʱ��ʹ��ί�п������嵥Ԫ��������༭��ʽ��

### �ؼ��㣺
- **�Զ���ģ��**��ͨ���̳� `QAbstractItemModel` �� `QStandardItemModel` ʵ�ָ��ӵ����ݽṹ��
- **ί�У�Delegate��**��ͨ���̳� `QStyledItemDelegate` ���Զ�����Ⱦ��༭���Ľ�����

### ����ʾ�����Զ���ģ�ͣ���

```C++
class TreeModel : public QAbstractItemModel {
    // ���캯����������ڵ����ӽڵ���߼�
public:
    int rowCount(const QModelIndex &parent = QModelIndex()) const override {
        // ��������
    }

    int columnCount(const QModelIndex &parent = QModelIndex()) const override {
        return 2;  // ���У�Name �� Value
    }

    QVariant data(const QModelIndex &index, int role) const override {
        if (!index.isValid())
            return QVariant();

        if (role == Qt::DisplayRole) {
            if (index.column() == 0) {
                return "Node Name";  // ʾ������
            } else if (index.column() == 1) {
                return "Node Value";  // ʾ������
            }
        }
        return QVariant();
    }

    QModelIndex parent(const QModelIndex &child) const override {
        // ���ظ��ڵ���߼�
    }

    QModelIndex index(int row, int column, const QModelIndex &parent = QModelIndex()) const override {
        // ����ָ��λ�õ��ӽڵ�����
    }
};
```

### ����ʾ�����Զ���ί�У���

```C++
class CustomDelegate : public QStyledItemDelegate {
public:
    CustomDelegate(QObject *parent = nullptr) : QStyledItemDelegate(parent) {}

    QWidget *createEditor(QWidget *parent, const QStyleOptionViewItem &option, const QModelIndex &index) const override {
        // �����Զ���༭��
        QLineEdit *editor = new QLineEdit(parent);
        return editor;
    }

    void setEditorData(QWidget *editor, const QModelIndex &index) const override {
        // ���������õ��༭��
        QString value = index.model()->data(index, Qt::EditRole).toString();
        static_cast<QLineEdit*>(editor)->setText(value);
    }

    void setModelData(QWidget *editor, QAbstractItemModel *model, const QModelIndex &index) const override {
        // ���༭������ݱ����ģ��
        QLineEdit *lineEdit = static_cast<QLineEdit*>(editor);
        model->setData(index, lineEdit->text(), Qt::EditRole);
    }

    void updateEditorGeometry(QWidget *editor, const QStyleOptionViewItem &option, const QModelIndex &index) const override {
        // ���ñ༭���Ĵ�С��λ��
        editor->setGeometry(option.rect);
    }
};
```

�Զ��� `TreeModel` �����ӵ����ݽṹ���� `CustomDelegate` �ṩ�Զ���ĵ�Ԫ��༭�߼���

---

## �߼����ԣ�֧�ָ������ݵ�QTreeView {#Advanced-TreeView-Features}

### ����������������
��ʹ�� `QTreeView` ��չʾ��������ʱ��ģ�͵���ƺ�ί�е�Ӧ��������Ҫ��������һЩ�����ĸ߼����������ԣ�

### �ؼ��㣺
- **֧�ֶ�̬���ݼ���**��ģ�Ϳ��Ը�����Ҫ��̬�������ݣ��ʺϴ��ģ���ݼ���
- **������**��ͨ������ `fetchMore()` �� `canFetchMore()` ʵ�������ء�
- **�Ϸ�֧��**��ͨ������ `dragMoveEvent()` �� `dropEvent()` ��ʵ���ϷŲ�����
- **���ӵ����ݽṹ**��`QAbstractItemModel` ���Ա�ʾ������ȵ����νṹ��

### ����ʾ���������أ���

```C++
bool TreeModel::canFetchMore(const QModelIndex &parent) const {
    // �ж��Ƿ��и���������Ҫ����
    return true;  // ������Զ���Լ��ظ�������
}

void TreeModel::fetchMore(const QModelIndex &parent) {
    // ���ظ������ݵ��߼�
    beginInsertRows(parent, rowCount(parent), rowCount(parent) + 10);
    // ��Ӹ�������ݽڵ�
    endInsertRows();
}
```

---

## �ܽ� {#Summary}

- Qt �� MVD �ܹ������ݡ���ʾ�뽻�����룬Ϊ�������ݵĴ����ṩ�˸߶ȵ�����Ժ���չ�ԡ�
- `QTreeWidget` �����ڼ򵥵����νṹ���� `QTreeView` �����Զ���ģ�����ʺϴ���̬�����ӵ���������
- �Զ���ģ�Ϳ����ÿ������������ӵ����ݽṹ����ͨ��ί�п��Զ���ʾ�ͱ༭�߼�����ϸ���ȵĿ��ơ�
- �߼������������ء��ϷŲ���ʹ `QTreeView` �ܴ�����ģ���ݺ͸��ӵ��û���������

ʹ�� MVD ģʽʱ�����õ�ģ�������ί�е�ʵ��������Ҫ����Ϊ���� UI ��ʵ�ִ�������ĺô���