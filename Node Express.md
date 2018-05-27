# Hello World

```javascript
var express = require('express');
var app = express();

app.get('/', function (req, res) {
  res.send('Hello World!');
});

app.listen(3000, function () {
  console.log('Example app listening on port 3000!');
});
```

# Express Generator

1. install `$ npm install express-generator -g`
2. see all options `express -h`
3. setup app frame `express <--options..> <app namne>`
4. go to the app folder, run `npm install` to save all the need dependencies
5. `npm start` to run the app at *localhost:3000*

# Routing

1. `app.HTTP_METHOD(PATH, HANDLER)`

   1. http methods can be : get, post, put and delete. app.all() deal with all the http methods

   2. handle function is like `(req, res)=>{}`

   3. handle function can be multiple like `app.get('/',<handler1>,<handler2>)` or `app.get('/',[<handler1>,<handler2>])`

      but <handler1> must have a next parameter and call it at last like`(req,res,next)=>{...; next()}`to pass control to next handler

## Route Paths

1. string eg. `app.get('/about',(req,res)=>{})` `-,.`is regarded as string 
2. string pattern: with mark like `?,+,*,()` (?:0 or 1; +:1 or more; *: anything)eg.
   1. `app.get('/ab?cd',(req,res)=>{})`
   2. `app.get('/ab(cd)+e',(req,res)=>{})`
3. regular expression:
   1. `app.get('/a/',(req,res)=>{})` anything with *a* in it
   2. `app.get('/.*fly$/',(req,res)=>{})`

## Route Parameters

1. use `:<name>` to declare the paras

2. retrieve the inputs in `req.params` eg.  

   `Route path: /users/:userId/books/:bookId`

    `Request URL: http://localhost:3000/users/34/books/8989`

   `req.params: { "userId": "34", "bookId": "8989" }`

3. Since the hyphen (`-`) and the dot (`.`) are interpreted literally, they can be used along with route parameters for useful purposes. eg.

   `Route path: /flights/:from-:to`

   `Request URL: http://localhost:3000/flights/LAX-SFO`

   `req.params: { "from": "LAX", "to": "SFO" }`

4. To have more control over the exact string that can be matched by a  route parameter, you can append a regular expression in parentheses (`()`) eg.

   `Route path: /user/:userId(\d+)`

## Response Methods

response methods will terminate the request-response cycle or the client request will be left handling

| Method                                                       | Description                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [res.download()](http://expressjs.com/en/4x/api.html#res.download) | Prompt a file to be downloaded.                              |
| [res.end()](http://expressjs.com/en/4x/api.html#res.end)     | End the response process.                                    |
| [res.json()](http://expressjs.com/en/4x/api.html#res.json)   | Send a JSON response.                                        |
| [res.jsonp()](http://expressjs.com/en/4x/api.html#res.jsonp) | Send a JSON response with JSONP support.                     |
| [res.redirect()](http://expressjs.com/en/4x/api.html#res.redirect) | Redirect a request.                                          |
| [res.render()](http://expressjs.com/en/4x/api.html#res.render) | Render a view template.                                      |
| [res.send()](http://expressjs.com/en/4x/api.html#res.send)   | Send a response of various types.                            |
| [res.sendFile()](http://expressjs.com/en/4x/api.html#res.sendFile) | Send a file as an octet stream.                              |
| [res.sendStatus()](http://expressjs.com/en/4x/api.html#res.sendStatus) | Set the response status code and send its string representation as the response body. |

## app.route()

create chainable route handlers for the same route path with different http request.

`app.route('/').get(<handler>).post(<handler>).put(<handler>)`

## app.Router

Use the `express.Router` class to create modular, mountable route handlers. A `Router` instance is a complete middleware and routing system; for this reason, it is often referred to as a “mini-app”.

1. Define & Export a Router

   ```javascript
   var express = require('express')
   var router = express.Router()
   ...
   module.exports = router
   ```

2. Import & Use a Router

   ```javascript
   var <name> = require('./<name>')
   ...
   app.use('<path>', <Router>)
   ```

# Static File

```
app.use(express.static('public'))
```

can access all the static files in the folder './public' . No need to include'/public' to cite

```
app.use('/static', express.static(path.join(__dirname, 'public')))
```

make a virtual route"/static". can access the files by '/static/'

#Middleware

1. *Middleware* functions are functions that have access to the [request object](http://expressjs.com/en/4x/api.html#req) (`req`), the [response object](http://expressjs.com/en/4x/api.html#res) (`res`), and the `next` function in the application’s request-response cycle. 

2. If the current middleware function does not end the request-response cycle, it must call `next()` to pass control to the next middleware function. Otherwise, the request will be left hanging.

3. `app.use(<middleware>)` to register the middleware function

4. the order of `app.use()` matters. Loaded first are executed first.

5. `app.use(<path>,<middleware>)` will bind the middleware function to the specific path. In contrast, `app.use(<middleware>)` binds the middleware function to every path.

6. app.METHOD() or router.METHOD() can use `next('route')` function to pass the control to the next route with the same path. 

   ```js
   app.get('/user/:id', function (req, res, next) {
     // if the user ID is 0, skip to the next route
     if (req.params.id === '0') next('route')
     // otherwise pass the control to the next middleware function in this stack
     else next()
   }, function (req, res, next) {
     // render a regular page
     res.render('regular')
   })
   
   // handler for the /user/:id path, which renders a special page
   app.get('/user/:id', function (req, res, next) {
     res.render('special')
   })
   ```

# Template Engine

1. all views should be default stored in the **/views** directory
2. `app.set('view engine', 'ejs')` to set up the template engine
3. `res.render('<name>',{<paras>})` no need to set the path and  include the file type( since it has been set by the view engine)

## EJS

1. `<%` 'Scriptlet' tag, for control-flow, no output
2. `<%=` Outputs the value into the template (HTML escaped)
3. `<%-` Outputs the unescaped value into the template usually using with `include`
4. `<%- include('<file path>',{<paras>}) %>` to include partials like header and footer

#Error Handling

1. Define error-handling middleware functions in the same way as other middleware functions, except error-handling functions have four arguments instead of three: `(err, req, res, next)`
2. define error-handling middleware last, after other `app.use()` and routes calls
3. Error handling functions can be more than one and chained but must use`next(err)` to call next error handling function or use `res.send` or other functions to stop the req-res cycle or the request will be always hanging.

# Debug

```bash
DEBUG=express:* node index.js
```

`*` means all the steps, see only routes replace it with `router` 

# Performance

## 使用 gzip 压缩

```js

var compression = require('compression');
var express = require('express');
var app = express();
app.use(compression());
```

通过 Gzip 压缩，有助于显著降低响应主体的大小，从而提高 Web 应用程序的速度。可使用[压缩](https://www.npmjs.com/package/compression)中间件进行 Express 应用程序中的 gzip 压缩

## 将 NODE_ENV 设置为“production”

```bash
export NODE_ENV=production
```

