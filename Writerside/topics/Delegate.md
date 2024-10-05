# Delegate

����Delegate���� Qt �� **Model-View-Delegate** (MVD) �ܹ��а����˷ǳ���Ҫ�Ľ�ɫ��������Ҫ����������ͼ (View) ��ģ�� (Model) ֮�䣬����������չʾ�ͱ༭���ݡ���ˣ����ֻ�Ǵ���򵥵����ݺ���ʾ���󣬴�������ÿ����Եò����ԡ����ڴ����ӵ���ʾ�����Զ���༭�����û�����ʱ��������������÷ǳ��ؼ���ǿ��

�����ҽ��Ӵ���Ļ������������;���Լ��߼���������ϸ���۴������Ҫ�ԡ�

## ����Delegate���Ļ�������

### ����������������
Qt Ĭ��ʹ���� `QStyledItemDelegate` ������ģ�����ݵ���ʾ�ͱ༭�����������������ÿ����Ԫ�����Ⱦ�ͱ༭�߼��������������Զ���ÿ����Ԫ���������ʾ���Լ����û��༭�õ�Ԫ��ʱ�����ֵı༭����

����ĺ������������
- **�Զ�����Ⱦ**��ͨ����������Ըı䵥Ԫ�����ݵ���ʾ��ʽ������Ϊ�ض����С��С����������Ͷ�����ʾ���
- **�Զ���༭��**�������ͨ������Ϊÿ����Ԫ���ṩ��ͬ�ı༭���������ı��������˵�������ѡ�����ȡ�

����ͨ��ͨ���̳� `QStyledItemDelegate` ��ʵ�֣���д�����ؼ��ĺ�����������Ⱦ�ͱ༭��Ϊ��
1. `paint()`: ���Ƶ�Ԫ��Ļ��Ʒ�ʽ��
2. `createEditor()`: �����Զ���ı༭����
3. `setEditorData()`: ���ñ༭���еĳ�ʼ���ݡ�
4. `setModelData()`: ���༭���е�����д��ģ�͡�
5. `updateEditorGeometry()`: ���ñ༭���Ĵ�С��λ�á�

---

## ������;���Զ�����Ⱦ��༭ {#Common-Usage}

### ����������������
��Ĭ������£�Qt ��ͼ�еĵ�Ԫ��ֻ���Լ򵥵��ı���ʽ��ʾ���ݣ������ĳЩ���ӵĳ����ǲ����ġ��������������Զ������ݵ���ʾ��ʽ�������ڱ���л�����ɫ��ͼ�ꡢ��������Ԫ�ء����⣬����Ҫ����ı༭��ʱ������ͨ�������˵�ѡ��ֵ������ѡ������������ʱ������Ҳ���ṩ�����Ķ��ƹ��ܡ�

### �ؼ��㣺
- **�Զ������ݵ���ʾ��ʽ**��ͨ����д `paint()` ��������������ڵ�Ԫ������Ⱦ���ӵ����ݣ���ͼ����ɫ��ͼ��ȡ�
- **�Զ���༭��**��ͨ�� `createEditor()` ����������Ϊÿ����Ԫ���ṩ��ͬ������ؼ������������򡢸�ѡ������ѡ�����ȡ�

### ����ʾ�����Զ�����Ⱦ�ͱ༭������

```C++
class CustomDelegate : public QStyledItemDelegate {
public:
    CustomDelegate(QObject *parent = nullptr) : QStyledItemDelegate(parent) {}

    // �Զ�����Ⱦ
    void paint(QPainter *painter, const QStyleOptionViewItem &option, const QModelIndex &index) const override {
        if (index.column() == 1) {
            int progress = index.data().toInt();  // ��ģ���л�ȡ����
            QStyleOptionProgressBar progressBarOption;
            progressBarOption.rect = option.rect;
            progressBarOption.minimum = 0;
            progressBarOption.maximum = 100;
            progressBarOption.progress = progress;
            progressBarOption.text = QString("%1%").arg(progress);
            progressBarOption.textVisible = true;
            QApplication::style()->drawControl(QStyle::CE_ProgressBar, &progressBarOption, painter);
        } else {
            QStyledItemDelegate::paint(painter, option, index);  // Ĭ�ϻ��Ʒ�ʽ
        }
    }

    // �Զ���༭��
    QWidget *createEditor(QWidget *parent, const QStyleOptionViewItem &option, const QModelIndex &index) const override {
        if (index.column() == 1) {
            QSlider *editor = new QSlider(parent);
            editor->setOrientation(Qt::Horizontal);
            editor->setRange(0, 100);
            return editor;
        }
        return QStyledItemDelegate::createEditor(parent, option, index);  // Ĭ�ϱ༭��
    }

    void setEditorData(QWidget *editor, const QModelIndex &index) const override {
        int value = index.data().toInt();
        QSlider *slider = static_cast<QSlider*>(editor);
        slider->setValue(value);
    }

    void setModelData(QWidget *editor, QAbstractItemModel *model, const QModelIndex &index) const override {
        QSlider *slider = static_cast<QSlider*>(editor);
        int value = slider->value();
        model->setData(index, value, Qt::EditRole);
    }

    void updateEditorGeometry(QWidget *editor, const QStyleOptionViewItem &option, const QModelIndex &index) const override {
        editor->setGeometry(option.rect);  // ���ñ༭���Ĵ�С
    }
};
```

�����������У�����ʵ���������ܣ�
1. **�Զ�����Ⱦ**���ڶ�����ʾ��������������ͨ�ı���
2. **�Զ���༭��**������ڶ��еĵ�Ԫ��ʱ����ʾ������������Ĭ�ϵ��ı������ڵ�����������ֵ��

### �ؼ��ԸĽ���
- ʹ�ô���ʹ�ñ�����ݸ��߽����Ժ������ԣ����������ͼ�ꡢ��ɫ��ʾ����
- �ṩ�û��Ѻõ��������飨�绬�����������˵��ȣ���

---

## �߼����������ӵ����ݽ��� {#Advanced-Delegate-Usage}

### ����������������
��ʵ�ʿ����У����������Զ��ֹ�ڼ򵥵���Ⱦ�ͱ༭�����������Դ����ӵ��û�����������Ż����ܡ����磺
- **���νṹ�еĸ��ӱ༭**��ͨ�����������νṹ��ÿ���ڵ����ʾ�ͱ༭��Ϊ��
- **��ͬ��ʹ�ò�ͬ���͵ı༭��**����ͬ���п���ʹ�ò�ͬ�ı༭�ؼ������ı�������ѡ��������ѡ��ȣ���
- **�����Ż�**���ڴ������ݼ��У�����ͨ������ʵ�ָ�Ч����Ⱦ�ͽ��������ٲ���Ҫ���ػ����Դռ�á�

### �ؼ��㣺
- **���νṹ�еĴ���Ӧ��**��������Թ��� `QTreeView` �и��ӵĽڵ���ʾ�ͱ༭����ȷ��ÿ���ڵ��ж��ƵĽ�����Ϊ��
- **�첽���ݼ���**����������� `QAbstractItemModel` һ����ϣ�ʵ���첽���ݵļ��غ���ʾ����߸��ӽ�������ܡ�

### ����ʾ������ͬ��ʹ�ò�ͬ�༭������

```C++
class CustomDelegate : public QStyledItemDelegate {
public:
    CustomDelegate(QObject *parent = nullptr) : QStyledItemDelegate(parent) {}

    QWidget *createEditor(QWidget *parent, const QStyleOptionViewItem &option, const QModelIndex &index) const override {
        if (index.column() == 0) {
            QLineEdit *editor = new QLineEdit(parent);  // ��һ�����ı���
            return editor;
        } else if (index.column() == 1) {
            QDateEdit *editor = new QDateEdit(parent);  // �ڶ���������ѡ����
            editor->setCalendarPopup(true);
            return editor;
        } else {
            return QStyledItemDelegate::createEditor(parent, option, index);  // Ĭ�ϱ༭��
        }
    }
};
```

�����ʾ���У����Ǹ����еĲ�ͬΪ�û��ṩ��ͬ������ؼ�����ǿ�˱�������ԡ�

---

## �����ǿ��֮���ܽ� {#Delegate-Summary}

�����������Ҫ���������¼������棺
1. **�Զ�����Ⱦ**����������������ȫ���������ʾÿ����Ԫ������ݣ�֧��ͼƬ��ͼ�ꡢ��ɫ���������ȸ��ָ��ӵ���Ⱦ����
2. **�Զ���༭��**���������Ϊÿ����Ԫ���ƽ�����ʽ��Ϊ�û��ṩ�ḻ���������飬֧���ı��򡢸�ѡ������ѡ�������������ȶ��ֱ༭����
3. **�����û�����**��ͨ������ʵ��ֱ�۵� UI �������绬����������ֵ�����ͼƬ�༭��˫����Ԫ�񵯳��Զ���������ȡ�
4. **�Ż�����**�������������Ǹ�����Ҫ�ӳټ������ݣ����ٲ���Ҫ����Ⱦ����� UI ����Ӧ�ٶȡ�
5. **����ҵ������**������Ҫ�����ض��߼����������������⴦��ʱ�������ṩ��ϸ���ȵĿ��ƣ�ȷ�����ӵ��û�����õ����㡣

### �ܽ᣺
�����ڼ򵥵ĳ����´����������п��ޣ����ڴ����ӵ���ʾ���༭�ͽ�������ʱ����������ü�Ϊ�ؼ����������ܹ��������չʾ������ԣ�����Ϊ�û��ṩ���Ѻá�ֱ�۵Ľ������顣��ˣ���Ⲣ���ô��������� Qt MVD �ܹ��е�һ����Ҫ���裬�ر����ڿ������ӵ��û�����ʱ������Ĺ��ܲ��ɺ��ӡ�