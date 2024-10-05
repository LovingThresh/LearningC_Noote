# Qt Model-View-Delegate (MVD)

## 目录
1. [MVD架构概述](#MVD-Overview)
2. [QTreeWidget vs QTreeView - 使用模型的区别](#TreeWidget-vs-TreeView)
3. [自定义模型与委托的应用](#Custom-Model-Delegate)
4. [高级特性：支持复杂数据的QTreeView](#Advanced-TreeView-Features)
5. [总结](#Summary)

---

## MVD架构概述 {#MVD-Overview}

### 背景与问题描述：
Qt 的 **Model-View-Delegate (MVD)** 架构是一种分离数据、显示和用户交互的设计模式，适用于复杂的用户界面需求。它通过分离模型、视图和数据委托，提升了代码的可维护性和可扩展性。对于复杂的树状结构，常见控件如 `QTreeWidget` 和 `QTreeView` 都能展示树形结构的数据。但 `QTreeWidget` 是简化版，适合简单数据展示，而 `QTreeView` 是基于 `MVD` 架构，适合处理复杂的数据结构和行为。

### 关键点：
- **Model (模型)**：负责管理数据，不关心数据的显示方式。
- **View (视图)**：负责将模型的数据展示给用户，不直接处理数据逻辑。
- **Delegate (委托)**：负责用户交互或复杂的数据渲染，实现自定义单元格编辑。

在使用 `QTreeWidget` 时，模型和视图是紧密结合的，而 `QTreeView` 分离了数据和显示，允许更高级的自定义与扩展。

---

## QTreeWidget vs QTreeView - 使用模型的区别 {#TreeWidget-vs-TreeView}

### 背景与问题描述：
`QTreeWidget` 是 `QTreeView` 的简化版，内置了一个 `QTreeWidgetItem` 的模型。因此，使用 `QTreeWidget` 时，开发者只需操作 `QTreeWidgetItem`，不必显式处理模型。而 `QTreeView` 依赖外部的 `QAbstractItemModel`，这使其在处理更复杂的数据时更加灵活。

### 关键点：
- **QTreeWidget**：适合简单的层次结构，适用场景包括静态树形数据结构，如文件目录展示。
- **QTreeView + QAbstractItemModel**：适用于动态、复杂的树形结构，能更好地处理大量数据和多样化的操作。

### 代码示例（使用QTreeWidget）：

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

### 代码示例（使用QTreeView + 自定义模型）：

```C++
class TreeModel : public QAbstractItemModel {
    // 实现 QAbstractItemModel 的必要函数，如 rowCount(), columnCount(), data(), etc.
};

QTreeView *treeView = new QTreeView(this);
TreeModel *model = new TreeModel();
treeView->setModel(model);
```

使用 `QTreeView` 时，所有数据通过 `TreeModel` 管理，这为动态更新数据、支持多线程等场景提供了更多的控制能力。

---

## 自定义模型与委托的应用 {#Custom-Model-Delegate}

### 背景与问题描述：
在复杂的应用场景中，开发者可能需要自定义模型来支持更加复杂的业务逻辑和数据结构。同时，使用委托可以灵活定义单元格的外观与编辑方式。

### 关键点：
- **自定义模型**：通过继承 `QAbstractItemModel` 或 `QStandardItemModel` 实现复杂的数据结构。
- **委托（Delegate）**：通过继承 `QStyledItemDelegate` 来自定义渲染与编辑器的交互。

### 代码示例（自定义模型）：

```C++
class TreeModel : public QAbstractItemModel {
    // 构造函数，管理根节点与子节点的逻辑
public:
    int rowCount(const QModelIndex &parent = QModelIndex()) const override {
        // 返回行数
    }

    int columnCount(const QModelIndex &parent = QModelIndex()) const override {
        return 2;  // 两列，Name 和 Value
    }

    QVariant data(const QModelIndex &index, int role) const override {
        if (!index.isValid())
            return QVariant();

        if (role == Qt::DisplayRole) {
            if (index.column() == 0) {
                return "Node Name";  // 示例数据
            } else if (index.column() == 1) {
                return "Node Value";  // 示例数据
            }
        }
        return QVariant();
    }

    QModelIndex parent(const QModelIndex &child) const override {
        // 返回父节点的逻辑
    }

    QModelIndex index(int row, int column, const QModelIndex &parent = QModelIndex()) const override {
        // 返回指定位置的子节点索引
    }
};
```

### 代码示例（自定义委托）：

```C++
class CustomDelegate : public QStyledItemDelegate {
public:
    CustomDelegate(QObject *parent = nullptr) : QStyledItemDelegate(parent) {}

    QWidget *createEditor(QWidget *parent, const QStyleOptionViewItem &option, const QModelIndex &index) const override {
        // 创建自定义编辑器
        QLineEdit *editor = new QLineEdit(parent);
        return editor;
    }

    void setEditorData(QWidget *editor, const QModelIndex &index) const override {
        // 将数据设置到编辑器
        QString value = index.model()->data(index, Qt::EditRole).toString();
        static_cast<QLineEdit*>(editor)->setText(value);
    }

    void setModelData(QWidget *editor, QAbstractItemModel *model, const QModelIndex &index) const override {
        // 将编辑后的数据保存回模型
        QLineEdit *lineEdit = static_cast<QLineEdit*>(editor);
        model->setData(index, lineEdit->text(), Qt::EditRole);
    }

    void updateEditorGeometry(QWidget *editor, const QStyleOptionViewItem &option, const QModelIndex &index) const override {
        // 设置编辑器的大小和位置
        editor->setGeometry(option.rect);
    }
};
```

自定义 `TreeModel` 管理复杂的数据结构，而 `CustomDelegate` 提供自定义的单元格编辑逻辑。

---

## 高级特性：支持复杂数据的QTreeView {#Advanced-TreeView-Features}

### 背景与问题描述：
当使用 `QTreeView` 来展示复杂数据时，模型的设计和委托的应用至关重要。以下是一些常见的高级需求与特性：

### 关键点：
- **支持动态数据加载**：模型可以根据需要动态加载数据，适合大规模数据集。
- **懒加载**：通过重载 `fetchMore()` 和 `canFetchMore()` 实现懒加载。
- **拖放支持**：通过重载 `dragMoveEvent()` 和 `dropEvent()` 来实现拖放操作。
- **复杂的数据结构**：`QAbstractItemModel` 可以表示任意深度的树形结构。

### 代码示例（懒加载）：

```C++
bool TreeModel::canFetchMore(const QModelIndex &parent) const {
    // 判断是否有更多数据需要加载
    return true;  // 假设永远可以加载更多数据
}

void TreeModel::fetchMore(const QModelIndex &parent) {
    // 加载更多数据的逻辑
    beginInsertRows(parent, rowCount(parent), rowCount(parent) + 10);
    // 添加更多的数据节点
    endInsertRows();
}
```

---

## 总结 {#Summary}

- Qt 的 MVD 架构将数据、显示与交互分离，为复杂数据的处理提供了高度的灵活性和扩展性。
- `QTreeWidget` 适用于简单的树形结构，而 `QTreeView` 搭配自定义模型则适合处理动态、复杂的数据需求。
- 自定义模型可以让开发者灵活管理复杂的数据结构，而通过委托可以对显示和编辑逻辑进行细粒度的控制。
- 高级功能如懒加载、拖放操作使 `QTreeView` 能处理大规模数据和复杂的用户交互需求。

使用 MVD 模式时，良好的模型设计与委托的实现至关重要，能为复杂 UI 的实现带来极大的好处。