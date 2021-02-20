---
title: JS DOM 中的空白节点的过滤 
date: 2017-09-14 12:13:31
tags: [前端, JavaScript]
categories: 前端
---

```
<html>  
<head><title>DOM Test</title></head>  
<body>  
<table>   
    <tr>   
    <td id="TEST">  
        <input type="submit" value="确定"/>  
        <input type="button" value="取消"/>  
    </td>  
    </tr>   
</table>      
<script type="text/javascript">  
    <!--  
        var td = document.getElementById("TEST");  
        alert(td.childNodes.length);    //结果为4   
    -->  
</script>  
</body>  
</html>  
```


由于DOM中，空白也是作为一个文本节点，而两个input元素后面都有空白（回车、空格、制表符），所以结果为4，删除空白后结果为2。
为了处理里面的空格节点,用以下函数来处理。


```
function cleanWhitespace(element)   
{   
    for(var i=0; i<element.childNodes.length; i++)   
    {   
        var node = element.childNodes[i];   
        if(node.nodeType == 3 && !/\S/.test(node.nodeValue))   
        {   
            node.parentNode.removeChild(node);   
        }   
    }   
}   
```


处理结点cleanWhitespace(document.getElementById("TEST"))后,OK,解决 