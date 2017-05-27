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


### `tmux` usage

list – `tmux ls`

resume – `tmux a`

detach – `Ctrl + b`, press `d`


### Disk usage
Usage by directory
```shell
du -h -d 1 /
```

Overall stats
```shell
df -h
```


### Find large files

```shell
sudo find / -size +500000 -print
```
```shell
sudo find / -size +500000 -exec sudo ls -lah "{}" \;
```

### Find large directories
```shell
sudo du -k /* | awk '$1 > 500000' | sort -nr
```

### System stress/load tests
```shell
sudo apt-get install sysbench
sysbench --test=cpu --cpu-max-prime=20000 --num-threads=4 run
```

### Run command and update its output every N_SECONDS
```shell
watch -n N_SECONDS <your command>
```
