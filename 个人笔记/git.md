- 查看远程所有分支：
```bash
git fetch origin
```
- 我commit了一条命令，发现这条命令我打错字了，我想对它进行修改，同时删除那条错误的commit日志，可以对这条错误的进行合并并修改commit日志。
```bash
git log -2 | cat
git reset --soft HEAD~1 && git commit -m"正确的命令"
```
- 使用cherry-pick把某个分支的部分提交到目标分支，而非合并整个分支
```bash
git branch // 查看当前所有的分支
git log // 查看要挑拣的commit hash
git checkout XXX // 切换到目标分支
出现冲突解决冲突，然后正常add、commit、pull、push
```
- git操作失误：假如我在cherry-pick到main分支，我pull代码的时候，不小心pull dev了，应该是要pull main的，这就是操作失误。
```bash
// 彻底撤销最近一次合并（git pull）
git reset --hard HEAD^
```


- 严格区分大小写：git config core.ignorecase false
- 清除pnpm缓存：pnpm store prune
- 清除VSCode的TS缓存：Ctrl + Shift + P，TypeScript：Restart TS Server