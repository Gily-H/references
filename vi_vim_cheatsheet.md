# Vi/Vim Commands
vi/vim are text editors that allow us to edit files in the command line. `vim` is an advanced version of `vi` and supports all the same features.

|Command|Description|
|:-:|:-|
|`i`|Switch to insert mode|
|`esc`|Switch to command mode|
|`:w`|Save changes|
|`:wq`|Save changes and exit|
|`ZZ`|Same effect as :wq|
|`:q!`|Exit without saving|
|`yy`|Copy line of text|
|`p`|Paste line of text below current line|
|`o`|Open a new line under current line|
|`O`|Open new line above current line|
|`A`|Append to the end of the line|
|`a`|Append after the cursor's current position|
|`I`|Insert text at the beginning of the current line|
|`b`|Go to the beginning of a word|
|`e`|Go to the end of a word|
|`x`|Delete a single character|
|`dd`|Delete an entire line|
|`Xdd`|Delete X number of lines|
|`Xyy`|Copy X number of lines|
|`G`|Go to last line of the file|
|`XG`|Go to line X in the file|
|`gg`|Go to first line of the file|
|`:num`|Display current line's number|
|`h`|Move cursor left one character|
|`j`|Move cursor down one line|
|`k`|Move cursor up one line|
|`l`|Move cursor right one character|
|'u'|Undo previous entry|
|`/`|Search prompt. Will pattern match the given input|
|`n`|During a `/`search, will go to next occurrence of a match|
|`N`|During a `/`search, will go to previous occurrence of a match|

__Search examples__
```bash
# vim editor

# search forwards
/<pattern-to-search>

# search backwards
?<pattern-to-search>

```
