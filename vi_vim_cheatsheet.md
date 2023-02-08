# Vi/Vim

vi/vim are text editors that allow us to edit files in the command line. `vim` is an advanced version of `vi` and supports all the same features.

## Vim Commands

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
|`0`|Go to the beginning of the current line|
|`$`|Go to the end of the current line|
|`b`|Go to the beginning of a word|
|`w`|Go to the next word|
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
|`Ctrl + D`|Scroll down the file|
|`Ctrl + U`|Scroll up the file

__Search examples__
```bash
# vim editor

# search forwards
/<pattern-to-search>

# search backwards
?<pattern-to-search>

```


## Visual Mode

Visual mode has _three_ modes in itself: Character mode, Line Mode, and Block Mode

To enter Character mode in vim, from the command mode we enter the `v` key

To enter Line mode in vim, from the commmand mode we enter the `V` (Shift v) key

To enter block mode in vim, from the command mode we enter `^v` (Ctrl v) key


|Command|Description|
|:-:|:-|
|v|Enter character mode|
|V|Enter line mode|
|^v|Enter block mode|

### Character mode

In character mode, we can highlight parts of a sentence for modification  

Enter _visual character mode_ by entering `v`  
Highlight text using the arrow keys, or using the `w` key (highlight to next word) or the `$` key (highlight rest of line)
- enter the `d` key to delete the highlighted text
- enter the `p` key to paste text
- enter the `c` key to change the highlighted text (will transition to insert mode)
