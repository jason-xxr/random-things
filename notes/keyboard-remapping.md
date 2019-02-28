# Remap key using regedit in Windows

### Example reg file
```
Alt  -> Ctrl
Win  -> Alt
Ctrl -> Win
```
```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Keyboard Layout]
"Scancode Map"=hex:00,00,00,00,00,00,00,00,07,00,00,00,1d,00,38,00,38,00,5b,e0,5b,e0,1d,00,1d,e0,38,e0,38,e0,5c,e0,5c,e0,1d,e0,00,00,00,00
```

### Explained

```
Alt  -> Ctrl
Win  -> Alt
Ctrl -> Win

00,00,00,00    Header: Version. Set to all zeroes.
00,00,00,00    Header: Flags. Set to all zeroes.
07,00,00,00    7 entries in the map (including null entry).
1d,00,38,00    Left Alt (38,00) -> Left Ctrl (1d,00)
38,00,5b,e0    Left Win (5b,e0) -> Left Alt (38,00)
5b,e0,1d,00    Left Ctrl (1d,00) -> Left Win (5b,e0)
1d,e0,38,e0    Right Alt (38,00) -> Right Ctrl (1d,00)
38,e0,5c,e0    Right Win (5b,e0) -> Right Alt (38,00)
5c,e0,1d,e0    Right Ctrl (1d,00) -> Right Win (5b,e0)
00,00,00,00    Null entry.
```

```
Disable Win keys

00,00,00,00	Header: Version. Set to all zeroes.
00,00,00,00	Header: Flags. Set to all zeroes.
03,00,00,00	3 entries in the map (including null entry).
00,00,5b,e0	Left Windows Key (0xe05b) -> Disable (0x00).
00,00,5c,e0	Right Windows Key (0xe05c) -> Disable (0x00).
00,00,00,00	Null entry.
```

### Keys

```
1d 00    Left Ctrl
1d e0    Right Ctrl
38 00    Left Alt
38 e0    Right Alt
5b e0    Left Windows Key
5c e0    Right Windows Key
5d e0    Windows Menu Key
```
