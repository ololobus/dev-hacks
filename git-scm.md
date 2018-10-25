# Git/SCM

### Rebase

`git rerere` -- https://git-scm.com/docs/git-rerere


### Show stats since selected commit

```
git diff --stat ef01f745d9a6dd14fc9f9941ef8546d078b75ffe
```


### Create and apply patch

```bash
git format-patch master --stdout > patch-name-v1.0.patch
```

`-c` option for extra context lines
```bash
git diff -c master > patch-name-v1.0.diff
```

`-3` option for fall back on 3-way merge
```bash
git apply -3 --stat patch-name-v1.0.patch
```
