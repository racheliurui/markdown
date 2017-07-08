title: Javascript loading issue
date: 2017-01-05 20:10:33
tags:
- javascript
- troubleshooting
---

Basic
http://javascript.info/tutorial/adding-script-html

Issue reason,
$timeout(function(){});

the default timeout will be zero. So the function will be triggerred without any delay.
The function itself is relying on some value to be initialized, so that's the reason causing the problem. after change it to
$timeout(function(){},3000);
the issue solved.
