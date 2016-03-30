title: '邮件信头中Return-Path、Reply-To和From的区别'
tags:
  - from
  - reply-to
  - return-path
date: 2010-08-01 02:14:27
---

懒得自己写，直接引用一段别人的回复（[原贴传送门](http://stackoverflow.com/questions/1235534/what-is-the-behavior-difference-between-return-path-reply-to-and-from)），大致跟自己之前的实践及查看的文档相符。  

<pre>
1)The Return-Path (sometimes called the Reverse-Path or Envelope-FROM -- all of these terms can be used interchangeably) is the value used during the SMTP session. As you can see, this does not need to be the same value that is actually found in the mail headers. Only the recipient's mail server is supposed to add a Return-Path header to the top of the email. This records the actual Return-Path sender during the SMTP session. If a Return-Path header is already exists in the email, then that header is to be removed, and replaced by the recipient's mail server.

All bounces that occur during the SMTP session should go back to the Return-Path value. Some servers may accept all email, and then queue it locally, until it has a free thread to deliver it to the recipient's mailbox. If the recipient doesn't exist, it should bounce it back to the recorded Return-Path value.

Note, not all mail servers obey this rule. Some mail servers will bounce it back to the FROM address.

2)The FROM address is the value actually found in the FROM header. This is supposed to be who the message is FROM. This is what you see as the "FROM" in most mail clients. If an email does not have a Reply-To header, then all human (mail client) replies should go back to the FROM address.

3)The Reply-To header is added by the sender (or the sender's software). It is where all human replies should be addressed too. Basically, when the user clicks "reply", the Reply-To value should be the value used as the recpient of the newly composed email. The Reply-To value should not be used by any server. It is meant for client side use.

However, as you can tell, not all mail servers obey the RFC standards or recommendations.
</pre>
