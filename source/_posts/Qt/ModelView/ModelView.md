# 总结

先看Model/View Tutorial再看Model/View Programming
qt的model/view是
view提供视图
model提供数据接口，包括
要开启拖拽得在Model里实现mimeType/drop/drag等函数，也得在View里设置DragDropMode

## Model

所有项目模型均基于QAbstractItemModel类。 
此类定义一个接口，视图和委托使用该接口访问数据。 
数据本身不必存储在模型中。 它可以保存在由单独的类，文件，数据库或某些其他应用程序组件提供的数据结构或存储库中。

QAbstractItemModel提供了一个数据接口，该接口足够灵活，可以处理以表、列表和树的形式表示数据的视图。 
但是，当为列表和类似表的数据结构实现新模型时，QAbstractListModel和QAbstractTableModel类是更好的起点，因为它们提供了常用功能的适当默认实现。 这些类中的每一个都可以被子类化以提供支持特殊类型的列表和表的模型。

Qt provides some ready-made models that can be used to handle items of data:
QStringListModel is used to store a simple list of QString items.
QStandardItemModel manages more complex tree structures of items, each of which can contain arbitrary data.
QFileSystemModel provides information about files and directories in the local filing system.
QSqlQueryModel, QSqlTableModel, and QSqlRelationalTableModel are used to access databases using model/view conventions.

Qt提供了一些现成的模型，可用于处理数据项：
QStringListModel 用于存储QString项目的简单列表。
QStandardItemModel 管理更复杂的项目树结构，每个项目可以包含任意数据。
QFileSystemModel 提供有关本地归档系统中文件和目录的信息。
QSqlQueryModel，QSqlTableModel和QSqlRelationalTableModel 用于使用模型/视图约定访问数据库。

If these standard models do not meet your requirements, you can subclass QAbstractItemModel, QAbstractListModel, or QAbstractTableModel to create your own custom models. 
如果这些标准模型不满足您的要求，则可以将QAbstractItemModel，QAbstractListModel或QAbstractTableModel子类化以创建您自己的自定义模型。
你可以继承它们来实现自己的模型。

## Viewer

提供了针对各种视图的完整实现：QListView显示项目列表，QTableView显示表中模型的数据，QTreeView在层次结构列表中显示数据的模型项。 这些类均基于QAbstractItemView抽象基类。 尽管这些类是现成的实现，但也可以将它们子类化以提供自定义视图。

QStyledItemDelegate，这被Qt的标准视图用作默认委托。 但是，QStyledItemDelegate和QItemDelegate是绘画和为视图中的项目提供编辑器的独立替代方法。 它们之间的区别在于QStyledItemDelegate使用当前样式来绘制其项目。 因此，在实现自定义委托或使用Qt样式表时，建议将QStyledItemDelegate用作基类。

## Delegates

QAbstractItemDelegate是模型/视图框架中委托的抽象基类。 默认委托实现由QStyledItemDelegate提供，并且被Qt的标准视图用作默认委托。 但是，QStyledItemDelegate和QItemDelegate是绘画和为视图中的项目提供编辑器的独立替代方法。 它们之间的区别在于QStyledItemDelegate使用当前样式来绘制其项目。 因此，在实现自定义委托或使用Qt样式表时，建议将QStyledItemDelegate用作基类。

## Sorting

排序方式依赖于您的模型。

如果您的模型是可排序的，即重新实现`QAbstractItemModel::sort()`函数，则QTableView和QTreeView都提供了API，可让您以编程方式对模型数据进行排序。 另外，通过将`QHeaderView::sortIndicatorChanged()`信号连接到`QTableView::sortByColumn()`插槽或`QTreeView::sortByColumn()`，可以启用交互式排序（即允许用户通过单击视图的标题对数据进行排序）。 ）插槽。

如果您的模型没有所需的接口，或者如果您想使用列表视图来呈现数据，则另一种方法是在视图中呈现数据之前，使用代理模型来转换模型的结构。 有关代理模型的部分将对此进行详细介绍。

## 方便的类?

从标准视图类派生出许多便利类，以使依赖Qt基于项目的项目视图和表类的应用程序受益。 它们不打算被子类化（继承）。
