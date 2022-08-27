## git的常用命令

### 1. 撤销commit
+ git rm
  
```
git rm <file> // 从工作区或者暂存区删除某个文件(可以checkout恢复)
git rm --cached <file> // 删除暂存区的文件
```
+ git reset
  
```
git reset --soft commit_id // 此次提交之后的修改会被退回到暂存区
git reset --hard commit_id  // 此次提交之后的修改不做任何保留
// 例如回滚代码
git log // 查询要回滚的 commit_id
git reset --hard commit_id // HEAD 就会指向此次的提交记录
git push origin HEAD --force // 强制推送到远端

// 误删commit恢复
git relog // 复制要恢复操作的前面的 hash 值
git reset --hard hash // 将 hash 换成要恢复的历史记录的 hash 值
```
+ git revert
  
```
git revert // 之前的提交仍会保留在 git log 中，而此次撤销会做为一次新的提交。
git revert -m // 用于对 merge 节点的操作，-m 指定具体某个提交点
```
