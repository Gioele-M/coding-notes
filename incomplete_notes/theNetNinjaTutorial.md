# Global environment

- In js in the _web_, the global object is the *WINDOW*, which has methods freely available in browser without calling 'window' ex. setTimeout()

- In _node_ instead the global object is *GLOBAL*, which has almost all methods of 'window'.
	+ Example we don't have document declared to call functions on


--- 

# Paths

`__dirname` -> returns the absolute path of the folder

`__filename` -> returns the absolute path of the folder plus name of file

---


# Node core modules (to be imported)

### 'os'(operating system)
Retrieves info about the operating system


### 'fs' (file system)
Reads, writes, deletes files


- readFile
	+ takes filename and callback with error and data
	+ it's *asynchronous* so the execution won't wait for it 

```js
fs.readFile('relative/path/filename.txt', (err, data) =>{
	if(err){
		console.log(err)
	}
	console.log(data.toString())

	})
```

---

- writeFile
	+ takes filename, content to write and callback for when the process is complete
	+ overwrites by default!
```js
fs.writeFile('relative/path/filename.txt', 'content to write', ()=>{
	console.log('file was written')
})
```

---

- mkdir / rmdir
	+ makes / deletes directory, takes the relative path plus new file's name
	+ callback function fires once process is complete

```js
if (!fs.existsSync('./newfilename')){ // checks if file exists
	fs.mkdir('./newfilename', (err)=>{
		if(err){
			console.log(err)
		}
		console.log(folder.created)
	})
}
```

---

- unlink
	+ removes file

```js
//if file exists delete it
if(fs.existsSync('./deletefile.txt')){
	fs.unlink('./deletefile.txt', (err)=>{
		console.log('deleted')
	})
}

```


---

# Streams
Option to be able to start using the data before it has finished loading

Small chuncks of data are loadend in the buffer and sent by the stream while the rest is loading and a new buffer is being made

There are read and write streams

### Read stream and write stream

```js
const fs = require('fs')


const readStream = fs.createReadStream('./filename.txt') //  {encoding:'utf8'} to not use toString() later
const writeStream = fs.createWriteStream('./filename.txt')



// This is similar to an event listener, every time new data is sent it gets processed by the function 
readStream.on('data', (chuck)=>{ 
	console.log(chunck.toString())
	writeStream.write(chunk)
})



// This can be done in a pipe

readStream.pipe(writeStream)
```


------------------------------------------------------------------

# Clients and Servers

### Request and response objects
All part of previous knowledge so far


*Response*
res.sendFile('./filename.html', { root: \__dirname}) (no \)

*Response 404*
Use 'app.use' to send a 404 page at the end of the file, that code is going to be run if none of the declared routes comprehends the requested one 

---


# View Engines - EJS

*in router declaration*
`app.set('view engine', 'ejs')` // declare view engine for express 
`app.set('views', 'filename')` // In here is where I put my EJS view files

Now you can switch sendFile with render + ejs filename (minus extension)
`res.status(200).render('index')`


## *EJS* files basically have the HTML content plus extra tags which help implement the dynamic functionality

With render we could also send values that then can be caught in the ejs files and implemented in the HTML
Ex.
`res.status(200).render('index', { title: 'home' }) // Pass object with all values you need`
This could also pass arrays as value



#### Ejs tags

`<% %>` -> declare js logic (ex if, declare value etc)
`<%= %>` -> output value
`<%- %>` -> output ejs
 
*create if statement with text injection example*

```html
<h2>All blogs</h2>
<% if (blogs.length > 0) {%>
	<% blogs.forEach(blog =>{ %>

		<h3 class='title'> <%= blog.title %> </h3>
		<p class='snippet'> <%= blog.snippet %></p>

	<% }) %> 
<% } %>
<!-- else ....  -->

```

### Ejs partials
Partial parts of the template which are repeated constantly

Requires:
- Create a file called partials
- Create as many ejs files as required with the html/ejs content required
- Declare in the main ejs files the inclusion of the partial template
	+ <%- include(./parials/filename.ejs)) %>



# Middlewares

- Functions/code which get run between the request and the response. 
- This runs top to bottom until the process is finished or we send the response to the browser.
- Every function called on 'app' is a middleware eg .use .get .post etc..

example of middleware which prints stuff for every request made

```js
app.use((req,res, next)=>{
	console.log(req.hostname, req.path, req.method)
	next() // this is to move to the next middleware! Required otherwise express stops here
})
```

---

*Share static files*
In express you can declare a list of files that can be accessed statically example css file
app.use(express.static('public')) // public is folder where static files are

---



# Send data from form in html
```html
<!-- This sends data to endpont /blogs with method post -->
<form action="/blogs" method="POST"> 
	<label for="title">Blog title</label>
	<input type="text" id="title" name="title" required>
	<button>Submit</button>
</form>

```

*Need to add a middleware to accept this data*
`app.use(express.urlencoded({ extended: true }))`
Now you can access with req.body

*In post route*
```js
app.post('/blogs', (req,res)=>{
	const blog = new Blog(req.body)

	blog.save()
	  .then((result)=>{
	  	res.redirect('/blogs') //this redirects to the homepage
	  })
	  .catch((err)=>{
	  	console.log(err)
	  })

```


## Route parameters
ex :id
get it 
req.params.id

##### Redirect in frontend
window.location.href = '/blog' //or whatever










