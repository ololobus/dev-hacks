# *nix zone


## Command line tools
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


### Copy all `stdout` and `stderr` to file

```shell
python3 very-verbose-program.py 2>&1 | tee -a results.log
```

### Disk usage
Usage by directory
```shell
du -h -d 1 /
```

Overall stats
```shell
df -h
```

### Find files and run some command per file

```shell
find postgres_data/base/ -regex '.*[0-9]+' -exec pg_filedump "{}" \; | grep Error | sort | uniq
```

```shell
find postgres_data/base/ -regex '.*[0-9]+' -print0 | xargs -0 -n1 pg_filedump | grep Error | sort | uniq
```

### Find large files and directories

```shell
du -hs * | sort -rh | head -5
```

```shell
sudo find / -size +500000 -print
```

```shell
sudo find / -size +500000 -exec sudo ls -lah "{}" \;
```

```shell
sudo du -k /* | awk '$1 > 500000' | sort -nr
```


### Create tmpfs partition

```sh
sudo mkdir /mnt/tmpfs
sudo chmod -R 777 /mnt/tmpfs
sudo mount -t tmpfs -o size=512m ext4 /mnt/tmpfs
sudo umount /mnt/tmpfs
```

### Create virtual disk with limited storage

 1. Create a file of the size you want (here 10MB)

    `dd if=/dev/zero of=/home/username/test bs=1024 count=10000`

 2. Make a loopback device out of this file

    `losetup -f /home/username/test`

 3. Format that device in the file system you want

    `mkfs.ext4 /dev/loopXX`

 4. Mount it wherever you want (`/mnt/test` should exist)

    `mount /dev/loopXX /mnt/test`

Don't forget to unmount and clean up loop device after use, with `sudo losetup -d /dev/loopXX`.

### System stress/load tests
```shell
sudo apt install sysbench
sysbench --test=cpu --cpu-max-prime=20000 --num-threads=4 run
```

### Run command and update its output every N_SECONDS
```shell
watch -n N_SECONDS <your command>
```

### Manually generate a locale
```shell
localedef -i en_US -f UTF-8 en_US.UTF-8
```


## Network
### OpenVPN

```shell
sudo openvpn --config '/path/to/openvpn/config.ovpn'
```
To use with login/password create `pass.txt` with:
```text
login
password
```
and add `auth-user-pass /path/to/pass.txt` to `config.ovpn`.

### Wireshark
Filter packets by data size and value 
```python
data.len > 1 or (data.len == 1 and data in {Q K Z I P X B E C S D H c d f F n 2})
```

### Change default TTL value of IP network packages

Maybe helpful to hack mobile hotspot limitations on some cellular data plans.

 * Run `sudo sysctl -w net.inet.ip.ttl=65`
 * Or make it persistent, put `net.inet.ip.ttl=65` into `/etc/sysctl.conf`

## Appearence
### GNOME tweaks

```shell
sudo apt install gnome-tweak-tool
```
