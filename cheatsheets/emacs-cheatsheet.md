# Emacs Mode
## What is Emacs?
Emacs is a text editor designed for POSIX operating systems and available on Linux, BSD, macOS, Windows, and more. Users love Emacs because it features efficient commands for common but complex actions and for the plugins and configuration hacks that have developed around it for nearly 40 years.

## Moving the cursor

- CTRL-A/HOME : Move to the beginning of a line.
- CTRL-E/END: Move to the end of a line.
- CTRL-B/LEFT : Move left one character.
- CTRL-F/RIGHT : Move right one character.
- ALT-B/CTRL-LEFT : Move left one word.
- ALT-F/CTRL-RIGHT : Move right one word.
- CTRL-XX: Hold CTRL and press X twice to move the cursor to the beginning of the line, and hold CTRL and press X twice again to move the cursor back.

## Editing text

- CTRL-U : Cut all the characters.
- CTRL-K : Cut the characters to the right of the cursor.
- CTRL-H/BACKSPACE/DELETE (MACOS) : Delete one character to the left.
- CTRL-D/DELETE/FN DELETE (MACOS) : Delete one character to the right.
- CTRL-W : Cut one word to the left.
- ALT-D : Cut one word to the right.
- CTRL-Y : Paste the characters previously cut.
- CTRL-_ : Undo the last edit.
- CTRL-XE : Open the $EDITOR to edit the line.
- ALT-U : Change one word to the right to uppercase.
- ALT-L : Change one word to the right to lowercase.

## Command completion

- TAB: Attempt shell expansion on the current word. If that fails, attempt completion.
```
gi<TAB>     # git
ls *<TAB>   # ls folder1 folder2 file3
```
- CTRL-G : List the expansion of the current word.

## Managing the screen

- CTRL-L : Clear screen (just like clear).
- CTRL-S : Stop screen output. Useful for preventing processes from spamming the stdout.
- CTRL-Q : Resume screen output.

## Managing processes

- CTRL-C : Terminate the foreground process. (Also can be used to delete the whole line.)
- CTRL-Z : Suspend the foreground process (use fg and bg to resume).
- CTRL-D : Exit shell (just like exit).

## Accessing Command History

- CTRL-R : Search the command history. Accept with ENTER/RETURN, abort with CTRL-G.
- CTRL-P/UP : The previous command in history.
- CTRL-N/DOWN : The next command in history.
