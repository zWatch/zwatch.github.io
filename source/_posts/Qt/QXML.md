# XML

我得先看一下xml，

```xml
<?xml version="1.0" encoding="UTF-8"?>
```

第一行是 XML 声明，可选。
必须有一个根元素，该元素是其他元素的父元素。  
元素必须要关闭标签。  
大小写敏感。
必须正确嵌套。
属性的值必须加引号（类似于html），一个元素可以有多个属性，属性名不能冲突，也不能使用<>&。
实体引用（转义）【< > & ' "】这五种。
空格会被保留。
以换行符结束。

## CDATA

CDATA sections are used to escape blocks of text containing characters that would otherwise be regarded as markup. The only delimiter that is recognized in a CDATA section is the "]]&gt;" string that terminates the CDATA section. CDATA sections cannot be nested. Their primary purpose is for including material such as XML fragments, without needing to escape all the delimiters.  
CDATA节用于转义包含字符的文本块，否则这些字符将被视为标记。CDATA节中唯一可识别的分隔符是终止CDATA节的“]]>;”字符串。CDATA节不能嵌套。它们的主要目的是包含XML片段之类的材料，而不需要转义所有分隔符。  
(这段真是太难翻译了)  
regarded：视为
转义：escape【本意为逃脱】
shallow copy:浅拷贝
represents ：代表

## QDomCharacterData

浅拷贝，包括复制构造和赋值都是浅拷贝。


