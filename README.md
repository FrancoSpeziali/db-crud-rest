# Mongoose DB CRUD operations with Restful API

This assignment will give you chance to practise:

- Database

## Expectations

👽 Important!

These assignments are a follow up to the assignment `node-jwt-issuer`

Please complete `node-jwt-issuer`. You will be expected to use the code from `node-jwt-issuer` as the starter code for the following assignments.

## Tasks

## Task 1a - Setting up our middleware (cors)

Since we will be running our frontend on a different domain, we need to inform our `cors()` middleware to accept `credentials` from a different domain.

1. In `server.js`, look for the `cors()` middleware

2. Pass the following object into the `cors` function, replacing <origin> with the URL for your frontend (including port):

```javascript
{ credentials: true, origin: <origin> }
```

## Task 1b - Setting up our middleware (passport & cookie-parser)

1. Install the npm dependencies:

```
passport
cookie-parser
```

2. Import 'cookie-parser' as `cookieParser`, and attach to your middleware in `server.js`

```javascript
app.use(cookieParser());
```

3. Import and attach `passport` to your middleware in `server.js`

```javascript
app.use(passport.initialize());
```

## Task 2 - Reading from the cookie with passport-jwt - Part 1

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

## Task 3 - Reading from the cookie with passport-jwt - Part 2

1. `Strategy` takes 2 parameters, `options` and a `callback`

2. For the `options`, pass in an object, with 2 keys:
    
    `jwtFromRequest: (req) => req.cookies.jwt`
    
    `secretOrKey` - with the secret you used to sign your tokens in `jwtIssuer.js`

3. For the `callback`, create a function which receives 2 arguments, `payload` and `done`
    
    > `payload` contains the contents of the JWT token.

    > `done` is a callback function, which takes 2 parameters - `error` and `user`
    
4. The property `sub` on the object `payload` should contain the user id. Using your  **User** model, search for the user from your database with the user id.
    
    - If the user exists, call the `done()` function with `null` for `error` and the results of the user details for `user`
    
    - if the user does not exist, call the `done()` function, with an appropriate error message for `error` and `false` for the `user`

## Task 4 - Setup the '/dashboard' route

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

## Task 5 - View dashboard route (R in CRUD)

Edit the route you created in `dashboard.js`

1. Create an endpoint called `/view`, which will return all the details of the user

## Task 6 - Protect the route!

We need to authorise that the user is logged in, before returning the data

1. Use the following middleware in your `dashboard` endpoint

```javascript
passport.authenticate('jwt', { session: false });
```

## Task 7 - Add more details to the User Schema

1. Add the following fields to your User Schema

`dateOfBirth` - date
`telephone` - string

## Task 8 - Allow the user to edit their details (U in CRUD)

Edit the route you created in `dashboard.js`

1. Create a new endpoint called `/edit`

    > This route will take inputs from the user, and update the user details

2. Protect the route with the `passport.authenticate()` middleware that you used in **Task 7**

## Task 9 - Allow the user to delete their details (D in CRUD)

Edit the route you created in `dashboard.js`

1. Create a new endpoint called `/delete`

    > This route will delete the user from the database

2. Protect the route with the `passport.authenticate()` middleware that you used in **Task 7**

## Task 10 - Create a React frontend

In this assignment you will create a React frontend which will have

> Note: Your requests to the backend should include the option:
> `credentials: "include"`
> This will tell your browser that it should send any cookies which were saved from the server (with the JWT token) with each request.
> For example, your fetch request might look like this:
> ```javascript
> fetch('http://localhost:3001', {
>     credentials: "include"
> })
> ```

- A register page, which allows the user to register and connects to your `/user/register` endpoint

- A login page, which allows the user to login to the site and connects to your `/user/login` endpoint

- An view page, which allows the user to view their current details, and connects to the `/dashboard/view` endpoint

- An edit page, which allows the user to edit their current details, and connects to the `/dashboard/edit` endpoint

- A delete page, which allows the user to delete their current details, and connects to the `/dashboard/delete` endpoint
