- 查看远程所有分支：
```bash
git fetch origin
```
- 我commit了一条命令，发现这条命令我打错字了，我想对它进行修改，同时删除那条错误的commit日志，可以对这条错误的进行合并并修改commit日志。
```bash
git log -2 | cat
git reset --soft HEAD~1  && git commit -m"正确的命令"
```