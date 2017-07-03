Basic
http://javascript.info/tutorial/adding-script-html

Issue reason,
$timeout(function(){});

the default timeout will be zero. So the function will be triggerred without any delay.
The function itself is relying on some value to be initialized, so that's the reason causing the problem. after change it to
$timeout(function(){},3000);
the issue solved.
