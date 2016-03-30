title: 'setSoTimeout does not work with nio SocketChannel' 
date: 2011-12-20 17:39:07  
tags:  
- java  
---  

通过SocketChannel.socket().setSoTimeout(timeout)设置读超时，对于SocketChannel.read(buffer)操作来说是不会有任何效果的，如果SocketChannel设置了blocking mode的话会一致阻塞直到有可读取的内容或EOF。  

有人就此给Sun提了个bug（http://bugs.sun.com/bugdatabase/view_bug.do?bug_id=4614802）， 但Sun不认为这是个bug：  

<pre>
Not a bug.  The read methods in SocketChannel (and DatagramChannel) do not
support timeouts.  If you need the timeout functionality then use the read
methods of the associated Socket (or DatagramSocket) object.
</pre>

既然Sun不认为这是个bug只能自己使用的时候注意了（有人说起码是个Javadoc的bug， absolutely！）  
