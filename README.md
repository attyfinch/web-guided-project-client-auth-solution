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
* 🐙 [Module Project](https://github.com/LambdaSchool/React-Testing-TV-Show)
* 🐙 [Module Project Solution](https://github.com/LambdaSchool/HTTP-Movies-Assignment)

----

## Guided Project Slack Message
> 1. Clone without forking the following repo: *base guided project repo*
> 2. Navigate into both the review and followAlong folders and run npm i to load dependences.
>
> :point_right: Technical issues spinning up the project? Please head over to the help channel!
> :point_right: If you fall behind during lecture and wish to catch up:
>
> git fetch && git reset --hard origin/lecture
>
> :point_right: Slido event: *insert slido link*

----

## Guided Project Zoom Invitation:
> Unit 3 | Sprint 1 | **Module 1: React Lifecycle**
> _______________________________________________________
> Zoom Link : *insert zoom link*
> Slido: *insert slido link*
> Guided Project: https://github.com/LambdaSchool/web-guided-project-client-auth
> Module Project: https://github.com/LambdaSchool/HTTP-Movies-Assignment

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
* <Switch />
* **<Route />**
* <Link />
* <Redirect />




## Guided Project Outline

### PrivateRoute
(May need to _breifly_ cover `<Switch />` if students haven't seen it)
1. Look through App.js. Note that there are a few routes, one of which we want to make a protected route. In order to do that, we will build a `PrivateRoute` component
2. Create a `PrivateRoute.js` file. In the file, create a basic functional component
3. Tell the students that there are three requirements for this component:

```javascript
import React from 'react';
// Requirements:
// 1. It has the same API as <Route />.
// 2. It renders a <Route /> and passes all the props through to it.
// 3. It checks if the user is authenticated, if they are, it renders
// the “component” prop. If not, it redirects the user to /login.
const PrivateRoute = (props) => {
  return ();
};
export default PrivateRoute;
```

We'll look at these one at a time

4. Requirement one - a component's API is how we "interface" with that component. In other words, when we render it, what props do we pass to it? 
    - This Component needs to be able to interface the same way as the Route component (accept the same props as Route)
    - We will do this is a pretty tricky way - We will deconstruct props, rename the `component` prop as `Component` so that we can render it in jsx, then we will "reconstruct" all the remaining props in an object called `rest`
```javascript
const PrivateRoute = ({ component: Component, ...rest }) => {
  return ();
};
export default PrivateRoute;
```
    - Doing this let's us render `PrivateRoute` in App.js in place of `Route`, and pass it the same props as `Route`

5. Requirement Two - Render `Route` and spread in `rest` to pass it all the props that we passed into `PrivateRoute`
```javascript
import React from 'react';
import { Route } from 'react-router-dom';
// Requirements:
// 1. It has the same API as <Route />.
// 2. It renders a <Route /> and passes all the props through to it.
// 3. It checks if the user is authenticated, if they are, it renders
// the “component” prop. If not, it redirects the user to /login.
const PrivateRoute = ({ component: Component, ...rest }) => {
  return (
    <Route {...rest} />
  );
};
export default PrivateRoute;
```

6. Requirement Three - render the `Component` in the render prop of `Route`. In the callback function we pass into render, we will check localStorage to see if there is a token saved there, which would mean that the user is logged in. If there is, we will render `Component`. If not, we will use the `Redirect` component to redirect the user to '/login'
```javascript
import React from 'react';
import { Route, Redirect, withRouter } from 'react-router-dom';
// Requirements:
// 1. It has the same API as <Route />.
// 2. It renders a <Route /> and passes all the props through to it.
// 3. It checks if the user is authenticated, if they are, it renders
// the “component” prop. If not, it redirects the user to /login.
const PrivateRoute = ({ component: Component, errorStatusCode, ...rest }) => {
  return (
    <Route
      {...rest}
      render={() => {
        if (localStorage.getItem('token')) {
          return <Component />;
        } else {
          // redirect
          console.log('redirecting!!!');
          return <Redirect to="/login" />;
        }
      }}
    />
  );
};
export default PrivateRoute;
```
7. In `App.js`, import `PrivateRoute` and replace the `Route` component for the protected route with `PrivateRoute`. Now you can test your app by going to "localhost:3000/protected" to see if it re-routes you to the login page.
```javascript
<PrivateRoute exact path="/protected" component={GasPrices} />
<Route path="/login" component={Login} />
```

#### Login functionality
8. In `Login.js` look through what we have. Note that the form has an onSubmit handler calling a login function. We'll build that now.
    - It will take in the event and call preventDefault.
    - It needs to make a post request to the server to login
    - It needs to add the returned token to localStorage
```javascript
import React from 'react';

class Login extends React.Component {
  state = {
    credentials: {
      username: '',
      password: ''
    }
  };

  handleChange = e => {
    this.setState({
      credentials: {
        ...this.state.credentials,
        [e.target.name]: e.target.value
      }
    });
  };

  login = e => {
    e.preventDefault();
    axios
      .post('/login', this.state.credentials)
      .then(res => {
        localStorage.setItem('token', res.data.payload);
        // redirect to the apps main page?
        this.props.history.push('/protected');
      })
      .catch(err => console.log(err));
  };

  render() {
    return (
      <div>
        <form onSubmit={this.login}>
          <input
            type="text"
            name="username"
            value={this.state.credentials.username}
            onChange={this.handleChange}
          />
          <input
            type="password"
            name="password"
            value={this.state.credentials.password}
            onChange={this.handleChange}
          />
          <button>Log in</button>
        </form>
      </div>
    );
  }
}

export default Login;
```

### Axios Config
9. Create a `axiosWithAuth.js` file
10. Build and export a function that returns the `.create()` method on `axios` to create a new instance of axios that we can use throughout our app
  - Pass in the config object with a baseUrl and authentication header
  
```javascript
import axios from 'axios';

// Build a module that "creates" a new "instance" of the axios object
// - baseURL
// - headers object -> authorization header with the token

export const axiosWithAuth = () => {
  const token = localStorage.getItem('token');

  return axios.create({
    baseURL: 'http://localhost:5000/api',
    headers: {
      Authorization: token
    }
  });
};
```

#### Getting data with an authentication token:
11. In `GasPrices.js` note that we are calling the `getData` function in componentDidMount. We still need to build that out.
    - instead of the axios.get call use `axiosWithAuth().get`
```javascript
  getData = () => {
    axiosWithAuth()
      .get('/data')
      .then(res => {
        this.setState({
          gasPrices: res.data.data.filter(
            price =>
              price.type === 'Gasoline - Regular' &&
              (price.location === 'US' || price.location === 'State of Hawaii')
          )
        });
      })
      .catch(err => console.log(err));
  };
```

12. After you finish this, go to the app, open the dev tools and login. You should see the chart built out now. Go to the network tab and find the network request called 'data'. Show that there is the authentication header with the token passed to it. That's how the server was able to authenticate the user.


### Module Project Review
* [base project repo](#)

## Breakout Slack Messages

----

## After Class Message
Hope you all enjoyed today's guided Lesson!
A reminder if that office hours are from 3:30 - 4:30 Lambda Time. Don't forget to complete the days Check for Understanding and Pulse Checks! 

Module Project
https://github.com/LambdaSchool/HTTP-Movies-Assignment

Here is a review of today's material.

Key Terminology
* 📝 *term* - [description](#)

Key Concepts
* 📝 *concept* - [description](#)