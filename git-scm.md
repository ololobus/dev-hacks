# Git/SCM

## Rebase

### rerere

`git rerere` -- https://git-scm.com/docs/git-rerere

### Rebase last N commits

`git rebase -i @~N`


## Edit history

### Add commit-message prefix to a range of commits

```shell
git filter-branch --msg-filter 'printf "FUNNY_LITTLE_PREFIX " && cat' sha_start..@
```

NB: `sha_start` is not included.

### Add commit-message postfix to a range of commits

```shell
git filter-branch --msg-filter 'cat && printf "\n\nFUNNY_LITTLE_POSTFIX"' -f sha_start..@
```


## Stats

Show stats since selected commit
```
git diff --stat ef01f745d9a6dd14fc9f9941ef8546d078b75ffe
```


## Search

### Check that specific commit is in the current branch

```bash
git branch --contains c7ae201c46 | grep $(git rev-parse --abbrev-ref @)
```


## Create and apply patch

```bash
git format-patch master --stdout > patch-name-v1.0.patch
```

`-c` option for extra context lines
```bash
git diff -c master > patch-name-v1.0.diff
```

Or add version and base commit
```bash
git format-patch -c -v7 --base=95bbe5d82e @~1
```

`-3` option for fall back on 3-way merge
```bash
git apply -3 --stat patch-name-v1.0.patch
```

## Subtree
List directories previously added as subtries
```shell
git log | grep git-subtree-dir
```

More: https://www.atlassian.com/blog/git/alternatives-to-git-submodule-git-subtree
