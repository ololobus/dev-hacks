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
