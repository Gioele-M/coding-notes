Not the best but works 

# HTML
---
## HTML semantic
elements (semantic elements have defined content while non-semantic contain semantic elements (ex div-span))

- <article> -> section of document

- <aside> -> defines content separate from the content it is placed into (like sidebar)

- <details> -> details user can view or hide

- <figcaption> -> figure contains img and figcaption tags
<figure>

- <footer> -> contact/sitemap/copyright

- <form> (?) -> aggregates form data -> action parameter redirects user to other page once form submitted

- <header> -> introductory content/nav links

- <main> -> specifies main content of the document

- <mark> -> defines marked/highlighted text

- <nav> -> set of navigation links

- <section> -> defines section in document

- <summary> -> defines a visible heading for <details>

- <time> -> defines date/time

---

## HTML non-semantic
- <section>

- <div>

- <span>


---
# CSS

### Usual requirements

- display: block | inline | inline-block
	+ by default most elements display property of block. 
		* block will give a new line by default
		* inline will keep on the same line 
		* inline-block will allow us to set w and h while removing new line

- position: static (default) | relative | absolute
	+ static: fills the space it needs not considering other spaces
	+ relative: will stick to position in page following the flow of the document but requires position-top/bottom/left/right to know where
	+ absolute: will stick to the position regardless of other elements, requires position

- top/bottom/right/left: 0;
	+ make parent relative and child absolute

- text-align: center;

- Box sizing eg width/height | border
	+ box-sizing: border/content-box
		* Border box considers whole box (content-padding-margin etc) when setting min/max height/width 
		* Content box considers only content

- Variables go in :root pseudo class
	:root{
		—color-x: ffffff;
	}
	usage > color: var(—color);


### Standard CSS helpful

```css
html{
	box-sizing:border-box;
}

*, *::before, ::after{
	padding:0;
	margin:0;
	box-sizing: inherit;
}

.container{
	margin: 0 auto;
	max-width: 600px;
}
```

## Sizing keywords
- min-content: the minimum size of the content.
- max-content: the maximum size of the content. 
- auto: this keyword is a lot like fr units, except that they “lose” the fight in sizing against fr units when allocating the remaining space.
- fit-content: use the space available, but never less than min-content and never more than max-content.




---

# CSS selectors

- Select all of tag name
`p { /* rules */ }`

- Select all of class name
`.these { /* rules */ }`

- Select the element with unique id
`#this { /* rules */ }`

- Select everything (use with caution!)
`* { /* rules */ }`

- Grouping selectors Applied to HTML elements that have any of the selectors.
`.these, #this { /* rules */ }`

- Chaining selectors Applied to HTML elements that have all the selectors.
`p.these { /* rules */ }`

- Descendant selectors Applied to any HTML elements where the second selector is inside the first selector (no matter how far nested).
`section#info li.name { /* rules */ }`

- Direct Descendant selectors Applied to any HTML elements where the second selector is inside the first selector as a direct descendant.
`section#info > li.name { /* rules */ }`

-Sibling selectors Applied to any HTML elements where the second selector immidiatly follows the first selector on the same level.
`h1 + p { /* rules */ }`

- Attribute selectors Applied to any HTML elements with the matching attribute.
`input[name="surname"] { /* rules */ }`

- Attribute selectors Applied to any HTML elements with the matching attribute.
`a[href="http://google.com/"] { /* rules */ }`

- You can get more flexible with the assignment here eg: 

```css
/* Selects all a tags with an href attribute that starts with 'http' */
a[href^="http"] { /* rules */ }

/* Selects all a tags with an href attribute that ends with '.com' */
a[href$=".com/"] { /* rules */ }

/* Selects all a tags with an href attribute that contains the text 'google' */
a[href*="google"] { /* rules */ }
```

- PSEUDO SELECTORS
`a:hover { /* rules */ }`

- PSEUDO ELEMENTS
`.quote::before{content: ‘xxx’;}`

---

## CSS @media
The @media is used in media queries to apply different styles for different media types or devices. Can check:
- Width and height of viewport and or device
- Orientation 
- Resolution

*Syntax*
```css
@media not | only mediatype and (mediafeature and | or | not mediafeature) {
  CSS-Code;
}
```

- not -> inverts meaning of query
- only -> used in old browsers
- and -> combines media features

*Mediatypes:*
- all -> all media types devices
- print -> for printers
- screen -> for screens
- speech -> for screenreaders

*Mediafeatures*: -> very many, most important:
max-height -> max height of display
min-width -> ..

*EXAMPLE* 
@media screen and (max-width:600px){…}

---

# Grid Layout
Preferred when solving layout issues 

## Important !!

- Div class container needs to be direct parent of all elements
- Div class item are the element of grid -> can have sub-item

- Column grid lines vertical / Row horizontal 
- Grid cell is a 'slot'
- Grid track is a row of cells
- Grid area is a area comprehending multiple cells


## Parent's properties (.container{})
- display: 
	+ grid - generates a block-level grid
	+ inline-grid - generates inline-level grid

- grid-template-columns: <track-size (any measure)> <line-name (optional)>
 + grid-template-rows: 
	* wouldn’t use the line name. Basically just type the size of each column for how many columns you want -> can do repeat(3, 30%)

- grid-area-name: 
	+ define area name for each of the cells (useful when applying it to classes 

- grid-template:
	+ unites the previous 3 functions into one

- column-gap: <line-size>
 + row-gap:
 + grid-column-gap:
 + grid-row-gap:
	* Used to specify the size of the grid lines.

- gap: <row> <column>
	+ shorthand for row/column-gap

- justify-items:
	+ start | end | center | stretch (default); 
	+ justifies items inside cells

- align-items: start | end | center | stretch (default);
	+ aligns grid items along the column axis
	+ stretch

- place-items:
	+ gives both justify and align values at once
	
- justify-content: start | end | center | stretch | space-around | space-between | space-evenly
	+ Used when the size of the grid is larger than the grid content. this aligns the grid along the row axis (opposed to align-content which does it on the column axis)

- align-content: start | end | center | stretch | space-around | space-between | space-evenly
	+ Same as before, aligns content on the column axis

- place-content:
	+ sets both the align and justify content 

- grid-auto-columns:
- grid-auto-rows:
	+ specifies the size of auto generated grid tracks when there are more grid items than cells.

- grid-auto-flow: row | column | row dense | column dense
	+ when grid items are not explicitly placed on the grid manually this determines the order in which they’re placed. dense adds rows/columns as required

- grid:
	+ shorthand for setting all grid-template-rows, grid-template-columns, grid-template-areas, grid-auto-rows, grid-auto-columns, and grid-auto-flow in one declaration. 


## Children's properties

Place child in the grid
- .item {
  + grid-column-start: <number> | <name> | span <number> | span <name> | auto;
  + grid-column-end: <number> | <name> | span <number> | span <name> | auto;
  + grid-row-start: <number> | <name> | span <number> | span <name> | auto;
  + grid-row-end: <number> | <name> | span <number> | span <name> | auto;
}
-> start is where the item starts / end obvs ends
Usage
	.item-a {
		 grid-column-start: 2;
		 grid-column-end: five;
		 grid-row-start: row1-start;
		 grid-row-end: 3;
		}

- grid-column: <start>/<end> -> 3 / 2
	+ grid-row:
	+ Shorthand for start+end

- grid-area: <name> | <row-start> / <column-start> / <row-end> / <column-end>;
	+ Shorthand for previous settings

- justify-self: start | end | center | stretch
	+ aligns grid item inside cell along rows

- align-self: start | end | center | stretch
	+ aligns grid item inside cell along columns

- place-self: 
	+ shorthand for previous declaration 


---

# Flexbox layout
Preferred when needing a one directional flow

### Important
- Flex container / flex axis
-  main axis -> horizontal // cross axis -> vertical
- WORKS OUT OF 12 COLUMNS

## Parent properties

*define flex container*

.container {
	display: flex | inline-flex;
}

- flex-direction: row | row-reverse | column | column-reverse;
	+ Establishes the direction of laying for content (left->right/top->bottom or reversed)

- flex-wrap: nowrap (default) | wrap | wrap-reverse;
	+ defines how the elements are wrapped in the container (no/ top to bottom/ opposite)

- flex-flow: (default) column wrap;
	+ shorthand for direction and wrap 

- justify-content: flex-start (default) | flex-end | center | space-between | space-around | space-evenly | start | end | left | right ... + safe | unsafe;
	+ defines how elements are aligned along the main axis
		* flex-start: items are packed toward the start of the flex-direction.
		* flex-end: items are packed toward the end of the flex-direction.
		* start: items are packed toward the start of the writing-mode direction.
		* end: items are packed toward the end of the writing-mode direction.
		* left: items are packed toward left edge of the container, unless that doesn’t make sense with the flex-direction, then it behaves like start.
		* right: items are packed toward right edge of the container, unless that doesn’t make sense with the flex-direction, then it behaves like end.
		* center: items are centered along the line
		* space-between: items are evenly distributed in the line; first item is on the start line, last item on the end line
		* space-around: items are evenly distributed in the line with equal space around them. 
		* space-evenly: items are distributed so that the spacing between any two items (and the space to the edges) is equal

- align-items: stretch | flex-start | flex-end | center | baseline | first baseline | last baseline | start | end | self-start | self-end + ... safe | unsafe;
	+ Defines the default behaviour of how items are laid along the cross axis

- align-content: flex-start | flex-end | center | space-between | space-around | space-evenly | stretch | start | end | baseline | first baseline | last baseline + ... safe | unsafe;
	+ aligns the flex container lines when there is extra spaces in ross axis

- gap: <row> <column>
- row-gap: <n>
- column-gap: <n>
	+ controls the space between flex items



## Children properties

*By default flex items are laid out in the source order, however this can be set in the item declaration*
.item {
	order:5;
}

- flex-grow: <n> (default 0)
	+ defines the ability of a flex item to grow if necessary. (dictates what amount of the available space inside the flex container the item should take up

- flex-shrink: <n>
	+ defines the ability of a item to shrink

- flex-basis: <n> | auto
	+ defines the default size of an element before the remaining space is distributed

- flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
	+ shorthand for previous 3 commands

- align-self: auto | flex-start | flex-end | center | baseline | stretch;
	+ allows the default alinement to be overridden for individual items.


--- 

# Bootstrap

*Bootstrap link for head:*
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-1BmE4kWBq78iYhFldvKuhfTAU6auU8tT94WrHftjDbrCEXSU1oBoqyl2QvZ6jIW3" crossorigin="anonymous">

# Breakpoints
Breakpoints are customisable widths that determine how the reponsive layout behaves across devices. DESIGNED TO CONTAIN MULTIPLES OF 12 
- xs: 0
- sm: 576px
- md: 768px
- lg: 992px
- xl: 1200px
- xxl: 1400px



















