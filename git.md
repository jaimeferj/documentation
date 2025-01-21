# Amending other than last commit
[Atlassian Docs](https://www.atlassian.com/git/tutorials/rewriting-history/git-rebase)
[Git Docs](https://git-scm.com/book/en/v2/Git-Branching-Rebasing)


In order to change not but the last commit, you can use the following command:

```git rebase --interactive HASH~```

Where HASH is the commit you want to change. Note the tilde at the end of the command, because you need to reapply commits on top of the previous commit of HASH (that is HASH~).
An editor will pop up with the list of commits starting from HASH. You can change the word "pick" to "edit" in the commit you want to change or even other advanced changes.
After all the changes haven been made save and close the editor.
An example showing the possible editions is:
```
pick 2231360 some old commit
pick ee2adc2 Adds new feature


# Rebase 2cf755d..ee2adc2 onto 2cf755d (9 commands)
#
# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# x, exec = run command (the rest of the line) using shell
# d, drop = remove commit
```

> **CLARIFICATION**: The commit are printed in reverse order, so the first commit is the last one (in this case ee2adc2).

In our case, we wanted to change commit 2231360, so we just have to change `pick` to `edit` in the commit, and save the file.
Then, we would edit the commit as we would, stage our changes and finally run `git rebase --continue` to finish the process.
