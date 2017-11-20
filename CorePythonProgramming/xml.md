## 1.构建xml文件
```python
# -*- coding: utf-8 -*-
import xml.etree.ElementTree as ET

root = ET.Element("html")
head = ET.SubElement(root, "head")
title = ET.SubElement(head, "title")
title.text = "Title"

body = ET.SubElement(root, "body")
body.set("bgcolor", "#ffffff")
body.text = "Hello, World!"

tree = ET.ElementTree(root)
tree.write("hello.xhtml")
```

hello.xhtml：

    <html><head><title>Title</title></head><body bgcolor="#ffffff">Hello, World!</body></html>

## 2.加载xml文件
```python
# -*- coding: utf-8 -*-
import xml.etree.ElementTree as ET

tree = ET.parse("hello.xhtml")
print tree.findtext("head/title")
root = tree.getroot()
# do something here
tree.write("out.xml")
```

输出：

    Title

out.xml:

    <html><head><title>Title</title></head><body bgcolor="#ffffff">Hello, World!</body></html>
