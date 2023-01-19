如果扫到了git目录，可以通过`GitHack`重建
```shell
./GitHack.py <host>/.git/
```

或者使用功能更加强大的`GitHacker`

```
githacker --url http://<host>/.git/ --output-folder result --delay 0.1 --threads 1
```

对于回退过版本的存储库，通过`git log --reflog`和`git reset --hard xxx`能穿越时空（

#Web #Misc #git泄露 