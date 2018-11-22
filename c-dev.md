# C development

## Debugging

### Compilation flags to enable debug

```shell
gcc -O0 -g exec.c -o exec
```

### Enable core dumps on Ubuntu
```shell
sudo sysctl -w kernel.core_pattern=core.%p
```

Next, set ulimit in the shell, where you run your program
```shell
ulimit -c unlimited
```
Core dumps will be side by side with executable.

### LLDB

Commands

* `b FunctionName` – set breakpoint
* `bt` – backtrace
* `n` – next
* `s` – step in
* `c` – continue
* `Enter` – previous command repeat

Read core-dumps

```bash
lldb --core /cores/core.30993
```

Select frame

```
fr sel 4
f 4
```

### Flamegraphs

[More info](http://www.brendangregg.com/FlameGraphs/cpuflamegraphs.html#DTrace)

Dtrace by pid
```bash
sudo dtrace -x ustackframes=100 -n 'profile-99 /pid == 21898  && arg1/ {@[ustack()] = count(); } tick-60s { exit(0); }' -o out.stacks
```

```bash
sudo dtrace -x ustackframes=100 -n 'profile-99 /execname == "postgres" && arg1/ {@[ustack()] = count(); } tick-60s { exit(0); }' -p 21800  -o out.stacks
```

Dtrace by process name
```
sudo dtrace -x ustackframes=100 -n 'profile-99 /execname == "postgres" && arg1/ {@[ustack()] = count(); } tick-60s { exit(0); }' -o out.stacks
```

Create SVG
```
~/dev/FlameGraph/stackcollapse.pl out.stacks > out.folded
~/dev/FlameGraph/flamegraph.pl out.folded > out.svg
```

