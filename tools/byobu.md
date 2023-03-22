# byobu

## chnage temperature data source

```shell
vim ~/.byobu/statusrc
```

```ini
#MONITORED_TEMP=/proc/acpi/thermal_zone/THM0/temperature

MONITORED_TEMP=/sys/class/thermal/thermal_zone5/temp
```

## copy scrollback history to a file

answer from [here](https://askubuntu.com/a/580685/103578)

This time I found a workable solution. From `man byobu`:

```pre
    SCROLLBACK, COPY, PASTE MODES

       Each  window  in  Byobu  has  up to 10,000 lines of scrollback history,
       which you can enter and navigate using the alt-pgup and alt-pgdn  keys.
       Exit  this  scrollback mode by hitting enter.  You can also easily copy
       and paste text from scrollback mode.  To do so, enter scrollback  using
       alt-pgup  or  alt-pgdn,  press the spacebar to start highlighting text,
       use up/down/left/right/pgup/pgdn to select the text, and press enter to
       copy  the  text.  You can then paste the text using alt-insert or ctrl-
       a-].
```

 1. I hit <kbd>F7</kbd> to enter scrollback mode,
 2. <kbd>Space</kbd> to start selecting,
 3. <kbd>g</kbd><kbd>g</kbd> to scroll to the top of the buffer (thanks @GeorgeMarian)
    - If that doesn't work, try this: either with lots of <kbd>Page up</kbd> or <kbd>:</kbd> followed by the largest line number (indicated top right) and <kbd>Page up</kbd> to get to the top of that page, 
 4. <kbd>Enter</kbd> to copy (to byobu's clipboard, not a terminal/system one),
 5. then `cat > my-byobu-dump.txt` in the terminal,
 6. <kbd>Alt</kbd>+<kbd>Insert</kbd> or <kbd>ctrl</kbd>+<kbd>A</kbd>,<kbd>]</kbd> to paste (again, from byobu's clipboard)
 7. <kbd>Ctrl</kbd>+<kbd>D</kbd> to close the file.