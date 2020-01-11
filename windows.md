# Windows

### Disk performance test

```shell
winsat disk -drive DISK_LETTER
```
where `DISK_LETTER` is in `c/d/g/f/etc.`

### Kill all processes by name

```shell
taskkill /F /IM program_name.exe
```

### Change default TTL value of IP network packages

Maybe helpful to hack mobile hotspot limitations on some cellular data plans.

1) Open `regedit`;

2) Go to `HKEY_LOCAL_MACHINE — SYSTEM — CurrentControlSet — Services — Tcpip — Parameters`;

3) Create `DefaultTTL` with `DWORD` type and value `65`.
