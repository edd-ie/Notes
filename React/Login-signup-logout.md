#react #authentification  [Video Guide](https://www.youtube.com/playlist?list=PLgYiyoyNPrv_yNp5Pzsx0A3gQ8-tfg66j)

## Initializing App

- Set up the web state and user data states in **App.jsx**
```javascript
function App(){
	const [loggedIn,setLoggedIn] = useState(false)
	const [user,setUser] = useState({})
	
	... 

}
```
- This data will be sent by login & signup form:
	- _Logged In_ -  tells the up if user is sign in or not
	- _User_ - hold user data to be used throughout the app


## Sign up

- Add the user and status props on the signup function
```javascript
//Route: 
<SignUp status={(e)=>setLoggedIn(e)} user={(e)=>setUser(e)}/>

//Component
export default function SignUp({user, status}) {  //Props sent to App.jsx
	...
}
```

- Create the form and UI etc
- For handle submit :
```javascript
function handleSubmit(e){
        e.preventDefault()

        let form = e.target
        let name = form[0].value
        let email = form[1].value
        let pass = form[2].value
        let pass_conf = form[3].value

        let data = {
            username: name,
            email: email,
            password: pass,
            password_confirmation: pass_conf
        }
  
        fetch('http://127.0.0.1:3000/signups',{
            method: 'POST',
            headers: {'Content-Type': 'application/json'},
            body: JSON.stringify(data),
            withCredentials: true
        })
        .then(res=>{
            return res.ok ? (res.json()
            .then(data=>{
                status(true)  // Login status sent to App.jsx
                user(data) // user data sent to App.jsx
            })
            ) : Promise.reject(res)
        })
       form.reset()
    }
```

## Login

- Add the user and status props on the login function
```javascript
//Route: 
<Login status={(e)=>setLoggedIn(e)} user={(e)=>setUser(e)}/>

//Component
export default function Login({user, status}) {  //Props sent to App.jsx
	...
}
```

Create the UI
- For the form handle Submit( ): 
```js
function handleSubmit(e){
        e.preventDefault() 

        let form = e.target
        let email = form[0].value
        let pass = form[1].value

        let data = {
            email: email,
            password: pass
        }  

        fetch('http://127.0.0.1:3000/login',{
            method: 'POST',
            headers: {'Content-Type': 'application/json'},
            body: JSON.stringify(data),
            withCredentials: true
        })
        .then(res=>{
            return res.ok ? (res.json()
            .then(data=>{
                status(true) // Login status sent to App.jsx
                user(data) // User data sent to App.jsx
            })
            ) : Promise.reject(res)
        })
        
        form.reset()
    }
```


## Checking if user is logged in

Checking if the cookie is saved in the browser
- Go to the root component --> _App.jsx_
- 