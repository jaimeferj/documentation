# Documentation for Git commands

## Amending other than last commit

[Atlassian Docs](https://www.atlassian.com/git/tutorials/rewriting-history/git-rebase)

[Git Docs](https://git-scm.com/book/en/v2/Git-Branching-Rebasing)

In order to change not but the last commit, you can use the following command:

```git rebase --interactive HASH~```

Where HASH is the commit you want to change. Note the tilde at the end of the
command, because you need to reapply commits on top of the previous commit of
HASH (that is HASH~).
An editor will pop up with the list of commits starting from HASH. You can
change the word "pick" to "edit" in the commit you want to change or even other
advanced changes.
After all the changes haven been made save and close the editor.
An example showing the possible editions is:

```plaintext
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

> **CLARIFICATION**: The commit are printed in reverse order, so the first
> commit is the last one (in this case ee2adc2).

In our case, we wanted to change commit 2231360, so we just have to change
`pick` to `edit` in the commit, and save the file.
Then, we would edit the commit as we would, stage our changes and finally run
`git rebase --continue` to finish the process.

Another useful usecase is renaming more than one commit. Do `git rebase
--interactive HEAD~n` where n is the number of commits you wanna change the
wording and then put reword in each of them you want to change. Save, quit, and
you will be prompted to edit each commit you chose. ## Invalid Remote Branch

Sometimes local branches are tracking invalid remote branches, or incorrect
ones. In order to first check which branch our local is tracking remotely, we
can run the following command:

```bash
git branch -vv
```

```plaintext
* feat/tasks-executor 86484cd [origin/main: ahead 6] feat: version 2
  main                041e136 [origin/main: ahead 1] change queue type name to vector-s3
```

As can be seen, the local branch `feat/tasks-executor` is tracking the remote
branch main instead of their respective branch. In order to fix this, we can
run the following command:

## Changing branch tracking

```bash
git branch <local-branch-name> --set-upstream-to <remote-name>/<remote-branch-name>
```

```plaintext
branch '<local-branch-name>' set up to track '<remote-name>/<remote-branch-name>'.
```

If the remote branch does not exist, the following error will be shown:

```plaintext
fatal: the requested upstream branch 'origin/feat/tasks-executor' does not exist
hint:
hint: If you are planning on basing your work on an upstream
hint: branch that already exists at the remote, you may need to
hint: run "git fetch" to retrieve it.
hint:
hint: If you are planning to push out a new local branch that
hint: will track its remote counterpart, you may want to use
hint: "git push -u" to set the upstream config as you push.
hint: Disable this message with "git config set advice.setUpstreamFailure false"

```

We will just have to create a the branch in the remote repository and then run
the command again.

## Craeting remote branches

In order to create a remote branch, we can run the following command to push
the local branch to a new remote branch:

```bash
git push <remote-name> <local-branch-name>:<remote-branch-name>
```
