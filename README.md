# Node-boilerplate

## Things you didn't know

- Setting up more than one environment has several perks.
```
$ NODE_ENV = Production node app.js
```

- `app.locals` is a place to store the data which persits throughtout the lifetime of an app.

- `res.locals` is a place to store the data which lasts only till the request.

- However you cannot access local variables in middleware though you can  access local variables in templates rendered within the application. This is useful for providing helper functions to templates, as well as app-level data.

- Subapp is an additional instance of express created to handle the routing part which is often the most complicated one. 

```js

var admin = express();

admin.get('/', function (req, res) {
  console.log(admin.mountpath); // [ '/adm*n', '/manager' ]
  res.send('Admin Homepage');
})

var secret = express();

secret.get('/', function (req, res) {
  console.log(secret.mountpath); // /secr*t
  res.send('Admin Secret');
});

// mount the 'secret' subapp on '/secr*t', on the 'admin' sub app
admin.use('/secr*t', secret);

// mount the 'admin' subapp on '/adm*n' and '/manager', on the parent app
app.use(['/adm*n', '/manager'], admin); 

```
- Authenticate api calls 
```js
app.all('/api/*', requireAuthentication);
```

- Disable/Enable
```js
app.disable('foobar') === app.set('foobar', false);
app.disabled('foobar');  // true
app.enable('foobar') === app.set('foobar', true);
app.enabled('foobar');  // true
```

- Middleware will be executed for every request to the app
```js
app.use(function (req, res, next) {
  /* do something here */
  next();
})
```

- Series of middlewares
```js
function mw1(req, res, next) { console.log('mw1'); next(); }
function mw2(req, res, next) { console.log('mw2'); next(); }

var r1 = express.Router();
r1.get('/', function (req, res, next) { console.log('r1'); next(); });

var r2 = express.Router();
r2.get('/', function (req, res, next) { console.log('r2'); next(); });

var subApp = express();
subApp.get('/', function (req, res, next) { console.log('subapp'); next(); });

// executes the callbacks in the provided order
app.use('/someRoute', mw1, [mw2, r1, r2], subApp);
```

- An error-handling middleware is same as other middlewares, except with four arguments instead of three.
```js
app.use(function(err, req, res, next) {
  console.error(err.stack);
  res.status(500).send('Something broke!');
});
```

- Express uses the `debug` module internally to log information about route matches, middleware in use, application mode, and the flow of the request-response cycle.
```js
$ DEBUG=express:* node index.js
```

- In HTTP, the `Content-Disposition`: attachment response header is usually used to hint to the client to present the response body as a downloadable file. Typically, when receiving such a response, a Web browser will prompt the user to save its content as a file instead of displaying it as a page in a browser window, with the filename parameter suggesting the default file name (this is useful for dynamically generated content, where deriving the filename from the URL may be meaningless or confusing to the user).
```js
// now this is how we do it in Express

res.attachment();
// Content-Disposition: attachment

res.attachment('path/to/logo.png');
// Content-Disposition: attachment; filename="logo.png"
// Content-Type: image/png

```

- Now, the above `res.attachment` basically just sets the `header`, you have to `res.send` the file as well. But you're in luck, express got a helper for you. 
```js
res.download('/akugvks-ygksyug-reyyrt7tr74.pdf', 'payslip.pdf');
```
