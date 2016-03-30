title: Javamail处理一些未知的编码
tags:
  - javamail
  - x-gbk
date: 2010-01-03 18:49:15
---

查看邮件信头的时候有时会看到x-gbk、hz-gb-2312等编码，然而这些编码javamail并不能识别出来。但是看名称感觉又似曾相识，尝试把x-gbk的编码改成gbk、hz-gb-2312改成gb2312后javamail又能解码出来。虽然并不知道x-gbk、hz-gb-2312是否和gbk、gb2312的标准有什么不同，或者仅仅只是这些编码的别名而已，但是的确可以通过把x-gbk映射成gbk来解决一些编码问题。那么一种便捷的做法便是通过往javamail的javamail.charset.map文件里添加x-gbk到gbk的映射关系，这样就可以对x-gbk进行解码了。

PS：通过findbugs发现，javax.mail.internet.MimeUtility里居然有一行代码是通过==来判断字符串是否相同。囧rz
