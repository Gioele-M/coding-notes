# Authorisation and authentication
Authorisation: check that someone is allowed to be there
Authentication: check that someone is who they say to be


*GDPR:*
General data protection regulation:
- Important hackers don't find a backdoor


*ICO*
Information commissioner office (?)


## Authorisation types
The main difference is session-based authentication of the connection stores the authentication details. The session method makes the server store most of the details, while in the case of the token-based one the client stores them.


*Session -> server side*
- Requires credential 


*Token -> client side*
- Requires credential and sends token to client to access data

******* Look at this again
----------



## Preparing passwords for storage

- Get user password
- Add salt
	+ complexity eg random characters
- Hash password+salt
	+ Stored hashed pass and salt for unaccessibility




### Demo bcrypt.js container
Implement password encription into container
`npm i bcryptjs`

*Provisioning an app:* (?) check!

###### In controller
```js
const bcrypt = require('bcryptjs') //-> for digesting and comparing passwords

//import router
const router = request('express').Router()

//import model object
const User = require('../models/user')




// Register user and hash password
router.post('/register', async(req,res)=>{
	try{
		//Add the salt
		const salt = await bcrypt.genSalt()
		//Hash the password
		const hashed = await bcrypt.hash(req.body.password, salt) //Can add additional arguments
		//Create object to send to db
		await User.create({...req.body, password:hashed})
		//Send to server
		res.status(201).json({msg: 'User created'})
	}catch(err){
		res.status(500).json({err})
	}
})


//Login route for authentication of encripted passwords
router.post('/login', async(req,res)=>{
	try{
		const user = await User.findByEmail(req.body.email)
		if(!user){
			throw new Error('Not found')
		}

		//This compares and sees for itself to add the salt
		const authed = await bcrypt.compare(req.body.passwordm user.passwordDigest)

		if(!!authed){ //!! is strict not
			res.status(200).json({user: user.name})
		}else{
			throw new Error('not authenticated')

		}

	}catch(err){
		res.status(401).json({err: err.message})
	}
})


```



### Encoding components
*Token components* 

*Header* (metadata eg type of algorithm used and all that)


*Payload* (cargo of data to deliver)


*Signature* (combination of header and payload to be verified)



### Demo jsonwebtoken
Install npm package first
`npm i jsonwebtoken`
Need also dotenv package
`npm i dotenv`

*Again in controller after all we had before*
_This basically sends a token to the user instead of sending data directly, complete before's login route_
_Token is destroyed at logout and a new one created at login_
	- This token is stored in the localStorage of the user 

```js
const jwt = require('jsonwebtoken')
const dotenv = require('dotenv')


//If the user authenticates we send a token
//Login route for authentication of encripted passwords 
//For this to work it *REQUIRES* an .env file with variable (requires npm i dotenv)
router.post('/login', async(req,res)=>{
	try{
		const user = await User.findByEmail(req.body.email)
		if(!user){
			throw new Error('Not found')
		}
		const authed = await bcrypt.compare(req.body.passwordm user.passwordDigest)

		if(!!authed){ //?? opposite?
			//////////////////////////////////// Changes compared to before
			//////////////////////////////////// Maybe in here it requires the environment variable to be added
			const payload = {username: user.name, email: user.email}
			const sendToken = (err, token) => {
				if(err){throw new Error('Error in token generation:' + err)}

				res.status(200).json({
					success: true,
					token: "Bearer" + token
				})

			}

		}else{
			throw new Error('not authenticated')

		}

	}catch(err){
		res.status(401).json({err: err.message})
	}
})


// IN INDEX.JS REQUIRE dotenv!!!!!
const dotenv = require('dotenv')


// .env file contains
SECRET=process.env.SECRET




//IN HTML REQUIRE JWT-DECODE LIBRARY WITH SCRIPT:SRC (?)
<script src='jwt-decode-link etc get it from documentation'>


// Jwt-decode is called in the getall route, we want to make sure that only authenticated users can access this!!
// Client - JS - Static - Request.js in here its called in repo

function login(data){
	const payload = jwt_decode(data.token) // Imported in html
	localStorage.setItem('token', data.token)
	localStorage.setItem('username', payload.username)
	localStorage.setItem('email', payload.email)
	location.hash = '#feed'
}

function logout(){
	localStorage.clear()
	location.hash = '#login'
}


```



*IN CONTROLLER*
Need to verify token in controller, requires middleware for authentication
```js
//router import ...
const {verifyToken} = require('../middleware/auth') // created underneath we get it from

router.get('/', verifyToken, async(req,res)=>{
	try{
		const posts = await Post.all

	}catch(err){
		res.status(500).send({err})
	}
})


```

*In Middleware file found in api*
_Just a authoriser that checks token_
```js
const jwt = require('jsonwebtoken')

function verifyToken(req, res, next){
	const header = req.headers['authorization']
	if(header){
		const token = header.split(' ')[1] //Second element bc bearer word before token
		jwt.verify(token, process.env.SECRET, async(err, data) =>{
			if(err){
				res.status(403).json({err: 'Invalid token'})
			}else{
				next()
			}
		})
	}else{
		res.status(403).json({err: 'Missing token'})
	}
}

module.exports = verifyToken
```



## Needs new file .env for secret environment
Add this file to gitignore!!!

```
SECRET=process.env.SECRET
```



REST Client -> API extension