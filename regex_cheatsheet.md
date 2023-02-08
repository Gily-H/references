# Regular Expressions

Regular expressions (or regex) are used for pattern matching character strings  
- Regex will search the character string until it finds a matching substring

- To denote a regex, we place the pattern between two backslash characters `\<regex-pattern>\`  
- We can also append pattern modifiers to alter how our regular expression matches characters `\<regex-pattern>\<pattern-modifier>` 

- Wildcard characters work on the preceding expression unit
- The preceding expression unit can either be a single character, a character class, or a group

    - Match one or more _d_ characters /abc`d+`/

    - Match character class 0 or more times `[ab]*`cd

    - Match group 0 or 1 time a`(b|c)?`d


## Greedy vs Reluctant

By default, regex favors matching whenever possible. This means that regex will find the longest match  
and will backtrace when new rules are introduced. This backtracing could cause regex matching to be considerably slow
as the regex will have to do multiple backtrace steps

We can make regex patterns reluctant to find a match. This means that the regex will find the closest match,  
and will move forward to search for a match as new rules are introduced. This will reduce the number of necessary  
backtraces, but can still amount to multiple steps

__Recommendations__
- Be precise with regex patterns
- Favor character class negation `[^]` as this will reduce greediness
- Add Reluctant Quantifiers if the language supports it

## Relucant Quantifiers

To make a quantifier reluctant, we add a question mark after the quantifier

\<unit\>?`?`  
\<unit\>+`?`  
\<unit\>\*`?`  
\<unit\>{n,m}`?`  

## Wildcard Characters

## __.__

The dot `.` wildcard will match any character except the newline character. In a character class the dot acts as literal dot

_NOTE: avoid using the `.` special character whenever possible to combat greedy matching_

## __?__

The `?` character will match `0 or 1` of the preceding expression

## __\*__

The `*` character represents `0 or more` of the preceding expression

## __+__

The `+` character represents `1 or more` of the preceding expression

## __{}__

You can input limits (n,m) between `{}` to indicate that the preceding expression should be repeated a minimum of n times and a max of m times

```python
- {n} # exactly n times
- {0, m} # 0 to m times
- {n,} # n to infinite times
- {n, m} # from n to m times
```

## __[]__

Any characters between the character class `[]` will denote a range of options that can constitute a valid expression to match

_NOTE: empty character classes are invalid_

## __[^]__

A character class with a carat `[^..]` at the beginning will match any option _not_ inside the character class

_NOTE: a carat ^ at the end of a character class [..^] will match a literal carat_

## __[ ] { } ( )__

To match a literal bracket in a character class, we can escape the bracket [`\[`]. The same will apply for a closing bracket

## __-__

The dash `-` character is used between two characters to denote a sequential range of values 

## __|__

The pipe `|` signifies an OR operation where the expression can match either the left or the right pattern  
Will denote a literal pipe in a charcter class

_NOTE: Best to group expressions used in a pipe `(expr1|expr2)`, and the left expression should be the most desired expression_

## __()__

Expressions can be grouped using `()`

## __^__

The `^` (not inside a character class) denotes the beginning of the matching character string

## __$__

The `$` denotes the end of the matching character string (can contain the \n character if present)

_NOTE: if available, best to use `\z` instead of `$` as it will ignore the new line character at the end of a string_

## __Whitespace Characters__

- \t - tab
- \f - page break
- \n - new line
- \r - carriage return
- ' ' - whitespace (' ' here is used to illustrate the whitespace, but the single quotes should not be included in the regex)
- /R - match all new lines on all platforms (UNIX, Mac, Windows)
- \s - match all whitespace characters


## __\b__

Word boundaries are points in a string between a word character and a non-word character (space, punctuation, etc...)
```python
Assertions: are fun!

# word boundaries
# word character | non-word character
# non-word character | word character
|Assertions|: |are| |fun|!
```

## __\B__

Non-word boundaries are points in a string between two characters of the same type
```python
Assertions: are fun!

# non-word boundaries
# word character | word character
# non-word character | non-word character
A|s|s|e|r|t|i|o|n|s:| a|r|e f|u|n!|
```

## Pattern Modifiers

### __i__

Appending an `i` will check for case-insensitive matches

### __g__

The global `g` modifier. Returns every possible match rather than just the first match (does not consider over-lapping matches)

### __x__

The extended `x` modifier. Allows for writing regex in a readable format. All whitespace in a regex pattern is ignored  
This allows for pattern separations with new lines or indents    
Comments can also be written in the regex using the `#` symbol  
The caveat is that any `#` or space character outside of a character class needs to be escaped `\` when matching the literal character


## Shortcodes

_NOTE: Avoid using shortcodes as there are many pitfalls associated with them due to platform and locale implementations_  

Shortcodes act similar to pre-defined character classes. They also retain their special meaning when placed in a character class

|Shortcode|Description|
|:-:|:-:|
|\d|digits - `[0-9]`|
|\D|negation of digits = `[^0-9]`|
|\w|alphanumeric characters and underscore - `[A-Za-z0-9_]`|
|\W|negation of alphanumeric characters and underscore - `[^A-Za-z0-9_]`|
|\s|space characters - `[\t\f\r\n ]`
|\S|negation of space characters - `[^\t\f\r\n ]`

_NOTE: Becareful when using two negation shortcodes together in a character class `[\D\S]`  
this example will have unintended results as it will match characters that are not whitespace OR a digit  
(character cannot be both at the same time)_
