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
* `p` – print variable or expression
* `expr` – evaluate and print expression
* `Enter` – previous command repeat

Read core-dumps

```bash
lldb --core /cores/core.30993
```
```shell
lldb /path/to/executable -c /path/to/core.2731
```

Select frame

```
(lldb) fr sel 4
(lldb) f 4
```

Print binary or hex

```
(lldb) p/t (uint32)((35184372088832) >> 32) & 0xffff
(unsigned int) $0 = 0b00000000000000000010000000000000
```

```
(lldb) p/x 100500
(int) $2 = 0x00018894
```

Change signal handling rules (e.g. allow SIGINT pass)

```
(lldb) pro hand -p true SIGINT
NAME         PASS   STOP   NOTIFY
===========  =====  =====  ======
SIGINT       true   true   true 
```

## Profiling

### Flamegraphs

[More info](http://www.brendangregg.com/FlameGraphs/cpuflamegraphs.html#DTrace)

#### dtrace
By pid
```bash
sudo dtrace -x ustackframes=100 -n 'profile-99 /pid == 21898  && arg1/ {@[ustack()] = count(); } tick-60s { exit(0); }' -o out.stacks
```

```bash
sudo dtrace -x ustackframes=100 -n 'profile-99 /execname == "postgres" && arg1/ {@[ustack()] = count(); } tick-60s { exit(0); }' -p 21800  -o out.stacks
```

By process name
```
sudo dtrace -x ustackframes=100 -n 'profile-99 /execname == "postgres" && arg1/ {@[ustack()] = count(); } tick-60s { exit(0); }' -o out.stacks
```

Create SVG
```shell
~/dev/FlameGraph/stackcollapse.pl out.stacks > out.folded
```
```shell
~/dev/FlameGraph/flamegraph.pl out.folded > out.svg
```

#### perf
By pid
```shell
sudo perf record -p 12345 -F 99 -g
```

```shell
mv perf.data my_prog_perf.data
```

Create SVG
```shell
sudo perf script -i my_prog_perf.data | ~/dev/FlameGraph/stackcollapse-perf.pl > my_prog_perf_out.perf-folded
```
```shell
~/dev/FlameGraph/flamegraph.pl my_prog_perf_out.perf-folded > my_prog_perf.svg
```
