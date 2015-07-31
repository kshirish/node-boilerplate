# Node-boilerplate

## Things you didn't know

- Setting up more than one environment has several perks.
```
$ NODE_ENV = Production node app.js
```

- `app.locals` is a place to store the data which persits throughtout the lifetime of an app.

- `res.locals` is a place to store the data which lasts only till the request.

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


