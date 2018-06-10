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

# Security

## Use TLS

a handy tool to get a free TLS certificate is [Let’s Encrypt](https://letsencrypt.org/about/), a free, automated, and open certificate authority (CA)

## Use Helmet

```bash
npm install --save helmet
```

```js
var helmet = require('helmet')
app.use(helmet())
```

## Cookies

The [express-session](https://www.npmjs.com/package/express-session) middleware stores session data on the server; it only saves the session ID in the cookie itself, not session data. By default, it uses  in-memory storage and is not designed for a production environment. In  production, you’ll need to set up a scalable session-store; see the list of [compatible session stores](https://github.com/expressjs/session#compatible-session-stores).

In contrast, [cookie-session](https://www.npmjs.com/package/cookie-session) middleware implements cookie-backed storage: it serializes the entire  session to the cookie, rather than just a session key. Only use it when  session data is relatively small and easily encoded as primitive values  (rather than objects).

```js
var session = require('express-session')
app.set('trust proxy', 1) // trust first proxy
app.use(session({
  secret: 's3Cur3',
  name: 'sessionId'
}))
```

```js
var session = require('cookie-session')
var express = require('express')
var app = express()

var expiryDate = new Date(Date.now() + 60 * 60 * 1000) // 1 hour
app.use(session({
  name: 'session',
  keys: ['key1', 'key2'],
  cookie: {
    secure: true,
    httpOnly: true,
    domain: 'example.com',
    path: 'foo/bar',
    expires: expiryDate
  }
}))
```

- `secure` - Ensures the browser only sends the cookie over HTTPS.
- `httpOnly` - Ensures the cookie is sent only over HTTP(S), not client JavaScript, helping to protect against cross-site scripting attacks.
- `domain` - indicates the domain of the cookie; use it to  compare against the domain of the server in which the URL is being  requested. If they match, then check the path attribute next.
- `path` - indicates the path of the cookie; use it to  compare against the request path. If this and domain match, then send  the cookie in the request.
- `expires` - use to set expiration date for persistent cookies.

## Ensure dependencies security

[nsp](https://www.npmjs.com/package/nsp) is a command-line tool that checks the [Node Security Project](https://nodesecurity.io/) vulnerability database to determine if your application uses packages with known vulnerabilities. Install it as follows:

```
$ npm i nsp -g
```

Use this command to submit the `npm-shrinkwrap.json` / `package.json` files for validation to [nodesecurity.io](https://nodesecurity.io/):

```
$ nsp check
```

Snyk offers both a [command-line tool](https://www.npmjs.com/package/snyk) and a [Github integration](https://snyk.io/docs/github) that checks your application against [Snyk’s open source vulnerability database](https://snyk.io/vuln/) for any known vulnerabilities in your dependencies. Install the CLI as follows:

```
$ npm install -g snyk
$ cd your-app
```

Use this command to test your application for vulnerabilities:

```
$ snyk test
```

Use this command to open a wizard that walks you through the process  of applying updates or patches to fix the vulnerabilities that were  found:

```
$ snyk wizard
```

# Middle Wares

## express-session & cookie-session

### Cookie

```
服务器向客户端发送 cookie。
通常使用 HTTP 协议规定的 set-cookie 头操作。
规范规定 cookie 的格式为 name = value 格式，且必须包含这部分。
浏览器将 cookie 保存。
每次请求浏览器都会将 cookie 发向服务器。
其他可选的 cookie 参数会影响将 cookie 发送给服务器端的过程，主要有以下几种：

path：表示 cookie 影响到的路径，匹配该路径才发送这个 cookie。
expires 和 maxAge：告诉浏览器这个 cookie 什么时候过期，expires 是 UTC 格式时间，maxAge 是 cookie 多久后过期的相对时间。当不设置这两个选项时，会产生 session cookie，session cookie 是 transient 的，当用户关闭浏览器时，就被清除。一般用来保存 session 的 session_id。
secure：当 secure 值为 true 时，cookie 在 HTTP 中是无效，在 HTTPS 中才有效。
httpOnly：浏览器不允许脚本操作 document.cookie 去更改 cookie。一般情况下都应该设置这个为 true，这样可以避免被 xss 攻击拿到 cookie。
```

```js
var cookieSession = require('cookie-session')
var express = require('express')
 
var app = express()
 
app.use(cookieSession({
  name: 'session',
  keys: [/* secret keys */],
 
  // Cookie Options
  maxAge: 24 * 60 * 60 * 1000 // 24 hours
}))
```

**cookieSession**(options)

通过提供的选项，创建一个新的cookie session中间件。这个中间件会将session属性依附依附到req上，session属性提供一个对象代表加载的session。这个session可以是一个新的session，如果request中没有提供有效的session，或者是一个在request中已经加载了的session。

这个中间件将自动地想response中添加Set-Cookie头，如果req.session的内容被修改了的话。

**Options**

Cookie session在options对象中接受下面这些属性。

name

设置cookie的名称，默认是session。

keys

用于加密和解密cookie值的秘钥列表。cookies总是使用keys[0]去加密，而其他的keys用来解密。

secret

如果keys没有提供的话，secret是一个字符串将作为单个的秘钥。

Cookie Options

其他的options可以通过cookies.get()和cookies.set()来使用，去控制安全、域名、路径和其他设置项。

这些选项包括下面的这些：

- maxAge：一个代表从现在到过期的数字，以毫秒为单位。
- expires：一个日期对象代表cookie失效的日期。（通常expires在session的最后一个选项）
- path：一个代表cookie路径的字符串。
- domain：一个代表cookie域名的选项（默认没有）
- sameSite：一个boolean值或者字符串代表cookie是否是同一个网站的cookie（默认false）。这个选项可以被设置为’strict’,’lax’或者true（会被映射为’strict’）。
- secure：一个boolean值暗示这个cokkie是否只能在https上传输（默认为false）
- httpOnly：一个boolean值代表cookie是否只能在HTTP(S)上传输，并且不可以通过javascript传输。
- signed：一个boolean值代表这个cookie是否可以被加密。（默认是true）
- overwrite：一个boolean值代表是否可以设置同样name的cookie的值（默认为true）

**req**.session

代表request中的session。

.isChanged 
如果在请求期间session发生了改变，值为true

.isNew 
如果session是新的，值为true

.isPopulated 
决定这个session是否已经存在或者是空的。

**req**.sessionOptions

代表当前请求的session设置项。

**Destroying** a session

删除一个session，只需要把它置为null。

```js
req.session = null
```

```js
var express = require('express');
// 首先引入 cookie-parser 这个模块
var cookieParser = require('cookie-parser');

var app = express();
app.listen(3000);

// 使用 cookieParser 中间件，cookieParser(secret, options)
// 其中 secret 用来加密 cookie 字符串（下面会提到 signedCookies）
// options 传入上面介绍的 cookie 可选参数
app.use(cookieParser());

app.get('/', function (req, res) {
  // 如果请求中的 cookie 存在 isVisit, 则输出 cookie
  // 否则，设置 cookie 字段 isVisit, 并设置过期时间为1分钟
  if (req.cookies.isVisit) {
    console.log(req.cookies);
    res.send("再次欢迎访问");
  } else {
    res.cookie('isVisit', 1, {maxAge: 60 * 1000});
    res.send("欢迎第一次访问");
  }
});
```

### Session

```
服务端为每一个session维护一份会话信息数据，而客户端和服务端依靠一个全局唯一的标识来访问会话信息数据。用户访问web应用时，服务端程序决定何时创建session，创建session可以概括为三个步骤：

1.   生成全局唯一标识符（sessionid）；
2.   开辟数据存储空间。一般会在内存中创建相应的数据结构，但这种情况下，系统一旦掉电，所有的会话数据就会丢失，如果是电子商务网站，这种事故会造成严重的后果。不过也可以写到文件里甚至存储在数据库中，这样虽然会增加I/O开销，但session可以实现某种程度的持久化，而且更有利于session的共享；
3.   将session的全局唯一标示符发送给客户端。
问题的关键就在服务端如何发送这个session的唯一标识上。联系到HTTP协议，数据无非可以放到请求行、头域或Body里，基于此，一般来说会有两种常用的方式：cookie和URL重写。

1.   Cookie
读者应该想到了，对，服务端只要设置Set-cookie头就可以将session的标识符传送到客户端，而客户端此后的每一次请求都会带上这个标识符，由于cookie可以设置失效时间，所以一般包含session信息的cookie会设置失效时间为0，即浏览器进程有效时间。至于浏览器怎么处理这个0，每个浏览器都有自己的方案，但差别都不会太大（一般体现在新建浏览器窗口的时候）；
2.   URL重写
所谓URL重写，顾名思义就是重写URL。试想，在返回用户请求的页面之前，将页面内所有的URL后面全部以get参数的方式加上session标识符（或者加在path info部分等等），这样用户在收到响应之后，无论点击哪个链接或提交表单，都会在再带上session的标识符，从而就实现了会话的保持。读者可能会觉得这种做法比较麻烦，确实是这样，但是，如果客户端禁用了cookie的话，URL重写将会是首选
```

```js
var app = express()
app.set('trust proxy', 1) // trust first proxy
app.use(session({
  secret: 'keyboard cat',
  resave: false,
  saveUninitialized: true,
  cookie: { secure: true }
}))
```

name: 设置 cookie 中，保存 session 的字段名称，默认为 connect.sid 。 

store: session 的存储方式，默认存放在内存中，也可以使用 redis，mongodb 等。express 生态中都有相应模块的支持。 

secret: 通过设置的 secret 字符串，来计算 hash 值并放在 cookie 中，使产生的 signedCookie 防篡改。 

cookie: 设置存放 session id 的 cookie 的相关选项，默认为 (default: { path: '/', httpOnly: true, secure: false, maxAge: null })

 genid: 产生一个新的 session_id 时，所使用的函数， 默认使用 uid2 这个 npm 包。 rolling: 每个请求都重新设置一个 cookie，默认为 false。

 resave: 即使 session 没有被修改，也保存 session 值，默认为 true

 **在内存中存储 session**

```js
var express = require('express');
// 首先引入 express-session 这个模块
var session = require('express-session');

var app = express();
app.listen(5000);

// 按照上面的解释，设置 session 的可选参数
app.use(session({
  secret: 'recommand 128 bytes random string', // 建议使用 128 个字符的随机字符串
  cookie: { maxAge: 60 * 1000 }
}));

app.get('/', function (req, res) {

  // 检查 session 中的 isVisit 字段
  // 如果存在则增加一次，否则为 session 设置 isVisit 字段，并初始化为 1。
  if(req.session.isVisit) {
    req.session.isVisit++;
    res.send('<p>第 ' + req.session.isVisit + '次来此页面</p>');
  } else {
    req.session.isVisit = 1;
    res.send("欢迎第一次来这里");
    console.log(req.session);
  }
});
```

**在 redis 中存储 session**

```js
var express = require('express');
var session = require('express-session');
var redisStore = require('connect-redis')(session);

var app = express();
app.listen(5000);

app.use(session({
  // 假如你不想使用 redis 而想要使用 memcached 的话，代码改动也不会超过 5 行。
  // 这些 store 都遵循着统一的接口，凡是实现了那些接口的库，都可以作为 session 的 store 使用，比如都需要实现 .get(keyString) 和 .set(keyString, value) 方法。
  // 编写自己的 store 也很简单
  store: new redisStore(),
  secret: 'somesecrettoken'
}));

app.get('/', function (req, res) {
  if(req.session.isVisit) {
    req.session.isVisit++;
    res.send('<p>第 ' + req.session.isVisit + '次来到此页面</p>');
  } else {
    req.session.isVisit = 1;
    res.send('欢迎第一次来这里');
  }
});
```

## [Multer](https://github.com/expressjs/multer/blob/master/doc/README-zh-cn.md)

Multer 是一个 node.js 中间件，用于处理 `multipart/form-data` 类型的表单数据，它主要用于上传文件。**注意**: Multer 不会处理任何非 `multipart/form-data` 类型的表单数据。

```js
var express = require('express')
var multer  = require('multer')
var upload = multer({ dest: 'uploads/' })

var app = express()

// 'avatar'是在html表单中相应的input对应的name后同
app.post('/profile', upload.single('avatar'), function (req, res, next) {
  // req.file 是 `avatar` 文件的信息
  // req.body 将具有文本域数据，如果存在的话
})

app.post('/photos/upload', upload.array('photos', 12), function (req, res, next) {
  // req.files 是 `photos` 文件数组的信息
  // req.body 将具有文本域数据，如果存在的话
})

var cpUpload = upload.fields([{ name: 'avatar', maxCount: 1 }, { name: 'gallery', maxCount: 8 }])
app.post('/cool-profile', cpUpload, function (req, res, next) {
  // req.files 是一个对象 (String -> Array) 键是文件名，值是文件数组
  //
  // 例如：
  //  req.files['avatar'][0] -> File
  //  req.files['gallery'] -> Array
  //
  // req.body 将具有文本域数据，如果存在的话
})
```

#### `DiskStorage`

磁盘存储引擎可以让你控制文件的存储。

```js
var storage = multer.diskStorage({
  destination: function (req, file, cb) {
    cb(null, '/tmp/my-uploads')
  },
  filename: function (req, file, cb) {
    cb(null, file.fieldname + '-' + Date.now())
  }
})

var upload = multer({ storage: storage })
```