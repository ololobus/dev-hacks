# macOS

### Disable discrete GPU dependency for selected app

Open `info.plist` inside `.app` package and add

```xml
<key>NSSupportsAutomaticGraphicsSwitching</key>
<string>YES</string>
```

### Hardware temps/fan speed

https://github.com/Chris911/iStats

```shell
gem install iStats
istats
```

### Using Apache Spark with ipython/jupyter

See [guide](https://gist.github.com/ololobus/4c221a0891775eaa86b0).

### Remove latest TimeMachine snapshots

```shell
sudo tmutil thinLocalSnapshots / 10000000000 4
```

More info: [1](https://www.jethrocarr.com/2017/11/06/macos-high-sierra-unable-to-free-disk-space/), [2](https://apple.stackexchange.com/questions/304651/high-sierra-shows-wrong-disk-usage-for-photos-in-information)

XXX: What do these `10000000000 4` mean?

### Use gdb

To resolve this
```
Unable to find Mach task port for process-id 83767: (os/kern) failure (0x5).
```
issue you have to sign `gdb` executable. Refer to [this](https://stackoverflow.com/questions/11504377/gdb-fails-with-unable-to-find-mach-task-port-for-process-id-error) and [this](https://sourceware.org/gdb/wiki/PermissionsDarwin).

### Core dumps

Your current user may have no permissions on writing to `/cores` directory. So even with `ulimit -c unlimited` core dumps will not be generated. Set `777` on it:

```sh
sudo chmod 777 /cores
```

### VNC client

What a luck macOS has a built-in VNC client! Just type:

```sh
open open vnc://8.8.8.8:5901
```

Or use `ssh` to setup a secure tunnel first:
```sh
ssh -L 5901:127.0.0.1:5901 -C -N -l username 8.8.8.8
```
and then
```sh
open vnc://localhost:5901
```
