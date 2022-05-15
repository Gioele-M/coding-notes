# JS Medium level tricks

### Rest parameters
By prefixing 3 dots (...) in front of a named argument, the parameters entered into the function at that position and onwards is automatically converted into an array.

eg function addNumbers(...numbers){//for number in numbers...}



# Destructuring

## Nested objects destructuring

Objects can be destructured by declaring the variables of interest exactly in the correct nesting position

```js
var jsondata = {
    title: 'Top 5 JavaScript ES6 Features',
    Details: {
        date: {
            created: '2017/09/19',
            modified: '2017/09/20',
        },
        Category: 'JavaScript',
    },
    url: '/top-5-es6-features/'
};
 
var {title, Details: {date: {created, modified}}} = jsondata

console.log(title) // 'Top 5 JavaScript ES6 Features'
console.log(created) // '2017/09/19'
console.log(modified) // '2017/09/20'
```


## Nested array destructuring
Destructuring on arrays works similarly as on Objects, except instead of curly braces on the left hand side, use square brackets instead:
```js
var soccerteam = ['George', 'Dennis', 'Sandy']
var [a, b] = soccerteam // destructure soccerteam array
console.log(a) // "George"
console.log(b) // "Dennis"

//You can also skip elements with a comma 
var [e,,f] = soccerteam // destructure soccerteam array
console.log(e) // "George"
console.log(f) // "Sandy"

```









