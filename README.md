pyselect
========

Python Interactive Selection Tool

pyselect is an interactive line selection tool for ASCII , operating via a
full-screen Curses-based terminal session. It is primarily written to be used by
other scripts (like bash, python, perl) to make useful scripts. It is inspired
from iselect (http://manpages.ubuntu.com/manpages/lucid/man1/iselect.1.html).
Since iselect is written in C, I wanted to do something in python which is more
fun, extensible and less than 100 lines. Just hack the pyselect to add more functionality.

Software requirement:
This python script requires urwid (one of the python curses library). On ubuntu
you can install urwid by executing "sudo apt-get install  python-urwid" In case
you do not have install permission, you can just download urwid tar from
http://excess.org/urwid/ and untar the tar.gz at some place and add this
script(pyselect) to the same directory.

Command line Help:
```
usage: pyselect [-h] -f FILE -l [line [line ...]]

Interactive selection of ASCII lines using python.

optional arguments:
  -h, --help            show this help message and exit
  -f FILE, --file FILE  File to write the user selection.
  -l [line [line ...]], --lines [line [line ...]]
                        Lines to be displayed interactively to the user
```

Examples:

Simple Usage:

This command will open full screen Curses-based terminal session. And then
depending on the user selection, the user selection will be return to the file
specified in -f argument of the command.

pyselect -f ~/.result -l line1 line2 line3

Advanced Usage:

pyselect is useful when you create wrappers over it. I was using it to
interactively select my clearcase views using the myviews function given below
in my .bashrc

```bash
function myviews()
{
    resultFile=~/.selectedview
    # clear any previous selection.
    cat /dev/null > $resultFile
    allViews=$(cleartool lsview -short | grep $LOGNAME)
    # pyselect shall be in your $PATH
    pyselect -f $resultFile -l $allViews
    # if file has some contents
    if [[ -s $resultFile ]]; then
        cleartool setview $(cat $resultFile)
    fi
}
```
