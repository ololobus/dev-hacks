# macOS

### Disable discrete GPU dependency for selected app

Open `info.plist` inside `.app` package and add

```xml
<key>NSSupportsAutomaticGraphicsSwitching</key>
<string>YES</string>
```

### Hardware temps/fan speed for Mac OS X

https://github.com/Chris911/iStats

```shell
gem install iStats
istats
```

### Using Spark with ipython/jupyter

See [guide](macos-spark-ipython.md).
