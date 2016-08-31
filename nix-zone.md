# *nix zone

### Generate random passwords from command line

```shell
openssl rand -base64 10
```

```shell
openssl rand -hex 10
```

### Remove `<CR>` characters from file (Mac OS X tested)

```shell
sed -i.bak $'s/\r//' file`
```

### Find large files

```shell
sudo find / -size +500000 -print
```
```shell
sudo find / -size +500000 -exec sudo ls -lah "{}" \;
```

### `tmux` usage

list – `tmux ls`

resume – `tmux a`

detach – `Ctrl + b`, press `d`