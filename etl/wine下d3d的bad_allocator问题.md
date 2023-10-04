wine运行部分galgame时，无论32位还是64位的prefix都会出现bad allocator的问题导致游戏崩溃，使用winetricks安装dxvk后得到解决

```bash
winetricks dxvk
```
