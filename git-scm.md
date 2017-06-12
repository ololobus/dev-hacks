# Git/SCM

### Rebase

`git rerere` -- https://git-scm.com/docs/git-rerere


### Show stats since selected commit

```
git diff --stat ef01f745d9a6dd14fc9f9941ef8546d078b75ffe
```


### Create and apply patch

```
git format-patch master --stdout > patch-name-v1.0.patch
```

```
git apply --stat patch-name-v1.0.patch
```
