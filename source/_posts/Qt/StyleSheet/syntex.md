# 语法

`QPushButton {color:red}`

`QPushButton, QLineEdit, QComboBox {color：red}`

`QPushButton {color：red; background-color：white}`

前边的部分称为选择器

+ 通用选择器 *
+ 类型选择器 QPushButton
+ 属性选择器 QPushButton[flat="false"]
+ 类型选择器 .QPushButton
+ ID选择器 QPushButton#okButton
+ 后代选择器 QDialog QPushButton
+ 直接后代选择器 QDialog > QPushButton

## 子控件

对于样式复杂的窗口小部件，必须访问窗口小部件的子控件，如QComboBox的下拉按钮或QSpinBox的向上和向下箭头。  
选择器可能包含子控件，从而可以将规则的应用限制到特定的小部件的子控件。  
`QComboBox::drop-down { image: url(dropdown{ image: url(dropdown.png) }) }`
