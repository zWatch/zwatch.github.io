# 委托

delegates提供绘图，操作model接口。
QStyledItemDelegate 是基于小部件实现的？

```c++
QWidget *createEditor(QWidget *parent, const QStyleOptionViewItem &option, const QModelIndex &index) const override;
void setEditorData(QWidget *editor, const QModelIndex &index) const override;
void setModelData(QWidget *editor, QAbstractItemModel *model, const QModelIndex &index) const override;
void updateEditorGeometry(QWidget *editor, const QStyleOptionViewItem &option, const QModelIndex &index) const override;
```

编辑部件将在需要时创建。

```c++
QWidget *SpinBoxDelegate::createEditor(QWidget *parent,
    const QStyleOptionViewItem &/* option */,
    const QModelIndex &/* index */) const
{
    QSpinBox *editor = new QSpinBox(parent);
    editor->setFrame(false);
    editor->setMinimum(0);
    editor->setMaximum(100);
    return editor;
}
```

然后设置编辑部件的值

```C++
void SpinBoxDelegate::setEditorData(QWidget *editor,
                                    const QModelIndex &index) const
{
    int value = index.model()->data(index, Qt::EditRole).toInt();
    QSpinBox *spinBox = static_cast<QSpinBox*>(editor);
    spinBox->setValue(value);
}
```

当用户编辑后

```C++
void SpinBoxDelegate::setModelData(QWidget *editor, QAbstractItemModel *model,
                                    const QModelIndex &index) const
{
    QSpinBox *spinBox = static_cast<QSpinBox*>(editor);
    spinBox->interpretText();
    int value = spinBox->value();
    model->setData(index, value, Qt::EditRole);
}
```

更新编辑部件的大小。Updating the editor's geometry

```C++
void SpinBoxDelegate::updateEditorGeometry(QWidget *editor,
                                            const QStyleOptionViewItem &option,
                                            const QModelIndex &/* index */) const
{
    editor->setGeometry(option.rect);
}
```

呈现多个元素的编辑器不会直接使用该grometry，它们会计算相对大小和位置（文档说的）。

编辑提示
编辑后，委托应向其他组件提供有关编辑过程的提示，并提供有助于任何后续编辑操作的提示。
这是通过发出带有适当提示的closeEditor（）信号来实现的。这是由默认的QStyledItemDelegate事件过滤器处理的，我们在构建数字调整框时安装了它。
我们可以通过自定义的事件过滤器来提高更加用户友好的编辑行为，比如发出带有EditNextItem提示的closeEditor信号来自动开启编辑视图的下一项。

另一种不需要使用事件过滤器的方法是 提供我们自己的编辑器小部件，为了方便可以继承QSpinBox。这种替代方法可以让我们以编写额外代码为代价，更好地控制编辑器小部件的行为。如果需要自定义标准Qt编辑器小部件的行为，那么在委托中安装事件过滤器通常更容易。

委托没有必要一定得发出这些提示，但不发出这些提示的委托在应用程序中的集成程度较低，而且比发出提示以支持常见编辑操作的委托更不可用。

## 选择模型

选择项目/当前项目
选择可以有多个，当前项目只有一个。当前项目可以被键盘和鼠标键所影响。
在操作选择时，可以将QItemSelectionModel看作一个选中项的记录。
项目视图类中使用的选择模型提供了基于模型/视图体系结构的功能的选择的一般描述。尽管用于操作选择的标准类对于所提供的项视图来说已经足够了，但是选择模型允许您创建专门的选择模型，以满足您自己的项模型和视图的需求。
在`QItemSelection`  

 ```C++
QModelIndex topLeft;
QModelIndex bottomRight;

topLeft = model->index(0, 0, QModelIndex());
bottomRight = model->index(5, 2, QModelIndex());
//To select these items in the model, and see the corresponding change in the table view, we need to construct a selection object then apply it to the selection model:
QItemSelection selection(topLeft, bottomRight);
selectionModel->select(selection, QItemSelectionModel::Select);
 ```

```C++
const QModelIndexList indexes = selectionModel->selectedIndexes();

for (const QModelIndex &index : indexes) {
    QString text = QString("(%1,%2)").arg(index.row()).arg(index.column());
    model->setData(index, text);
}
```

在这里应当使用rangr-based循环

`signal selectionChanged()` 参数包含两个选中项，一个是新选择的，另一个是新取消的。

`selectionModel->select(toggleSelection, QItemSelectionModel::Toggle);` 反选.
//如果元素其行或列,和选择元素toggleSelection中某些(个)元素的行或列相同,而它又不在toggleSelection中,那么它被选中.
//对于那些行或列都不在toggleSelection中的,本身也不在toggleSelection中的,依然不选中.

### 选中行或列

```C++
    //选中1/2两列
    QItemSelection columnSelection;
    topLeft = model->index(0, 1, QModelIndex());
    bottomRight = model->index(0, 2, QModelIndex());

    columnSelection.select(topLeft, bottomRight);
    selectionModel->select(columnSelection,
            QItemSelectionModel::Select | QItemSelectionModel::Columns);

    //选中0/1两行
    QItemSelection rowSelection;
    topLeft = model->index(0, 0, QModelIndex());
    bottomRight = model->index(1, 0, QModelIndex());

    rowSelection.select(topLeft, bottomRight);
    selectionModel->select(rowSelection,
    QItemSelectionModel::Select | QItemSelectionModel::Rows);
```

## 自定义模型

### 一个只读模型

```C++
class StringListModel : public QAbstractListModel
{
    Q_OBJECT
public:
    StringListModel(const QStringList &strings, QObject *parent = nullptr)
        : QAbstractListModel(parent), stringList(strings) {}
    int rowCount(const QModelIndex &parent = QModelIndex()) const override;
    QVariant data(const QModelIndex &index, int role) const override;
    QVariant headerData(int section, Qt::Orientation orientation,
                        int role = Qt::DisplayRole) const override;

private:
    QStringList stringList;
};
```

//请注意，这是一个非层次模型，因此我们不必担心父子关系。如果我们的模型是分层的，我们还必须实现index（）和parent（）函数。

//Well behaved models also implement headerData() to give tree and table views something to display in their headers.
//行为良好的模型还实现了headerData（），为树视图和表视图提供了在其标题中显示的内容。

```C++
 QVariant StringListModel::headerData(int section, Qt::Orientation orientation,
                                      int role) const
 {
     if (role != Qt::DisplayRole)
         return QVariant();

     if (orientation == Qt::Horizontal)
         return QStringLiteral("Column %1").arg(section);
     else
         return QStringLiteral("Row %1").arg(section);
 }
```

### 可编辑的模型

```C++
Qt::ItemFlags StringListModel::flags(const QModelIndex &index) const
{
    if (!index.isValid())
        return Qt::ItemIsEnabled;
    return QAbstractItemModel::flags(index) | Qt::ItemIsEditable;
}
```

对于每一个元素都可以有不同的flags，你可以改变它们让部分可写，部分不可写。

#### 插入和删除

```C++
bool insertRows(int position, int rows, const QModelIndex &index = QModelIndex()) override;
bool removeRows(int position, int rows, const QModelIndex &index = QModelIndex()) override;

bool StringListModel::insertRows(int position, int rows, const QModelIndex &parent)
{
    beginInsertRows(QModelIndex(), position, position+rows-1);
    for (int row = 0; row < rows; ++row) { stringList.insert(position, ""); }
    endInsertRows();
    return true;
}

bool StringListModel::removeRows(int position, int rows, const QModelIndex &parent)
{
    beginRemoveRows(QModelIndex(), position, position+rows-1);
    for (int row = 0; row < rows; ++row) { stringList.removeAt(position); }
    endRemoveRows();
    return true;
}
```

The model first calls the beginInsertRows() function to inform other components that the number of rows is about to change. The function specifies the row numbers of the first and last new rows to be inserted, and the model index for their parent item. After changing the string list, it calls endInsertRows() to complete the operation and inform other components that the dimensions of the model have changed, returning true to indicate success.
模型首先调用beginInsertRows（）函数，通知其他组件行数即将更改。该函数指定要插入的第一行和最后一行的行号，以及它们的父项的模型索引。更改字符串列表后，它调用endInsertRows（）来完成操作，并通知其他组件模型的维度已更改，返回true表示成功。  
**这里需要测试，尤其是关于树的**

## ListWidgets

QListWidget是从QListView继承下来的，同样的还有QTreeWidget和TableWidget。
单个级别的项目列表通常使用QListWidget和许多QListWidgetItems来显示。列表小部件的构造方式与任何其他小部件相同：
`QListWidget *listWidget = new QListWidget(this);`
List items can be added directly to the list widget when they are constructed:

```C++
new QListWidgetItem(tr("Sycamore"), listWidget);
new QListWidgetItem(tr("Chestnut"), listWidget);
new QListWidgetItem(tr("Mahogany"), listWidget);
```

They can also be constructed without a parent list widget and added to a list at some later time:
它们也可以在没有父元素（listWidget）的情况下构造，随后再加入到listWidget中。
QListWidgetItem *newItem = new QListWidgetItem;
newItem->setText(itemText);
listWidget->insertItem(row, newItem);

Each item in a list can display a text label and an icon.  
每一个item都可以展示一个text 标题和一个icon。颜色和字体也可以被改变。Tooltips， status tips， and “What's This”  

## Tree widgets

树或项目的层次列表是由QTreeWidget和QTreeWidgetItem类提供的。树小部件中的每个项都可以有自己的子项，并且可以显示许多列的信息。树小部件的创建与任何其他小部件一样  
`QTreeWidget *treeWidget = new QTreeWidget(this);`  
在将项添加到树小部件之前，必须设置列数。例如，我们可以定义两个列，并创建一个标题，在每个列的顶部提供标签：  

```C++
    treeWidget->setColumnCount(2);
    QStringList headers;
    headers << tr("Subject") << tr("Default");
    treeWidget->setHeaderLabels(headers);
```

为每个部分设置标签的最简单方法是提供一个字符串列表。对于更复杂的标题，您可以构造一个树项，根据需要对其进行装饰，并将其用作树小部件的头。  
树小部件中的顶层项是用树小部件作为其父小部件构建的。它们可以按任意顺序插入，也可以通过在构造每个项时指定前一项来确保它们按特定顺序列出：

```C++
QTreeWidgetItem *cities = new QTreeWidgetItem(treeWidget);
cities->setText(0, tr("Cities"));
QTreeWidgetItem *osloItem = new QTreeWidgetItem(cities);
osloItem->setText(0, tr("Oslo"));
osloItem->setText(1, tr("Yes"));

QTreeWidgetItem *planets = new QTreeWidgetItem(treeWidget, cities);
```

树窗口小部件处理顶层项目的方式与树内部的其他项目略有不同。可以通过调用树小部件的takeTopLevelItem（）函数从树的顶层删除项，但通过调用其父项的takeChild（）函数，可以从较低级别删除项。使用insertTopLevelItem（）函数将项插入树的顶层。在树的较低级别，使用父项的insertChild（）函数。

### 删除

```C++
QTreeWidgetItem *parent = currentItem->parent();
int index;

if (parent) {
    index = parent->indexOfChild(treeWidget->currentItem());
    delete parent->takeChild(index);
} else {
    index = treeWidget->indexOfTopLevelItem(treeWidget->currentItem());
    delete treeWidget->takeTopLevelItem(index);
}
```

### 插入

Inserting the item somewhere else in the tree widget follows the same pattern:

```C++
QTreeWidgetItem *parent = currentItem->parent();
QTreeWidgetItem *newItem;
if (parent)
    newItem = new QTreeWidgetItem(parent, treeWidget->currentItem());
else
    newItem = new QTreeWidgetItem(treeWidget, treeWidget->currentItem());
```

## Table widgets

使用QTableWidget和QTableWidgetItem构造类似于电子表格应用程序中找到的项目的表。 这些提供了带有标题和要在其中使用的项目的滚动表窗口小部件。  
可以创建具有一定数量的行和列的表，也可以根据需要将它们添加到未调整大小的表中。  

```C++
QTableWidget *tableWidget;
tableWidget = new QTableWidget(12, 3, this);
QTableWidgetItem *newItem = new QTableWidgetItem(tr("%1").arg(
                                                pow(row, column+1)));
tableWidget->setItem(row, column, newItem);
```

通过在表外部构造项目并将其用作标题，可以将水平和垂直标题添加到表中：

```C++
QTableWidgetItem *valuesHeaderItem = new QTableWidgetItem(tr("Values"));
tableWidget->setHorizontalHeaderItem(0, valuesHeaderItem);
```

## Common features

### Hidden items

It is sometimes useful to be able to hide items in an item view widget rather than remove them. Items for all of the above widgets can be hidden and later shown again. You can determine whether an item is hidden by calling the isItemHidden() function, and items can be hidden with setItemHidden().
Since this operation is item-based, the same function is available for all three convenience classes.
可以隐藏某个item,`setItemHidden()` `isItemHidden()`

### Selections选择

#### Single item selections 单选  

只能选一个

### Multi-item selections 复选  

可选中多个

### Extended selections 扩展

通常需要选择许多相邻项目的小部件（例如在电子表格中找到的小部件）需要ExtendedSelection模式。 在这种模式下，可以用鼠标和键盘选择窗口小部件中连续的项目范围。 如果使用修饰键，则还可以创建涉及许多与控件中其他选定项目不相邻的项目的复杂选择。  
如果用户在不使用修饰键的情况下选择了项目，则会清除现有选择。  

**注意** 对于单选模式，当前项目将在选择中。 在多选和扩展选择模式下，取决于用户形成选择的方式，当前项目可能不在选择之内。  

## 拖拽

模型/视图框架完全支持Qt的拖放基础结构。 列表，表和树中的项目可以在视图中拖动，并且数据可以作为MIME编码的数据导入和导出。  
( Multipurpose Internet Mail Extensions MIME是一种保证非ASCII码文件在internet上传播的规格。)  

默认情况下，与QListWidget，QTableWidget和QTreeWidget一起使用的每种项目类型都配置为使用不同的标志集。 例如，每个QListWidgetItem或QTreeWidgetItem最初都是启用的，可检查的，可选的，并且可用作拖放操作的源。 每个QTableWidgetItem也可以进行编辑，并用作拖放操作的目标。

尽管所有标准项都为拖放设置了一个或两个标志，但是您通常需要在视图本身中设置各种属性，以利用对拖放的内置支持：

+ To enable item dragging, set the view's dragEnabled property to true.
+ To allow the user to drop either internal or external items within the view, set the view's viewport()'s acceptDrops property to true.
+ To show the user where the item currently being dragged will be placed if dropped, set the view's showDropIndicator property. This provides the user with continuously updating information about item placement within the view.

For example, we can enable drag and drop in a list widget with the following lines of code:
例如，我们可以使用以下代码行在列表小部件中拖放：

```C++
 QListWidget *listWidget = new QListWidget(this);
 listWidget->setSelectionMode(QAbstractItemView::SingleSelection);
 listWidget->setDragEnabled(true);
 listWidget->viewport()->setAcceptDrops(true);
 listWidget->setDropIndicatorShown(true);
```

To enable the user to move the items around within the view, we must set the list widget's dragDropMode:  
为了使用户能够在视图内移动项目，我们必须设置列表小部件的dragDropMode：  
`listWidget->setDragDropMode(QAbstractItemView::InternalMove);`  

设置拖放视图的方式与便捷视图相同。 例如，可以使用与QListWidget相同的方式来设置QListView：

```C++
 QListView *listView = new QListView(this);
 listView->setSelectionMode(QAbstractItemView::ExtendedSelection);
 listView->setDragEnabled(true);
 listView->setAcceptDrops(true);
 listView->setDropIndicatorShown(true);
```

由于对视图显示的数据的访问由模型控制，因此所使用的模型还必须提供对拖放操作的支持。 可以通过重新实现`QAbstractItemModel::supportedDropActions()`函数来指定模型支持的动作。  
例如，使用以下代码启用复制和移动操作：

```C++
Qt::DropActions DragDropListModel::supportedDropActions() const
{
    return Qt::CopyAction | Qt::MoveAction;
}
```

尽管可以给出Qt :: DropActions中值的任意组合，但是需要编写模型来支持它们。 例如，要允许Qt :: MoveAction与列表模型一起正确使用，该模型必须直接或通过从其基类继承实现来提供QAbstractItemModel :: removeRows（）的实现。

### Enabling drag and drop for items

通过重新实现 `QAbstractItemModel::flags()` 函数以提供适当的标志，模型向视图指示可以拖动哪些项目，哪些将接受放置。

For example, a model which provides a simple list based on QAbstractListModel can enable drag and drop for each of the items by ensuring that the flags returned contain the Qt::ItemIsDragEnabled and Qt::ItemIsDropEnabled values:  
例如，一个基于QAbstractListModel的简单列表的模型可以通过提供包含`Qt::ItemIsDragEnabled`和`Qt::ItemIsDropEnabled`值的标志来启用每个项目的拖放操作：

```C++
Qt::ItemFlags DragDropListModel::flags(const QModelIndex &index) const
{
    Qt::ItemFlags defaultFlags = QStringListModel::flags(index);

    if (index.isValid())
        return Qt::ItemIsDragEnabled | Qt::ItemIsDropEnabled | defaultFlags;
    else
        return Qt::ItemIsDropEnabled | defaultFlags;
}
```

请注意，可以将项目拖放到模型的顶层，但是仅对有效项目启用拖动。(大概是这段代码只对有效的index启动拖拽)。  

### 编码导出的数据

当通过拖放操作从模型中导出数据项时，会将它们编码为与一种或多种MIME类型相对应的适当格式。 通过重新实现QAbstractItemModel :: mimeTypes（）函数，并返回标准MIME类型的列表，模型声明了可用于提供项目的MIME类型。  
例如，仅提供纯文本的模型将提供以下实现：  

```C++
QStringList DragDropListModel::mimeTypes() const
{
    QStringList types;
    types << "application/vnd.text.list";
    return types;
}
```

以下代码显示了与给定索引列表相对应的每一项数据如何被编码为纯文本并存储在QMimeData对象中。  

```C++
 QMimeData *DragDropListModel::mimeData(const QModelIndexList &indexes) const
 {
     QMimeData *mimeData = new QMimeData;
     QByteArray encodedData;

     QDataStream stream(&encodedData, QIODevice::WriteOnly);

     for (const QModelIndex &index : indexes) {
         if (index.isValid()) {
             QString text = data(index, Qt::DisplayRole).toString();
             stream << text;
         }
     }

     mimeData->setData("application/vnd.text.list", encodedData);
     return mimeData;
 }
```

由于向函数提供了模型索引列表，因此该方法足够通用，可以在分层模型和非分层模型中使用。  
请注意，必须将自定义数据类型声明为元对象，并且必须为其实现流运算符。 有关详细信息，请参见QMetaObject类描述。  

### 将拖拽的数据加入模型

不同类型的模型倾向于以不同方式处理拖拽的数据。 列表和表模型仅提供用于存储数据项的平面结构。 因此，当将数据放置到视图中的现有项目上时，他们可能会插入新的行（和列），或者可能会使用提供的某些数据覆盖模型中项目的内容。 树模型通常能够将包含新数据的子项添加到其基础数据存储中，因此就用户而言，其行为更具可预测性。  

拖拽的数据由模型的`QAbstractItemModel::dropMimeData()`重新实现来处理。 例如，处理字符串的简单列表的模型可以提供一种实现，该实现将拖拽到现有项目上的数据与拖拽到模型顶层（即到无效项目）的数据分开处理。  
通过重新实现`QAbstractItemModel::canDropMimeData()`，模型可以禁止删除某些项目，或者取决于拖拽的数据。  

该模型首先必须确保执行操作，所提供的数据采用可以使用的格式，并且其在模型中的目的地是有效的：

```C++
 bool DragDropListModel::canDropMimeData(const QMimeData *data,
     Qt::DropAction action, int row, int column, const QModelIndex &parent)
 {
     Q_UNUSED(action);
     Q_UNUSED(row);
     Q_UNUSED(parent);

     if (!data->hasFormat("application/vnd.text.list"))
         return false;

     if (column > 0)
         return false;

     return true;
 }
```

```C++
bool DragDropListModel::dropMimeData(const QMimeData *data,
     Qt::DropAction action, int row, int column, const QModelIndex &parent)
 {
    if (!canDropMimeData(data, action, row, column, parent))
        return false;

    if (action == Qt::IgnoreAction)
        return true;

    //根据要插入模型的数据是否被放到现有项目上，其处理方式有所不同。 在这个简单的示例中，我们希望允许在现有项目之间，列表中的第一个项目之前以及最后一个项目之后进行放置。
    //发生放置时，对应于父项的模型索引将是有效的（指示该放置发生在某个项目上），或者将是无效的（指示该放置发生在视图中与模型顶层相对应的某处） 。
    int beginRow;
    if (row != -1)
        beginRow = row;
    //最初，我们检查提供的行号，以查看是否可以使用它向模型中插入项目，而不管父索引是否有效。
    else if (parent.isValid())
        beginRow = parent.row();
    //如果父模型索引有效，则该拖拽发生在项目上。 在这个简单的列表模型中，我们找出项目的行号，并使用该值将拖拽的项目插入模型的顶层。
    else
        beginRow = rowCount(QModelIndex());
    //当视图中的其他位置发生拖放并且行号不可用时，我们会将项目追加到模型的顶层。

    /*
    **WARING:**在分层模型中，当某项发生拖拽时，最好将新项作为该项的子项插入模型中。 在此处显示的简单示例中，模型只有一个级别，因此此方法不合适。
    */
    QByteArray encodedData = data->data("application/vnd.text.list");
    QDataStream stream(&encodedData, QIODevice::ReadOnly);
    QStringList newItems;
    int rows = 0;

    while (!stream.atEnd()) {
        QString text;
        stream >> text;
        newItems << text;
        ++rows;
    }
    insertRows(beginRow, rows, QModelIndex());
    for (const QString &text : qAsConst(newItems)) {
        QModelIndex idx = index(beginRow, 0, QModelIndex());
        setData(idx, text);
        beginRow++;
    }

    return true;
}
```

### Decoding imported data 解码导入的数据

```C++
while (!stream.atEnd()) {
    QString text;
    stream >> text;
    newItems << text;
    ++rows;
}

insertRows(beginRow, rows, QModelIndex());
for (const QString &text : qAsConst(newItems)) {
    QModelIndex idx = index(beginRow, 0, QModelIndex());
    setData(idx, text);
    beginRow++;
}
```

请注意，该模型通常将需要提供QAbstractItemModel :: insertRows（）和QAbstractItemModel :: setData（）函数的实现。  

## Proxy Models 代理模型

在模型/视图框架中，单个模型提供的数据项可以被任意数量的视图共享，并且每个视图可能以完全不同的方式表示相同的信息。   
自定义视图和委托是提供完全不同的相同数据表示形式的有效方法。 但是，应用程序通常需要提供对相同数据的处理版本的常规视图，例如对项列表的不同排序视图。  

可以在现有模型和任意数量的视图之间插入代理模型。 Qt提供了一个标准的代理模型QSortFilterProxyModel，该模型通常被实例化并直接使用，但是也可以被子类化以提供自定义的过滤和排序行为。 QSortFilterProxyModel类可以按以下方式使用：  

```C++
QSortFilterProxyModel *filterModel = new QSortFilterProxyModel(parent);
filterModel->setSourceModel(stringListModel);

QListView *filteredView = new QListView;
filteredView->setModel(filterModel);
```

由于代理模型是从QAbstractItemModel继承的，因此它们可以连接到任何类型的视图，并且可以在视图之间共享。 它们还可以用于处理从管道布置中的其他代理模型获得的信息。  
QSortFilterProxyModel类旨在实例化并直接在应用程序中使用。 通过子类化此类并实现所需的比较操作，可以创建更多专业的代理模型。  

## Customizing proxy models

通常，代理模型中使用的处理类型涉及将每一项数据从源模型中的原始位置映射到代理模型中的不同位置。   
在某些模型中，某些项目在代理模型中可能没有对应的位置； 这些模型是过滤代理模型。 使用代理模型提供的模型索引查看访问项，这些索引不包含有关源模型或该模型中原始项的位置的信息。  

QSortFilterProxyModel允许在将源模型中的数据提供给视图之前对其进行过滤，并且还允许将源模型的内容作为预排序的数据提供给视图。  

### 过滤代理

QSortFilterProxyModel的子类可以重新实现两个虚拟函数，只要请求或使用来自代理模型的模型索引，就会调用这两个虚函数：  
`filterAcceptsColumn（）`用于过滤部分源模型中的特定列。  
`filterAcceptsRow（）`用于过滤部分源模型中的特定行。  
返回true表示接受该行或列,false表示过滤该行或列.

### 排序代理

QSortFilterProxyModel实例使用std :: stable_sort（）函数在源模型中的项目和代理模型中的项目之间设置映射，从而允许将分类的项目层次结构公开给视图，而无需修改源模型的结构。 要提供自定义排序行为，请重新实现lessThan（）函数以执行自定义比较。

## 继承Model，自定义模型

模型子类需要提供QAbstractItemModel基类中定义的许多虚函数的实现。  
需要实现的这些功能的数量取决于模型的类型-它是否为视图提供简单的列表，表还是复杂的项目层次结构。  
从QAbstractListModel和QAbstractTableModel继承的模型可以利用这些类提供的函数的默认实现。  
以树状结构显示数据项的模型必须为QAbstractItemModel中的许多虚拟函数提供实现。  

在模型子类中需要实现的功能可以分为三类：  

+ 项目数据处理：所有模型都需要实现一些功能，以使视图和委托能够查询模型的尺寸，检查项目并检索数据。
+ 导航和索引创建：层次模型需要提供视图可以调用的功能，以导航它们公开的树状结构，并获取项目的模型索引。
+ 拖放支持和MIME类型处理：模型继承了控制内部和外部拖放操作执行方式的函数。 这些功能允许使用其他组件和应用程序可以理解的MIME类型来描述数据项。

### Item data handling 项目数据处理

#### Read-Only access

+ flags(): 由其他组件用于获取有关模型提供的每个项目的信息。 在许多模型中，标志的组合应包括`Qt::ItemIsEnabled`和`Qt::ItemIsSelectable`。
+ data(): 用于向视图和委托提供项目数据。 通常，模型只需要为`Qt::DisplayRole`和任何特定于应用程序的用户角色提供数据，但是为`Qt::ToolTipRole`，`Qt::AccessibleTextRole`和`Qt::AccessibleDescriptionRole`提供数据也是一种好习惯。 有关与每个角色关联的类型的信息，请参见Qt :: ItemDataRole枚举文档。
+ headerData(): 提供具有在标题中显示的信息的视图。 该信息仅由可显示标题信息的视图检索。
+ rowCount(): Provides the number of rows of data exposed by the model.提供模型公开的数据行数。

1**这里省略一段，需要用到时再看吧**

## Performance optimization for large amounts of data



