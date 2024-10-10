# QT proxy-vs-delegate

## 目录 {#index}

- [代理与委托概述](#proxy-vs-delegate)
- [树状视图代理与委托实例分析](#tree-view-proxy-delegate-example)
- [示例代码](#example-code)
- [总结](#summary)

---

## 代理与委托概述 {#proxy-vs-delegate}

### 背景与问题描述

在开发中，代理和委托是常见的设计模式，特别是在复杂 UI 结构中（如树状视图）。理解代理与委托的区别对正确使用组件和模型至关重要。一般来说：

- **代理（Proxy）**：是用于处理数据源和视图间逻辑的数据模型。它不直接管理数据，而是对原始数据进行筛选、排序或其他操作。
- **委托（Delegate）**：是用于在视图层管理显示细节的接口，负责具体元素的绘制和交互。

在树状视图结构中，这两者分别通过 `TreeSearchModel`（代理模型）和 `TreeDelegate`（委托类）实现不同的分工和功能。

### 关键点

- **代理（Proxy）**：用于在数据和视图间处理中间逻辑，通常用于筛选、排序等，属于数据管理部分。
- **委托（Delegate）**：管理具体视图细节，如元素的绘制、交互，属于视图管理部分。

---

## 树状视图代理与委托实例分析 {#tree-view-proxy-delegate-example}

### 背景与问题描述

在树状视图中，通常有以下关键组件：

1. **DataModel**：提供原始数据模型，直接从数据源获取数据，不负责处理视图层次。
2. **TreeView**：负责展示树状结构的控件，与数据模型或代理模型关联以显示数据。
3. **TreeDelegate**：负责元素的绘制和事件处理，将 `TreeView` 中的每个节点的绘制和交互逻辑独立出来，便于自定义。
4. **TreeSearchModel**：充当代理模型，基于 `DataModel` 提供筛选或排序功能。`TreeSearchModel` 可按搜索条件过滤 `DataModel` 中的数据，使得 `TreeView` 仅显示符合条件的节点。

### 关键点

- **TreeSearchModel（代理模型）**：作为代理负责从 `DataModel` 中筛选数据，按搜索条件提供符合条件的数据。
- **TreeDelegate（委托类）**：处理节点的绘制和交互细节，将视图和数据分离，便于个性化显示。

---

## 示例代码 {#example-code}

```C++
#include <QAbstractItemModel>
#include <QSortFilterProxyModel>
#include <QStyledItemDelegate>
#include <QTreeView>

// DataModel: 提供原始数据
class DataModel : public QAbstractItemModel {
    Q_OBJECT
public:
    // 数据处理逻辑省略...
};

// TreeSearchModel: 数据筛选代理模型
class TreeSearchModel : public QSortFilterProxyModel {
    Q_OBJECT
public:
    explicit TreeSearchModel(QObject* pParent = nullptr) 
        : QSortFilterProxyModel(pParent) {
        setDynamicSortFilter(true);
    }

protected:
    // 重写过滤规则
    bool filterAcceptsRow(int nSourceRow, const QModelIndex &oSourceParent) const override {
        // 根据条件筛选数据
        return true; // 示例：默认通过
    }
};

// TreeDelegate: 自定义绘制和交互委托
class TreeDelegate : public QStyledItemDelegate {
    Q_OBJECT
public:
    explicit TreeDelegate(QObject* pParent = nullptr) : QStyledItemDelegate(pParent) {}

    void paint(QPainter* pPainter, const QStyleOptionViewItem &oOption, const QModelIndex &oIndex) const override {
        // 定制化节点绘制
        QStyledItemDelegate::paint(pPainter, oOption, oIndex);
    }
};

// TreeView: 树状视图
QTreeView* createTreeView(DataModel* pDataModel, TreeSearchModel* pProxyModel, TreeDelegate* pDelegate) {
    auto* pTreeView = new QTreeView;
    pProxyModel->setSourceModel(pDataModel); // 设置代理数据源
    pTreeView->setModel(pProxyModel);        // 设置树状视图的数据模型为代理模型
    pTreeView->setItemDelegate(pDelegate);    // 设置委托
    return pTreeView;
}
```

---

## 总结 {#summary}

- **代理（Proxy）** 主要用于数据的筛选和排序，使得 `TreeView` 展示的数据符合过滤条件，便于数据管理。
- **委托（Delegate）** 主要用于定制化视图中的元素展示和交互，管理每个节点的绘制和用户操作。