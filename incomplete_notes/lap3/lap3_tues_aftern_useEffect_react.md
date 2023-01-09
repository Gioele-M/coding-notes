# useEffect
Mainly used to fetch data

## Axios
Substitute of fetch

`npm install axios`
Can be used both in node and browser


### Example of useEffect

```jsx
import './App.css'
import React, {useEffect, useState} from 'react'
import axios from 'axios'


function App(){
	useEffect(()=>{

		//Set useState
		const [students, setStudents] = useState([])

		//useState for input
		const [cohort, setCohort] = useState('')


		//useSTate for search term
		const [search, setSearch] = useState('')


		//State for handling errors
		const [error, setError] = useState('')

		//Status to communicate with the users
		const [statusMessage, setStatusMessage] = useState('Loading')



		//Given that useEffect does not take a async function as callback we need to make one inside

		const fetchStudents = async (searchTerm) =>{
			//On page reload, if there is no search term use that
			searchTerm ||= 'auguste'

			try{

				//URL declaration
				const URL = `https://raw.githubusercontent.com/getfutureproof/fp_study_notes_hello_github/main/${searchTerm}/roster.json`


				//This is the fetch request, returns a response
				const response = awaitaxios.get(URL)
				console.log(response)

				//can also straight destructure data from response
				const { data } = await axios.get(URL)
				console.log(data)

				//Set data as with useState function
				setStudents(data.students)

				//Empty the status message as not loading anymore
				setStatusMessage('')

			}catch(err){
				//set error
				setError(err)
				//Set status message
				setStatusMessage('Loading')
				console.log(err)
			}

		}

		//Set timeout for loading errors when calling app
		//Store ID in variable to 'clean after yourself'

		const timeoutId = setTimeout(()=>{
			fetchStudents(search)
		}, 3000)

		//Clean thing by

		return() =>{
			clearTimeout(timeoutId)
		}



	}, 
	[search]) //Called dependency array
	// Checks on values and runs if changes
	
	// Other cases for the dependecy array
	// empty -> runs in loop
	// [] -> runs once
	// [value, value2] -> will run every time the value changes (can have >1 variable)



	//Function to map students in lis
	const renderedStudents = students.map(st => {
		return <li key={st.github}>{st.name}</li>
	})//Keys are necessary for react to recognise elements



	//Function to handle input and set it as cohort of reference
	const onInputChange = () =>{
		setCohort(e.target.value)


	}

	//Function to handle form sumbit and preventing page refresh
	const onFormSubmit = (e)=>{
		e.preventDefault()
		//Set the cohort as value
		setSearch(cohort)
		//Clean cohort storage
		setCohort('')
	}


	return(
		<div className="App">
			<header className="App-header">
				//If there's error show to user, otherwise load status message and rendered students

				{ error 
					? <h1>Sorry, we could not find a(n) {search} cohort <h3>
					//else block if not error
					//if status message show it, after render students
					: <h3> {statusMessage ? statusMessage : ''} </h3> 
					<ul>
					{renderedStudents}
					</ul>
				}
 
				//If status message show it, otherwise dont
				<h3> { statusMessage ? statusMessage : '' } </h3>

				<ul>
					{renderedStudents}
				</ul>

				<form onSubmit={onFormSubmit}> //Callback for form submission
					<label htmlFor="cohort">Cohort</label>
					<input 
						type="text" 
						id="cohort" 
						value={cohort}//The value can be stored in cohort, previously declared in the useState
						onChange={onInputChange}//Function to handle input
						/> 
					<button></button>
				</form>

			</header>
		</div>
	)



}


```