# Regular expressions
http://www.javascriptkit.com/javatutors/re.shtml

### Declare regular expression in JS

```js
const RegularExpression = /pattern/

//OR

const RegularExpression  =  new RegExp("pattern");
```

### RegExp methods
- test(string) -> Test string for pattern matches, returns boolean based on if found or not
- exec(string) -> Searches for a pattern, if not found returns null, if >1 match returns array

---


# Pattern categories

## Position matching
Used to match a string that occurs at a specific location (ex beginning or end)

- *^* -> Only matches the beginning of a string 
	+ (/^the/ matches 'the night' but doesn't 'in the night')

- *$* -> Only matches the end of a string 
	+ `(/and$/ does the same as before)`

- *\b* -> Matches any word boundary (test characters must exist at the beginning or end of a word within the string) 
	+ (/ly\b/ matches 'ly' in 'This is real*ly* cool)

- *\B* -> Matches any non-word boundary 
	+ (/\Bor/ matches 'or' in 'normal' but not 'origami')


---


## Special character matching
Backslash ( \ ), used to match special characters 

- All alphabetical and numerical characters match themselves literally. So /2 days/ will match "2 days" inside a string.

- *\n* -> Matches a new line character

- *\f* -> Matches a form feed character

- *\r* -> Matches carriage return character

- *\t* -> Matches a horizontal tab character

- *\v* -> Matches a vertical tab character

- *\xxx* -> Matches the ASCII character expressed by the octal number xxx 
	+ ("\50" matches left parentheses character "(")

- *\xdd* -> Matches the ASCII character expressed by the hex number dd 
	+ ("\x28" matches left parentheses character "(" )

- *\uxxxx* ->Matches the ASCII character expressed by the UNICODE xxxx 
	+ ("\u00A3" matches "�")

- The backslash ( \ ) is also used when you wish to match a special character literally. For example, if you wish to match the symbol "$" literally instead of have it signal the end of the string, backslash it: 
	+ ```/\$/ ```


---


## Character classes matching []
Combine individual characters into square brackets to cover more matches (ex /[abc]/ matches a, b or c)

- *[xyz]* -> Match any one character enclosed in the set - Use hyphen to denote a range 
	+ (ex [a-z] ot [0-9])

- *[^xyz]* -> Match any character other than the ones in bracket

- *.* -> Match any character except newline or Unicode line terminator 
	+ (/b.t/ matches 'bat' 'bit' etc..)

- *\w* -> Match any alphanumeric character including underscore 
	+ (Equivalent to [a-zA-Z0-9_]) (/\w/ matches "200" in "200%")

- *\W* -> Match any single non-word character (opposite of before) 
	+ (/\w\ matches '%' in '200%')

- *\d* -> Match any single digit 
	+ (equals to [0-9])

- *\D* -> opposite as before

- *\s* -> Match any single space character 
	+ (Equivalent to [ \t\r\n\v\f])

- *\S* -> opposite as before


---


## Repetition matching {}
Match characters that occur in certain repetitions with {} (ex /5{3}/ matches 555)

- *{x}* -> Match exactly x occurrences of regular expression 
	+ (/\d{5}/ matches 5 digits)

- *{x,}* -> Match x or more occurrences of regular expression


- *{x,y}* -> Matches x to y number of occurrences of regular expression

- *?* -> Matches 0 or 1 occurrences 
	+ (== {0,1}) (/a\s?b/ matches "ab" or "a b")

- (\*) -> Matches 0 or more occurrences (== {0,}) 
	+ (we*/ matches "w" in "why" and "wee" in "between", but nothing in "bad")

- *+* -> Matches one or more occurrences (=={1,})


---


## Alternation and grouping matching
Group characters to be considered as a single entity and 'OR' logic to pattern

- *()* -> Group characters together (/(abc)+(def)/ matches one or more occurrences of "abc" followed by one occurrence of "def".)

- *|* -> Alternation combines clauses into one regular expression then matches any of the infividual clauses, similar to or statement (/(ab)|(cd)|(ef)/ matches "ab" or "cd" or "ef")


---


## Back reference matching
If you want to refer to a subexpression in the same regular expression to perform matches based on results of previous match
	
- *()\n* -> Matches a parenthesized clause in the pattern string. n is the number of the clause to the left of the backreference 
	+ (\w+)\s+\1 matches any word that occurs twice in a row, such as "hubba hubba." The \1 denotes that the first word after the space must match the portion of the string that matched the pattern in the last set of parentheses. If there were more than one set of parentheses in the pattern string you would use \2 or \3 to match the appropriate grouping to the left of the backreference. Up to 9 backreferences can be used in a pattern string.)


---


## Pattern Switches
In addition to the pattern-matching characters, you can use switches to make the match global or case- insensitive or both: Switches are added to the very end of a regular expression.

- *i* -> Ignore the case of characters
	+ (/The/i matches "the" and "The" and "tHe")

- *g* -> Global search for all occurrences of a pattern
	+ /ain/g matches both "ain"s in "No pain no gain", instead of just the first.
	
- *gi* -> Global search, ignore case 



















