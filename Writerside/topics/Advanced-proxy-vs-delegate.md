# Advanced proxy-vs-delegate

## �߼�����֪ʶ���Ż�����չ {#advanced-development}

### Ŀ¼��չ {#index-extended}

- [����ģ���Ż�](#proxy-model-optimization)
- [ί����ĸ߼�����](#delegate-advanced-customization)
- [�������ڴ��Ż�](#performance-memory-optimization)
- [�߼�Ӧ�ã�Ƕ����״��ͼ](#nested-tree-view-advanced-use)

---

## ����ģ���Ż� {#proxy-model-optimization}

����ģ�͵����Ŀ����������ݴ���Ч�ʣ������Ǽ��ָ߼��Ż����ԣ�

1. **����������������**�����������ϴ��ģ�ͣ�����ģ�Ϳ���ͨ�����������������ؼ�����������Ҫչʾʱ�������ݡ����磬������ `TreeSearchModel` �У���չ��״̬��̬ɸѡ������ӽڵ����ݡ�

2. **������**���ڸ��ӳ����У�����ʹ�ö���������һ��������л���ɸѡ���ٵ���һ������������򣬴Ӷ�ʵ�ָ��ӵ����ݴ�����������ʾ�������ͨ�����������㴦�����ݡ�

3. **��̬���˹���**������ģ�Ϳ��Ը�����ͼ�еı仯��̬����ɸѡ�������磬�����ڴ�����ʹ��������ʽɸѡ���Ӷ�ʵ�ָ����Ĺ��ˡ�

```C++
class AdvancedFilterModel : public QSortFilterProxyModel {
    Q_OBJECT
public:
    explicit AdvancedFilterModel(QObject* pParent = nullptr) : QSortFilterProxyModel(pParent) {}

protected:
    bool filterAcceptsRow(int nSourceRow, const QModelIndex &oSourceParent) const override {
        // ��������ʾ��
        if (!oSourceParent.isValid()) {
            // �Զ���ڵ����Ԥ����
            return true;
        }
        return false; // ����������������ж�
    }
};
```

---

## ί����ĸ߼����� {#delegate-advanced-customization}

ί����ĸ߼���������ǿ����������������Ҫ���ã�

1. **�߼���������**������ί�������˫�����Ҽ��˵����¼���Ӧ�����磬��ÿ���ڵ����һ���Ҽ��˵������ڶ�̬�����ڵ����ݡ�

2. **�Զ���༭��**������Ϊ�ض��Ľڵ������ṩ�Զ���༭���������ڽڵ�����Ϊ��ɫʱ������ɫѡ������Ϊ�ˣ���д `createEditor` �� `setEditorData` �����������ί�и��½ڵ����ݡ�

3. **����Ч��**���ڽڵ�չ����ѡ��ʱ�����Ӷ���Ч�������絭�뵭����ƽ�������ȡ���� `QPropertyAnimation` ���������û����顣

```C++
class AdvancedTreeDelegate : public QStyledItemDelegate {
    Q_OBJECT
public:
    explicit AdvancedTreeDelegate(QObject* pParent = nullptr) : QStyledItemDelegate(pParent) {}

    QWidget* createEditor(QWidget* pParent, const QStyleOptionViewItem &oOption, const QModelIndex &oIndex) const override {
        if (oIndex.data().type() == QVariant::Color) {
            return new QColorDialog(oIndex.data().value<QColor>(), pParent);
        }
        return QStyledItemDelegate::createEditor(pParent, oOption, oIndex);
    }
};
```

---

## �������ڴ��Ż� {#performance-memory-optimization}

���������Ӵ����״��ͼ�У��������ڴ���Ż��ǳ���Ҫ��

1. **�������뻺�����**�����ڴ����ڵ����״�ṹ���ɿ��������غͻ�����ԣ�����չ���ڵ�ʱ�����ӽڵ㡣ͬʱ��ʹ���ڴ�ع����ڴ��Լ����ڴ���Ƭ��

2. **���ٲ���Ҫ���ź�-������**���ź�-�۵Ĺ������ӻ�Ӱ�����ܣ�ȷ�����ڱ�Ҫʱ������ͼ��

3. **�������ݸ���**����ģ�����ݸ���ʱ����������ڵ�Ĳ��롢ɾ���Ȳ������������������ܡ����磬QAbstractItemModel �ṩ�� `beginResetModel()` �� `endResetModel()` �������������ػ档

---

## �߼�Ӧ�ã�Ƕ����״��ͼ {#nested-tree-view-advanced-use}

��ʵ�ʿ����У�Ƕ����״��ͼ�Ǹ��� UI ����еĸ߼�Ӧ�ã�����Ƕ����Ŀ��ݹ����ݽṹչʾ��

1. **�ݹ����ģ��**������ʹ�õݹ����ģ�Ͷ�Ƕ�ײ�ν���ɸѡ�����򣬹���ÿ��Ƕ����ͼ������Դ������һ���ԡ�

2. **Ƕ����ͼ�Ķ�ί�����**���Բ�ͬ��Ƕ�ײ㼶�������ò�ͬ��ί�У��ֱ��������븸���Ľڵ�չʾ�뽻���߼����ṩ���ḻ���û����顣

3. **����ͬ����״̬����**�����Ƕ����ͼ�ڴ���ģ�ͼ䴫������ʱ����Ҫͬ��չ��״̬��ɸѡ����������ʹ��һ���������ݹ����������й���״̬��