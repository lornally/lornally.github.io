git rebase出错, 不能continue了, 咋办呢? 很简单: 

This happens because when fixing a conflict, you removed all code in the patch beeing applied to the branch you are rebasing on. Use `git rebase --skip` to continue.



参考: https://stackoverflow.com/questions/8523776/git-rebase-continue-complains-even-when-all-merge-conflicts-have-been-resolved

原因是合并的过程中删掉了所有需要的



[Forgot “git rebase --continue” and did “git commit”. How to fix?](https://stackoverflow.com/questions/6457044/forgot-git-rebase-continue-and-did-git-commit-how-to-fix)

- 解决: Just do `git reset --soft HEAD^`. It moves the HEAD pointer to its parent but keeps the work tree and adds the merge change to the index. So you can continue rebasing with `git rebase --continue` as before.



处理merge: https://stackoverflow.com/questions/161813/how-to-resolve-merge-conflicts-in-git

