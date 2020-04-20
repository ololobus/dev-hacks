# macOS

### Disable discrete GPU dependency for selected app

Open `info.plist` inside `.app` package and add

```xml
<key>NSSupportsAutomaticGraphicsSwitching</key>
<string>YES</string>
```

### Hardware temps/fan speed for macOS

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

### Use gdb on macOS

To resolve this
```
Unable to find Mach task port for process-id 83767: (os/kern) failure (0x5).
```
issue you have to sign `gdb` executable. Refer to [this](https://stackoverflow.com/questions/11504377/gdb-fails-with-unable-to-find-mach-task-port-for-process-id-error) and [this](https://sourceware.org/gdb/wiki/PermissionsDarwin).
