# Concepts

## Authenticate

Authenticating requests is as simple as calling `passport.authenticate()` and specifying which strategy to employ.`authenticate()`'s function signature is standard [Connect](http://www.senchalabs.org/connect/) middleware, which makes it convenient to use as route middleware in [Express](http://expressjs.com/) applications.

```js
app.post('/login',
  passport.authenticate('local'),
  function(req, res) {
    // If this function gets called, authentication was successful.
    // `req.user` contains the authenticated user.
    res.redirect('/users/' + req.user.username);
  });
```

### Redirects

```js
app.post('/login',
  passport.authenticate('local', { successRedirect: '/',
                                   failureRedirect: '/login' }));
```

### Flash Messages

```js
app.post('/login',
  passport.authenticate('local', { successRedirect: '/',
                                   failureRedirect: '/login',
                                   failureFlash: true })
);
//or
passport.authenticate('local', { failureFlash: 'Invalid username or password.' });
//or
passport.authenticate('local', { successFlash: 'Welcome!' });
```

**Note:** Using flash messages requires a `req.flash()` function. Express 2.x provided this functionality, however it was removed from Express 3.x. Use of [connect-flash](https://github.com/jaredhanson/connect-flash) middleware is recommended to provide this functionality when using Express 3.x.

### Disable Sessions

```js
app.get('/api/users/me',
  passport.authenticate('basic', { session: false }),
  function(req, res) {
    res.json({ id: req.user.id, username: req.user.username });
  });
```

### Custom Callback

```js
app.get('/login', function(req, res, next) {
  passport.authenticate('local', function(err, user, info) {
    if (err) { return next(err); }
    if (!user) { return res.redirect('/login'); }
    req.logIn(user, function(err) {
      if (err) { return next(err); }
      return res.redirect('/users/' + user.username);
    });
  })(req, res, next);
});
```

In this example, note that `authenticate()` is called from within the route handler, rather than being used as route middleware. This gives the callback access to the `req` and `res` objects through closure.

If authentication failed, `user` will be set to `false`. If an exception occurred, `err` will be set. An optional `info`argument will be passed, containing additional details provided by the strategy's verify callback.

The callback can use the arguments supplied to handle the authentication result as desired. Note that when using a custom callback, it becomes the application's responsibility to establish a session (by calling `req.login()`) and send a response.

## Configure

Three pieces need to be configured to use Passport for authentication:

1. Authentication strategies
2. Application middleware
3. Sessions (*optional*)

### Strategies

Strategies, and their configuration, are supplied via the `use()` function. For example, the following uses the `LocalStrategy` for username/password authentication.

```js
var passport = require('passport')
  , LocalStrategy = require('passport-local').Strategy;

passport.use(new LocalStrategy(
  function(username, password, done) {
    User.findOne({ username: username }, function (err, user) {
      if (err) { return done(err); }
      if (!user) {
        return done(null, false, { message: 'Incorrect username.' });
      }
      if (!user.validPassword(password)) {
        return done(null, false, { message: 'Incorrect password.' });
      }
      return done(null, user);
    });
  }
));
```

### Verify Callback

When Passport authenticates a request, it parses the credentials contained in the request. It then invokes the verify callback with those credentials as arguments, in this case `username` and `password`. If the credentials are valid, the verify callback invokes `done` to supply Passport with the user that authenticated.

```js
return done(null, user);
```

If the credentials are not valid (for example, if the password is incorrect), `done` should be invoked with `false`instead of a user to indicate an authentication failure.

```js
return done(null, false);
```

An additional info message can be supplied to indicate the reason for the failure. This is useful for displaying a flash message prompting the user to try again.

```js
return done(null, false, { message: 'Incorrect password.' });
```

Finally, if an exception occurred while verifying the credentials (for example, if the database is not available), `done` should be invoked with an error, in conventional Node style.

```js
return done(err);
```

### Middleware

```js
var session = require("express-session"),
    bodyParser = require("body-parser");

app.use(express.static("public"));
app.use(session({ secret: "cats" }));
app.use(bodyParser.urlencoded({ extended: false }));
app.use(passport.initialize());
app.use(passport.session());
```

In a [Connect](http://senchalabs.github.com/connect/) or [Express](http://expressjs.com/)-based application, `passport.initialize()` middleware is required to initialize Passport. If your application uses persistent login sessions, `passport.session()` middleware must also be used. be sure to use `session()` *before* `passport.session()` to ensure that the login session is restored in the correct order

### Session

Each subsequent request will not contain credentials, but rather the unique cookie that identifies the session. In order to support login sessions, Passport will serialize and deserialize `user` instances to and from the session.

```js
passport.serializeUser(function(user, done) {
  done(null, user.id);
});

passport.deserializeUser(function(id, done) {
  User.findById(id, function(err, user) {
    done(err, user);
  });
});
```

In this example, only the user ID is serialized to the session, keeping the amount of data stored within the session small. When subsequent requests are received, this ID is used to find the user, which will be restored to `req.user`.

# Strategies

## Local

```js
var passport = require('passport')
  , LocalStrategy = require('passport-local').Strategy;

passport.use(new LocalStrategy(
  function(username, password, done) {
    User.findOne({ username: username }, function(err, user) {
      if (err) { return done(err); }
      if (!user) {
        return done(null, false, { message: 'Incorrect username.' });
      }
      if (!user.validPassword(password)) {
        return done(null, false, { message: 'Incorrect password.' });
      }
      return done(null, user);
    });
  }
));
```

By default, `LocalStrategy` expects to find credentials in parameters named `username` and `password`. If your site prefers to name these fields differently, options are available to change the defaults.

```js
passport.use(new LocalStrategy({
    usernameField: 'email',
    passwordField: 'passwd'
  },
  function(username, password, done) {
    // ...
  }
));
```

#### Passport-local-mongoose

##### Plugin

```js
const mongoose = require('mongoose');
const Schema = mongoose.Schema;
const passportLocalMongoose = require('passport-local-mongoose');

const User = new Schema({});

User.plugin(passportLocalMongoose);

module.exports = mongoose.model('User', User);
```

You're free to define your User how you like. Passport-Local Mongoose will add a username, hash and salt field to store the username, the hashed password and the salt value.

##### Configure

```js
// requires the model with Passport-Local Mongoose plugged in
const User = require('./models/user');

// use static authenticate method of model in LocalStrategy
passport.use(new LocalStrategy(User.authenticate()));

// use static serialize and deserialize of model for passport session support
passport.serializeUser(User.serializeUser());
passport.deserializeUser(User.deserializeUser());
```

##### Options

```js
User.plugin(passportLocalMongoose, options);
```

- saltlen: specifies the salt length in bytes. Default: 32
- iterations: specifies the number of iterations used in pbkdf2 hashing algorithm. Default: 25000
- keylen: specifies the length in byte of the generated key. Default: 512
- digestAlgorithm: specifies the pbkdf2 digest algorithm. Default: sha256. (get a list of supported algorithms with crypto.getHashes())
- interval: specifies the interval in milliseconds between login attempts, which increases exponentially based on the number of failed attempts, up to maxInterval. Default: 100
- maxInterval: specifies the maximum amount of time an account can be locked. Default 30000 (5 minutes)
- usernameField: specifies the field name that holds the username. Defaults to 'username'. This option can be used if you want to use a different field to hold the username for example "email".
- usernameUnique : specifies if the username field should be enforced to be unique by a mongodb index or not. Defaults to true.
- saltField: specifies the field name that holds the salt value. Defaults to 'salt'.
- hashField: specifies the field name that holds the password hash value. Defaults to 'hash'.
- attemptsField: specifies the field name that holds the number of login failures since the last successful login. Defaults to 'attempts'.
- lastLoginField: specifies the field name that holds the timestamp of the last login attempt. Defaults to 'last'.
- selectFields: specifies the fields of the model to be selected from mongodb (and stored in the session). Defaults to 'undefined' so that all fields of the model are selected.
- usernameLowerCase: convert username field value to lower case when saving an querying. Defaults to 'false'.
- populateFields: specifies fields to populate in findByUsername function. Defaults to 'undefined'.
- encoding: specifies the encoding the generated salt and hash will be stored in. Defaults to 'hex'.
- limitAttempts: specifies whether login attempts should be limited and login failures should be penalized. Default: false.
- maxAttempts: specifies the maximum number of failed attempts allowed before preventing login. Default: Infinity.
- passwordValidator: specifies your custom validation function for the password in the form 'function(password,cb)'. Default: validates non-empty passwords.
- usernameQueryFields: specifies alternative fields of the model for identifying a user (e.g. email).
- findByUsername: Specifies a query function that is executed with query parameters to restrict the query with extra query parameters. For example query only users with field "active" set to `true`. Default: `function(model, queryParameters) { return model.findOne(queryParameters); }`. See the examples section for a use case.

##### Instance methods

###### setPassword(password, [cb])

Sets a user password. Does not save the user object. If no callback `cb` is provided a `Promise` is returned.

###### changePassword(oldPassword, newPassword, [cb])

Sets a change a user's password hash and salt. Does not save the user object. If no callback `cb` is provided a `Promise` is returned. If oldPassword does not match the user's old password an `IncorrectPasswordError` is passed to `cb` or the `Promise` is rejected.

###### authenticate(password, [cb])

Authenticate a user object. If no callback `cb` is provided a `Promise` is returned.

###### resetAttempts([cb])

Reset a user's number of failed password attempts (only defined if `options.limitAttempts` is true) If no callback `cb` is provided a `Promise` is returned.

##### Static methods

- authenticate() Generates a function that is used in Passport's LocalStrategy
- serializeUser() Generates a function that is used by Passport to serialize users into the session
- deserializeUser() Generates a function that is used by Passport to deserialize users into the session
- register(user, password, cb) Convenience method to register a new user instance with a given password. Checks if username is unique. See [login example](https://github.com/saintedlama/passport-local-mongoose/tree/master/examples/login).
- findByUsername() Convenience method to find a user instance by it's unique username.
- createStrategy() Creates a configured passport-local `LocalStrategy` instance that can be used in passport.

#### Process

1. Setup mongoose user model

```js
const mongoose = require('mongoose');
const Schema = mongoose.Schema;
const passportLocalMongoose = require('passport-local-mongoose');

const User = new Schema({});

User.plugin(passportLocalMongoose);

module.exports = mongoose.model('User', User);
```



2. Setup passport middleware

```js
app.use(session({keys: ['secretkey1', 'secretkey2', '...']}));
app.use(passport.initialize());
app.use(passport.session());
```



3. Setup passport strategies

```js
const User = require('./models/user');

// CHANGE: USE "createStrategy" INSTEAD OF "authenticate"
passport.use(User.createStrategy());

passport.serializeUser(User.serializeUser());
passport.deserializeUser(User.deserializeUser());
```



4.Mongoose connects database

# Operations

## Log in

```js
req.login(user, function(err) {
  if (err) { return next(err); }
  return res.redirect('/users/' + req.user.username);
});
```

When the login operation completes, `user` will be assigned to `req.user`.

Note: `passport.authenticate()` middleware invokes `req.login()` automatically. This function is primarily used when users sign up, during which `req.login()` can be invoked to automatically log in the newly registered user.

## Log out

```js
app.get('/logout', function(req, res){
  req.logout();
  res.redirect('/');
});
```

Passport exposes a `logout()` function on `req` (also aliased as `logOut()`) that can be called from any route handler which needs to terminate a login session. Invoking `logout()` will remove the `req.user` property and clear the login session (if any).