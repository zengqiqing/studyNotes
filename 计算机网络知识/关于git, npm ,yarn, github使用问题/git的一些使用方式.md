场景一：git 操作中，进行了一些错误的提交，但还没有 push 到远程分支上，想撤销本次提交，可以使用 git rest --soft/hard.

git reset --soft ：回退到某个版本，只回退 commit 信息，不会直接恢复到上一级的代码中，如果还需要提交，直接执行 commit 即可。

Git rest