title: 'Java添加UTF-7字符集支持'  
tags:  
  - java  
  - utf7  
date: 2010-01-03 17:47:27  
---  

这段时间在做PushServer时，需要对编码过的邮件标题及发信人进行解码，然而开发的时候发现Javamail无法对UTF-7等编码解码，会抛出UnsupportedEncodingException。查看过JDK中rt.jar的部分代码，也看过javamail的部分代码，总结原因如下：JDK本身并不支持UTF-7字符集。关于这个bug（[传送门](http://bugs.sun.com/bugdatabase/view_bug.do?bug_id=4304013)），很早之前就有人反馈过给SUN了，但SUN已经明确表示不会修复这个bug，所以要想支持UTF-7字符集就必须自己写对应的Charset。  

当然，秉承不重复发明轮子的精神，如果有人已经提供了这样的类的话我们就没必要再自己重新实现了。确实已经有人做了这样的事，jcharset（[传送门](http://www.freeutils.net/source/jcharset/)）这个项目已经提供了UTF-7、UTF-7-OPTIONAL等字符集的实现方式，只需要下载jcharset.jar并将其放到${JAVA_HOME}/jre/lib/ext/下即可（这是SUN JRE的一个[bug](http://bugs.sun.com/bugdatabase/view_bug.do?bug_id=4619777)），这样就已经可以对UTF-7编码的字符串进行解码了。需要注意的是jcharset项目是在GPL许可下发布的，若只是需要添加UTF-7字符集的支持的话，可以考虑jutf7项目（[传送门](http://jutf7.sourceforge.net/)），这个项目是在MIT许可下发布的，这样做2次开发之类的都方便。若需要自己添加其他字符集的支持，可以参考jcharset、jutf7等项目。  
