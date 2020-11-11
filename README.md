# Mongoose DB CRUD operations with Restful API

This assignment will give you chance to practise:

- Sending a JWT token via a httpOnly cookie
- Database

## Expectations

👽 Important!

These assignments are a follow up to the assignment `node-jwt-issuer`

Please complete `node-jwt-issuer`. You will be expected to use the code from `node-jwt-issuer` as the starter code for the following assignments.

## Assignments

## Assignment 1 - httpOnly JWT

In the last assignment, we planned to send the JWT token as part of the response, and handle it in the frontend (via localStorage).

We will switch this approach to use `httpOnly` cookies

In your `jwtIssuer.js` file

1. Change the returned value for the key `token` from `'Bearer' + signedToken` to just `signedToken`

We no longer need to send the `"Bearer"` string since we will not be manually sending the token in the request Auth header

2. Modify the login route to respond with the method `response.cookie()`

Research: [Express.js response.cookie() method [en]](http://expressjs.com/en/4x/api.html#res.cookie)

The `cookie()` method takes 3 parameters, `name`, `value` and `options`.

 - For `name` use the string `jwt`
 - For `value` pass in your JWT token
 - For `options`, use the object:
```javascript
{
    httpOnly: true,
    sameSite: 'lax'
}
```

This will instruct the client (browser) to set the JWT token as a `httpOnly` cookie

Once the user logs in with this route, all future requests will include this `httpOnly` cookie

## Assignment 2 - Setting up passport

1. Install the dependencies:

```
passport
```

2. Import and attach `passport` to your middleware in `server.js`

```javascript
app.use(passport.initialize());
```

## Assignment 3 - Reading from the cookie with passport-jwt - Part 1

We will use `passport-jwt` to read the token from the request header

1. Install the dependencies:

```
passport-jwt
```

2. Create the file `config/passport-jwt.js`

3. Inside this file, import the named import `Strategy` from `passport-jwt`

4. `Strategy` is a class. Create an instance of `Strategy` called `jwtStrategy`

5. Export your instance

6. Import your instance into `server.js`

7. Use the following code **before** your middleware

```javascript
passport.use(jwtStrategy);
```

## Assignment 4 - Reading from the cookie with passport-jwt - Part 2

1. `Strategy` takes 2 parameters, `options` and a `callback`

2. For the `options`, pass in an object, with 2 keys:
    
    `jwtFromRequest: (req) => req.cookies.jwt`
    
    `secretOrKey` - with the secret you used to sign your tokens in `jwtIssuer.js`

3. For the `callback`, create a function which receives 2 arguments, `payload` and `done`
    
    > `payload` contains the contents of the JWT token.

    > `done` is a callback function, which takes 2 parameters - `error` and `user`
    
4. The property `sub` on the object `payload` should contain the user id. Search for the user from your database using the user id.
    
    - If the user exists, call the `done()` function with `null` for `error` and the results of the user details for `user`
    
    - if the user does not exist, call the `done()` function, with an appropriate error message for `error` and `false` for the `user`

## Assignment 5 - Setup the '/dashboard' route

We will create a new route called `dashboard` and protect it from unauthorised users

1. Create a file `dashboard.js` in a folder `routes`

This will contain all the dashboard routes

2. Import `express`

3. Use the following code to create a route

```javascript
const router = express.Router();
```

4. Export `router` from `user.js`

5. Inside `server.js`, import `router` from `dashboard.js`

6. Use `app.use()` to redirect all `/dashboard` routes to the `router` variable you exported from `dashboard.js`

## Assignment 6 - View dashboard route (R in CRUD)

Edit the route you created in `dashboard.js`

1. Create an endpoint called `/view`, which will return all the details of the user

## Assignment 7 - Protect the route!

We need to authorise that the user is logged in, before returning the data

1. Use the following middleware in your `dashboard` endpoint

```javascript
passport.authenticate('jwt', { session: false });
```

## Assignment 8 - Add more details to the User Schema

1. Add the following fields to your User Schema

`dateOfBirth` - date
`telephone` - string

## Assignment 9 - Allow the user to edit their details (U in CRUD)

Edit the route you created in `dashboard.js`

1. Create a new endpoint called `/edit`

    > This route will take inputs from the user, and update the user details

2. Protect the route with the `passport.authenticate()` middleware that you used in **Assignment 7**

## Assignment 10 - Allow the user to delete their details (D in CRUD)

Edit the route you created in `dashboard.js`

1. Create a new endpoint called `/delete`

    > This route will delete the user from the database

2. Protect the route with the `passport.authenticate()` middleware that you used in **Assignment 7**

## Assignment 11 - Create a React frontend

In this assignment you will create a React frontend which will have

- A register page, which allows the user to register and connects to your `/user/register` endpoint

- A login page, which allows the user to login to the site and connects to your `/user/login` endpoint

- An view page, which allows the user to view their current details, and connects to the `/dashboard/view` endpoint

- An edit page, which allows the user to edit their current details, and connects to the `/dashboard/edit` endpoint

- A delete page, which allows the user to delete their current details, and connects to the `/dashboard/delete` endpoint
