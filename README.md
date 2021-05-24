# Sprint 3 - Module 2 : Client Side Authentication

## [Training Kit](https://github.com/LambdaSchool/Full-Stack-Web-Curriculum/tree/main/03-WebApplications-II/Sprint%2003%20-%20Advanced%20Web%20Applications/Module%202%20-%20Client-Side%20Auth)

### [Previous Lesson Plan](https://github.com/LambdaSchool/Web-GuidedProject-ClientSideAuth)

----

## Objectives

By the end of this module, learners should be able to:
* handle authentication with tokens in a React app
* implement protected routes using an authentication token and Redirect

----

## Instructor Resources
* 🐙 [Guided Project Starter](https://github.com/LambdaSchool/web-guided-project-client-auth)
* 🐙 [Guided Project Solution](https://github.com/LambdaSchool/web-guided-project-client-auth-solution)
* 🐙 [Module Project](https://github.com/LambdaSchool/web-module-project-client-auth)
* 🐙 [Module Project Solution](https://github.com/LambdaSchool/web-module-project-client-auth-solution)

----

## Guided Project Slack Message
> **Welcome Unit 3!**
> 1. Clone without forking the following repo: https://github.com/LambdaSchool/web-guided-project-client-auth
> 2. Navigate into both the server and client folders and run npm i to load dependences.
>
> :point_right: Technical issues spinning up the project? Please head over to the help channel!
>
> :point_right: If you fall behind during lecture and wish to catch up:
>
> `git fetch && git reset --hard origin/lecture`
>
> :point_right: Slido event: https://app.sli.do/event/msea4w8q

----

## Guided Project Zoom Invitation:
> **Unit 3 | Sprint 3 | Module 2: React Lifecycle**
> _______________________________________________________
> Zoom Link : https://lambdaschool.zoom.us/my/unit3withwarren
>
> Slido: https://app.sli.do/event/msea4w8q
>
> Guided Project: https://github.com/LambdaSchool/web-guided-project-client-auth
>
> Module Project: https://github.com/LambdaSchool/web-module-project-client-auth

----

## Check for Understanding Questions

These are the questions used internally to check student understanding. Students will be instructed to answer these questions after the guided project. Please make sure to emphasize the concepts behind these answers.

#### What kind of tokens did we use to check if a user is authenticated?
* **JSON web token (JWT)**
* Sessions
* Auth0 Tokens
* Cookies

#### What function can you call to create a new instance of axios with a custom configuration?
* create(axios)
* **axios.create()**
* axios.new()
* new(axios)

#### What header do we use with our API calls to send the token to the server?
* Token
* **Authentication**
* Header
* Authorization

#### What component does PrivateRoute replace to reroute users from a specific path if they are not authenticated?
* Switch
* **Route**
* Link
* Redirect

## Guided Project Outline

### Module Review
* The beginings of covering security
* Focusing on the clientside of that path

### Define Authentication
* The process of determining the identity
* Not the same as authorization

### Walkthrough of Authentication Workflow
* Start with login.
* Handle the request
* Handle logout.

### Indicate JWT
* jwt.io
* Indicate that these are one way encrption

### Take a tour of our server code
* Express server
* Indicate each endpoints
* Indicate the login endpoint
* Indicate the request
* Indicate the logout

### Take a tour of our client code
* Indicate login, logout, and protected page.
* Tour App.js, Login.js and GasPrices.js

### BREAK

### Start with our login process
* Makes sure that the server is running.
* Navigate to the Login component.
* Discuss where we locate the login code.
* Add in click handling.
* Prompt students to add in an axios call to get a post response from login.
* Go over result:
```js
  axios.post('http://localhost:5000/api/login', this.state)
    .then(res=>{
      console.log(res);
    })
```
* Notice that we get different results if we pass in correct or incorrect data.
* Notice that on correct logins we get back a token.
* Prompt how we can save our login token - localStorage.

### Brief Review of LocalStorage
* localStorage.setItem(key, value)
* localStorage.getItem(key)
* localStorage.removeItem(key)
* implement localStorage for storing token

### Handle login redirect
* this.props.history.push('/protected');

### BREAK

### Handle authentiation request
* Note that we want to load data immediately
* Add in axios call to getData
```js
  axios.get("http://localhost:5000/api/data", {
    headers: {
      authorization: localStorage.getItem("token");
    }
  })
```
* Get data and add it to state.
```js
this.setState({
  gasPrices: req.data.data
})
```

### Discuss login path
* We have a function for logout
* Add an axios call for logout
```js
  axios
    .post('http://localhost:5000/api/logout')
    .then(req=>{
      console.log(req);
    })
```
* Make sure to delete our token as well.
* Redirect
```js
  window.location.href = "/login"
```

### Make an axios util for authentintication api calls
* Make utils folder
* axiosWithAuth.js
* Add a util that returns an axios request with Auth sent.
```js
export const axiosWithAuth = ()=> {
  const token = localStorage.getItem('token');
  return axios.create({
    headers:{
      authorization: token
    },
    baseURL: 'http://localhost:5000/api'
  });
}
```
* Import in axiosWithAuth for all requests.

### BREAK

### Introduce Private Routes as a concept
* It is good that our data is protected
* We also don't want user to access pages with protected data (potenially) on them.
* Be good to make a general util for protecting pages.
* Similar to a custom hook.

### Build PrivateRoute
* Add PrivateRoute.js as a component.
* import react and route.
* create privateRoute function
```js
export const PrivateRoute = ({component: Component}, ...rest) => {
  return <Route 
    {...rest}
    render={(props)=>{
      if (localStorage.getItem('token')) {
        return <Component {...props}/>;
      } else {
        return <Redirect to="/login"/>;
      }
    }
  />
}
```
* Implement PrivateRoute
```js
  <PrivateRoute exact path="/protected" component={GasPrices}/>
```



### Module Project Review
* [base project repo](#)

## Breakout Slack Messages

----

## After Class Message
**Hope you all enjoyed today's guided Lesson!**
**A reminder if that office hours are from 2:30 - 3:00 Lambda Time.** 
**Don't forget to complete the days Check for Understanding and Pulse Checks!**

**Here is a review of today's material.**

### Module Materials
Module Project: https://github.com/LambdaSchool/web-module-project-client-auth

GP Slides: https://docs.google.com/presentation/d/1pWlfJhuZpeX054EgZGjElnWuzNPX2YQ1UXkUSw9QXBk/edit?usp=sharing

Monday's Module Project Review: https://www.loom.com/share/e01d193711004a64ba265329f5337329 PW: lSwXpF

Alternative GP Video: [Dustin Myers](https://youtu.be/ATr7QQP7lw4) | [Josh Jacobson](https://youtu.be/0AvJFNiqhD4)| [Warren C. Longmire](https://youtu.be/zVSH2n1wZCY) | [Brian Kirkby](https://youtu.be/hGICwuTH6Ag)

### Key Terminology
* 📝 *...rest* - [A means to get capture the remaining values within a javascript array or object easily.](https://medium.com/wesionary-team/spread-and-rest-operator-in-javascript-db3f15cec185)
* 📝 *Redirect Route* - [A redirect method used through react-router.](https://medium.com/@alexfarmer/redirects-in-react-router-dom-46198938eedc)
* 📝 *this.history redirect* - [A redirect method used through Route props.](https://www.codesd.com/item/react-this-props-history-push-does-not-redirect.html)
* 📝 *window.location redirect* - [A redirect method used through the windows location object.](https://developer.mozilla.org/en-US/docs/Web/API/Window/location)
* 📝 *Route* - [A react router component that allows programmers to connect a component to a url path](https://reactrouter.com/web/api/Route)
* 📝 *axios.create* - [A means to create a stub of an axios call with preset values attached](https://masteringjs.io/tutorials/axios/create)
* 📝 *jwt tokens* - [The current web standard for encrypted authentication tokens](https://dzone.com/articles/what-is-jwt-token)

### Key Concepts
* 📝 *Token Saving* - [A great writeup on techniques in saving authentication tokens.](https://medium.com/@ryanchenkie_40935/react-authentication-how-to-store-jwt-in-a-cookie-346519310e81)
* 📝 *Authentication* - [The process for identifying user identity.](https://www.youtube.com/watch?v=woNZJMSNbuo)
* 📝 *Authorization* - [The process for identifying user permissions.](https://www.youtube.com/watch?v=I0poT4UxFxE)
* 📝 *http headers* - [Additional data added to http requests for interperation within your backend code](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)
