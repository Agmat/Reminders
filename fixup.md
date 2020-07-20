# Fixup

`git commit --fixup <COMMIT HASH>` is used to fix a given commit.

The main difference between `git commit --amend` (in addition to fixing older commits) is that fixup create a new brand new commit. This commit will be squashable automatically with `git rebase -i --autosquash` once the PR is approved.

## Exemple (from [here](https://blog.sebastian-daschner.com/entries/git-commit-fixup-autosquash)): 

See the following history as an example:

```
3320dec (HEAD) commit 4
03c9685 commit 3
041c401 commit 2
981fffd commit 1
22f759b (tag: base) initial commit
```

We can modify changes introduced in 981fffd commit 1 and add them as a fixup commit via `git commit -a --fixup 981fffd`

```
c24491b (HEAD) fixup! commit 1
3320dec commit 4
03c9685 commit 3
041c401 commit 2
981fffd commit 1
22f759b (tag: base) initial commit
```

In order to clean-up the history we interactively rebase our changes with `git rebase -i --autosquash base`. This will produce a clean history again

```
caeb1d8 (HEAD) commit 4
03c9685 commit 3
041c401 commit 2
981fffd commit 1
22f759b (tag: base) initial commit
```

The commit hashes after 22f759b now have been changed — they’re based on different commits.
