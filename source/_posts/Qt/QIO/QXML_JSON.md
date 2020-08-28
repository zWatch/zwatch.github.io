# DataInterface

## XML

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

### CDATA

CDATA sections are used to escape blocks of text containing characters that would otherwise be regarded as markup. The only delimiter that is recognized in a CDATA section is the "]]&gt;" string that terminates the CDATA section. CDATA sections cannot be nested. Their primary purpose is for including material such as XML fragments, without needing to escape all the delimiters.  
CDATA节用于转义包含字符的文本块，否则这些字符将被视为标记。CDATA节中唯一可识别的分隔符是终止CDATA节的“]]>;”字符串。CDATA节不能嵌套。它们的主要目的是包含XML片段之类的材料，而不需要转义所有分隔符。  
(这段真是太难翻译了)  
regarded：视为
转义：escape【本意为逃脱】
shallow copy:浅拷贝
represents ：代表
术语 CDATA 指的是不应由 XML 解析器进行解析的文本数据（Unparsed Character Data）。

在 XML 元素中，"<" 和 "&" 是非法的。

"<" 会产生错误，因为解析器会把该字符解释为新元素的开始。

"&" 也会产生错误，因为解析器会把该字符解释为字符实体的开始。

某些文本，比如 JavaScript 代码，包含大量 "<" 或 "&" 字符。为了避免错误，可以将脚本代码定义为 CDATA。

CDATA 部分中的所有内容都会被解析器忽略。

CDATA 部分由 "<![CDATA[" 开始，由 "]]>" 结束：
**关于 CDATA 部分：**
CDATA 部分不能包含字符串 "]]>"。也不允许嵌套的 CDATA 部分。
标记 CDATA 部分结尾的 "]]>" 不能包含空格或折行。

```html
<script>
<![CDATA[
function matchwo(a,b)
{
if (a < b && a < 0) then
  {
  return 1;
  }
else
  {
  return 0;
  }
}
]]>
</script>
```

### QDomCharacterData

浅拷贝，包括复制构造和赋值都是浅拷贝。

### QXMLStreamWriter

### QXMLStreamReader

流程是

```C++
while (!xmlreader.atEnd())
{
    switch (xmlreader.readNext())
    {
    case QXmlStreamReader::StartDocument:
        qDebug() << "StartDocument :: isStandaloneDocument: " << xmlreader.isStandaloneDocument();
        break;
    case QXmlStreamReader::EndDocument:
        qDebug() << "EndDocument :: isStandaloneDocument: " << xmlreader.isStandaloneDocument();
        break;
    case QXmlStreamReader::Characters:
        qDebug() << "Characters: ";
        //qDebug() << " name :" << xmlreader.name();
        //text(), isWhitespace(), isCDATA()
        if (xmlreader.isCDATA())
        {
            qDebug() << "   isCDATA: true";
        }
        else if (xmlreader.isWhitespace())
        {
            qDebug() << "   isWhitespace: true";
        }
        else
        {
            qDebug() << "   text :" << xmlreader.text();
        }
        break;
    case QXmlStreamReader::StartElement:
        qDebug() << "StartElement: ";
        qDebug() << "   name :"<< xmlreader.name();
        qDebug() << "   namespaceUri :" << xmlreader.namespaceUri();
        if (xmlreader.attributes().size()>0)
        {
            for (auto i: xmlreader.attributes())
            {
                qDebug() << "   attributes " << i.name() << "=" << i.value();
            }
        }
        else
        {
            qDebug() << "   attributes : no";
        }

        if (xmlreader.namespaceDeclarations().size() > 0)
        {
            for (auto i : xmlreader.namespaceDeclarations())
            {
                qDebug() << "   namespaceDeclarations " << i.prefix() << i.namespaceUri();
            }
        }
        else
        {
            qDebug() << "   namespaceDeclarations : no";
        }
        break;
    case QXmlStreamReader::EndElement:
        qDebug() << "EndElement: ";
        qDebug() << "   namespaceUri :" << xmlreader.namespaceUri();
        qDebug() << "   name :" << xmlreader.name();
        //namespaceUri(),name()  
        break;
    }
}
```

遇到无法识别元素的可以使用readNextStartElement跳过。

读到的标识符 和 可用的函数

记号类型        示例            getter 函数

+ StartDocument           N/A             isStandaloneDocument()
+ EndDocument             N/A             isStandaloneDocument()  
+ StartElement            \<item>         namespaceUri(),name(),attributes(),namespaceDeclarations()  
+ EndElement              \</item>        namespaceUri(),name()  
+ Characters              AT&;T           text(),isWhitespace(),isCDATA()  
+ Comment                 <! i–fix -->    text()  
+ DTD                     <!DOCTYPE…>     text(),notationDeclarations(),entityDeclarations()  
+ EntityReference         &trade;         name()，text()  
+ ProcessingInstruction   <？alert?>      processingInstructionTarget()，processingInstructionData()  
+ Invalid                 >&amp<          error()，errorstring()  

### isStandaloneDocument

是否有且唯一的xml文档。

### namespaceUri

在html中，同一标识符可以多次出现，不会冲突。

```html
<h:table xmlns:h="http://www.w3.org/TR/html4/">
   <h:tr>
   <h:td>Apples</h:td>
   <h:td>Bananas</h:td>
   </h:tr>
</h:table>
```

```html
<f:table xmlns:f="http://www.w3school.com.cn/furniture">
   <f:name>African Coffee Table</f:name>
   <f:width>80</f:width>
   <f:length>120</f:length>
</f:table>
```

可以用来表示两个不同的table
也可以使用默认命名kon

```html
<table xmlns="http://www.w3.org/TR/html4/">
   <tr>
   <td>Apples</td>
   <td>Bananas</td>
   </tr>
</table>
```

## JSON

