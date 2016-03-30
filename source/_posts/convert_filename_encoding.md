title: 转换文件名编码
tags:
  - iconv
  - convmv
date: 2010-07-16 22:46:40
---

合作方给了一批含有大量中文文件名的文件过来，需要对这批文件批量处理后再将结果反馈回去。这批文件的文件名是GBK编码的，但是我本地的环境是zh_CN.UTF-8的，在我的机器上这些文件名就乱码了。当然，可以先通过LANG=zh_CN.GBK将本地环境临时改成GBK来处理，但是能不能一次性将这些文件名批量转成UTF-8呢？  

很显然，是可以的。iconv能将文件内容从一种字符集编码转换成另一种，类似的，convmv能将文件名从一种编码转换成另一种。
<pre>
conmv - converts filenames from one encoding to another. 
</pre>
这个命令相当强大，可以递归目录，可以指定其他命令替代rename操作，可以指定低内存使用量，可以unescape，可以转换大小写等。实乃居家旅行，杀人灭口必备良品啊！

