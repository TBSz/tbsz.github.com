---
layout: post
title: windows Git 邮件通知
categories:
- Git
tags:
- Git
---

windows git 邮件通知

准备工具

[winwebmail(邮件服务器)](http://www.winwebmail.com/wemdownent.html)

两个命令行发邮件的工具,任选一个

[blat](http://www.blat.net/examples)

[sendemail](http://caspian.dotconf.net/menu/Software/SendEmail)

[命令行收邮件](http://pages.interlog.com/~tcharron/getmail.html) 可不需要

winwebmail默认域、用户名、密码 system.mail/admin/admin

命令行发邮件

blat -body test -to admin@system.mail -s "miss you" -u admin -pw admin

去掉-body 后，控制台接收邮件体 ,ctrl+z 退出,发送

-q 静默方式,-debug 显示调试信息

blat -body test -to admin@system.mail -s "miss you" -u admin -pw admin -debug


Blat v2.7.6 w/GSS encryption (build : Oct 25 2011 21:12:01)


\<<<getline<<< 220 ESMTP on WinWebMail [3.8.1.5] ready.  http://www.winwebmail.com

\>>>putline>>> EHLO WS-SH-L0140.system.mail

\<<<getline<<< 250-SIZE

\<<<getline<<< 250 AUTH LOGIN

Sending stdin.txt to admin@system.mail

Subject: miss you

Login name is admin@system.mail

Try number 1 of 3.

\>>>putline>>> AUTH LOGIN

\<<<getline<<< 334 VXNlcm5hbWU6

\>>>putline>>> YWRtaW4=

\<<<getline<<< 334 UGFzc3dvcmQ6

\>>>putline>>> YWRtaW4=

\<<<getline<<< 235 Authentication successful.

\>>>putline>>> MAIL FROM:<admin@system.mail>

\<<<getline<<< 250 OK

\>>>putline>>> RCPT TO:<admin@system.mail>

\<<<getline<<< 250 OK, recipient accepted

\>>>putline>>> DATA

\<<<getline<<< 354 Send checkpointed message, ending in CRLF.CRLF

\<<<getline<<< 250 RCP:5bb42f1b RCID:20120209141158562_00403~2a7c4980

\>>>putline>>> QUIT

\<<<getline<<< 221 Closing connection




建空仓库

git init --bare

设置接收人员

git config hooks.mailinglist "admin@system.mail"

git config hooks.envelopesender "admin@system.mail"

本质是

.git\config

[hooks]
    mailinglist = admin@system.mail
    envelopesender = admin@system.mail
    emailprefix = "[GIT] "
    showrev = "git show -C %s; echo"

修改post-receive.sample为post-receive
<pre class="prettyprint">
send_mail()
{
    if [ -n "$envelopesender" ]; then
        #sendemail -t admin@system.mail -f admin@system.mail -m
        blat -to admin@system.mail -s "git"  -q
        #/usr/sbin/sendmail -t -f "$envelopesender"

</pre>

<pre class="prettyprint">
cmd /c "echo 1 >> init.txt"
cmd /c "git add ."
cmd /c "git commit -m 1"
cmd /c "git push origin master"


</pre>

<pre class="prettyprint">
To: admin@system.mail
Subject: [GIT] test branch master updated. a63fc11d7c086f719c8d08d36df78c0cf7cbb6bf
X-Git-Refname: refs/heads/master
X-Git-Reftype: branch
X-Git-Oldrev: 45384097f58cfae3c65a1b43aaa295d11615fdf8
X-Git-Newrev: a63fc11d7c086f719c8d08d36df78c0cf7cbb6bf

This is an automated email from the git hooks/post-receive script. 
It was generated because a ref change was pushed to the repository containing the project
 "test".

The branch, master has been updated
       via  a63fc11d7c086f719c8d08d36df78c0cf7cbb6bf (commit)
      from  45384097f58cfae3c65a1b43aaa295d11615fdf8 (commit)

Those revisions listed above that are new to this repository have not appeared on 
any other notification email; so we list those revisions in full, below.

- Log -----------------------------------------------------------------
commit a63fc11d7c086f719c8d08d36df78c0cf7cbb6bf
Author: ddatsh <admin@system.mail>
Date:   Thu Feb 9 14:17:29 2012 +0800

    1

diff --git a/init.txt b/init.txt
index 77fe233..eccec7d 100644
--- a/init.txt
+++ b/init.txt
@@ -11,3 +11,4 @@
 1
 1
 1
+1

-----------------------------------------------------------------------

Summary of changes:
 init.txt |    1 +
 1 files changed, 1 insertions(+), 0 deletions(-)


hooks/post-receive
-- 

test
</pre>
 

还未解决问题

邮件主题

sendmail 可以从控制台根据格式获取 from,subject等，而blat要定义主题必须设置参数 -s
<pre class="prettyprint">
	sendmail -t <<EOF
	From: Mail testing <freemouse.test@gmail.com>		//从哪里发出
	To: freemouse.test@gmail.com	//发给谁
	Cc: freemouse.another@gmail.com 	//抄送给谁
	Bcc: freemouse.test.another@gmail.com	//暗送给谁
	Subject: 邮件测试 	//标题
	———————————- 	//内容
	测试邮件内容
	———————————
	EOF
</pre>
 
 