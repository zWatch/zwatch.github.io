# QClipboard

QClipboard类提供对窗口系统剪贴板的访问。 更多...

剪贴板提供了一种简单的机制，可以在应用程序之间复制和粘贴数据。
QClipboard支持与QDrag相同的数据类型，并使用类似的机制。 有关剪贴板的高级用法，请阅读拖放。
应用程序中只有一个QClipboard对象，可以通过QGuiApplication :: clipboard（）访问。

```c++
 QClipboard *clipboard = QGuiApplication::clipboard();
 QString originalText = clipboard->text();
 ...
 clipboard->setText(newText);
```

QClipboard具有一些访问常用数据类型的便利功能：setText()允许交换Unicode文本和`setPixmap()`，`setImage()`允许在应用程序之间交换QPixmap和QImage。 `setMimeData()`函数具有最大的灵活性：它允许您将任何QMimeData添加到剪贴板。 每个都有对应的吸气剂，例如 `text()`，`image()`和`pixmap()`。 您可以通过调用`clear()`清除剪贴板。

```c++
 void DropArea::paste()
 {
     const QClipboard *clipboard = QApplication::clipboard();
     const QMimeData *mimeData = clipboard->mimeData();

     if (mimeData->hasImage()) {
         setPixmap(qvariant_cast<QPixmap>(mimeData->imageData()));
     } else if (mimeData->hasHtml()) {
         setText(mimeData->html());
         setTextFormat(Qt::RichText);
     } else if (mimeData->hasText()) {
         setText(mimeData->text());
         setTextFormat(Qt::PlainText);
     } else {
         setText(tr("Cannot display data"));
     } 
```

Windows和macOS不支持全局鼠标选择。 它们仅支持全局剪贴板，即仅在进行显式复制或剪切时才将文本添加到剪贴板。
Windows和macOS没有所有权的概念。 剪贴板是完全全局的资源，因此会将更改通知所有应用程序。

通用Windows平台（UWP）仅允许在应用程序处于活动状态且应用程序窗口具有焦点的情况下查询剪贴板。 在后台访问剪贴板数据将由于拒绝访问而失败。