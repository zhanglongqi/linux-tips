# git merge tool

setting merge tool for `git mergetool  
For the default path of meld (you can use Unix path style in git bash): 
``` 
git config --global mergetool.meld.path /C/Program\ files\ \(x86\)/Meld/meld/meld.exe  
```
for the non-default path you can use tab key, it will suggest the pathso its easy to get it right.`

**note** sometimes you need use `meldc.exe or bin/meld,`

you may also want to check this:[how-to-set-meld-as-git-mergetool](http://stackoverflow.com/questions/12956509/how-to-set-meld-as-git-mergetool)

## error: too many arguments

```bash
$ git config --global diff.external meld
$ git diff filename
Usage:
  meld                        Start with an empty window
  meld <file|dir>             Start a version control comparison
  meld <file> <file> [<file>] Start a 2- or 3-way file comparison
  meld <dir> <dir> [<dir>]    Start a 2- or 3-way directory comparison
  meld <file> <dir>           Start a comparison between file and dir/file

meld: error: too many arguments (wanted 0-3, got 7)
external diff died, stopping at src/transferlistfilterswidget.h.
```

Meld will open but it will complain about bad parameters. The problem is that Git sends its external diff viewer seven parameters when Meld only needs two of them; two filenames of files to compare. One way to fix it is to write a script to format the parameters before sending them to Meld. Let's do that.

Create a new python script in your home directory \(or wherever, it doesn't matter\) and call it diff.py.

```python
#!/usr/bin/python

import sys
import os

os.system('meld "%s" "%s"' % (sys.argv[2], sys.argv[5]))
```

Now we can set Git to perform it's diff on our new script \(replacing the path with yours\):

```bash
$ git config --global diff.external /home/USERNAME/bin/diff.py
$ git diff filename
```



