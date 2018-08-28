## 路由

下载

`npm i koa-router --s`

使用

```js
var Koa = require('koa');
var Router = require('koa-router');
 
var app = new Koa();
var router = new Router();
 
router.get('/', (ctx, next) => {
  // ctx.router available
});
 
app
  .use(router.routes())
  .use(router.allowedMethods());
```

## 中间件

先执行app.use,遇到next再执行router.get

应用级中间件

```js
//Koa中间件

//匹配任何路由  ，如果不写next，这个路由被匹配到了就不会继续向下匹配
/*
 app.use(async (ctx)=>{
    ctx.body='这是一个中间件';
 })
*/

/*匹配路由之前打印日期*/
app.use(async (ctx,next)=>{

    console.log(new Date());

    await next(); /*当前路由匹配完成以后继续向下匹配*/
})

router.get('/',async (ctx)=>{

    ctx.body="首页";

})

app.use(router.routes());   /*启动路由*/
app.use(router.allowedMethods());
app.listen(3002);
```

路由级中间件

```js
//Koa中间件
// 匹配到news路由以后继续向下匹配路由
router.get('/news',async (ctx,next)=>{
    console.log('这是一个新闻1');
    await next();
})
router.get('/news',async (ctx)=>{
    ctx.body='这是一个新闻';
})

app.use(router.routes());   /*启动路由*/
app.use(router.allowedMethods());
app.listen(3002);
```

### 错误处理

```js
//www.域名.com/news
app.use(async (ctx,next)=>{
    console.log('01');
    next();

    console.log('03');
    if(ctx.status==404){   /*如果页面找不到*/
        ctx.status = 404;
        ctx.body="这是一个 404 页面"
    }else{
        console.log('04');
    }
})

router.get('/news',async (ctx)=>{
    console.log('02');
    ctx.body='这是一个新闻';
})

app.use(router.routes());   /*启动路由*/
app.use(router.allowedMethods());
app.listen(3002);
```

### 执行流程

洋葱模型,从上到下再从下到上

```js
//www.域名.com/news
app.use(async (ctx,next)=>{
    console.log('第一个中间件01');
    await next();

    console.log('05匹配路由完成后返回来执行中间件');
})

app.use(async (ctx,next)=>{
    console.log('第二个中间件02');
    await next();

    console.log('04匹配路由完成后返回来执行中间件');
})

router.get('/news',async (ctx)=>{
    console.log('03匹配到了这个路由');
    ctx.body='这是一个新闻';
})
```



## 获取post提交数据

在koa中写原生nodejs获取

```js
//app.js
var Koa=require('koa'),
    router = require('koa-router')(),
    views = require('koa-views'),
    common = require('./module/common.js');
var app=new Koa();
/*应用ejs模板引擎*/
app.use(views('views',{
    extension:'ejs'
}))

router.get('/',async (ctx)=>{
    await  ctx.render('index');
})

//接收post提交的数据
router.post('/doAdd',async (ctx)=>{
    //原生nodejs 在koa中获取表单提交的数据
    var data=await common.getPostData(ctx);
    console.log(data);
    ctx.body=data;
})
//module/common.js
exports.getPostData=function(ctx){
    //获取数据  异步
    return new Promise(function(resolve,reject){
           try{
               let str='';
               ctx.req.on('data',function(chunk){str+=chunk;})
               ctx.req.on('end',function(chunk){resolve(str)})
           }catch(err){reject(err)}
    })
}
```

### 使用koa-bodyparser

```js
/*
    1.npm install --save koa-bodyparser
    2.引入var bodyParser = require('koa-bodyparser');
    3.app.use(bodyParser());
    4.ctx.request.body;  获取表单提交的数据
*/
var bodyParser = require('koa-bodyparser');
var app=new Koa();
/*应用ejs模板引擎*/
app.use(views('views',{
    extension:'ejs'
}))

//配置post bodyparser的中间件
app.use(bodyParser());		//ctx.request.body就是post数据了

router.get('/',async (ctx)=>{

    await  ctx.render('index');
})

//接收post提交的数据
router.post('/doAdd',async (ctx)=>{

    console.log(ctx.request.body);
    ctx.body=ctx.request.body;  //获取表单提交的数据
})

app.use(router.routes());   /*启动路由*/
app.use(router.allowedMethods());
app.listen(3000);
```

## 静态资源中间件

 koa-static 静态资源中间件  静态web服务
1.cnpm install  koa-static --save
2.const static = require('koa-static')
3.配置中间件
 app.use(static('static'))

```js
//配置静态web服务的中间件
//app.use(static('./static'));

app.use(static(__dirname+'/static'));
app.use(static(__dirname+'/public'));   //koa静态资源中间件可以配置多个
```

## art模版引擎

http://aui.github.io/art-template/koa/

```js
/*
 http://aui.github.io/art-template/koa/
 1、
 npm install --save art-template
 npm install --save koa-art-template

 2、const render = require('koa-art-template');

 3、
 render(app, {
     root: path.join(__dirname, 'view'),   视图的位置
     extname: '.art', 后缀名
     debug: process.env.NODE_ENV !== 'production'  是否开启调试模式

 });
4、
 await ctx.render('user');
 */
var Koa=require('koa'),
    router = require('koa-router')(),
    render = require('koa-art-template'),
    path=require('path');

//配置 koa-art-template模板引擎
render(app, {
    root: path.join(__dirname, 'views'),   // 视图的位置
    extname: '.html',  // 后缀名
    debug: process.env.NODE_ENV !== 'production'  //是否开启调试模式

});

router.get('/',async (ctx)=>{
   //ctx.body="首页"
    let list={
        name:'张三',
        h:'<h2>这是一个h2</h2>',
        num:20,
        data:['11111111','2222222222','33333333333']
    }
    await ctx.render('index',{
        list:list
    });
})

router.get('/news',async (ctx)=>{
    let app={
        name:'张三11',
        h:'<h2>这是一个h211</h2>',
        num:20,
        data:['11111111','2222222222','33333333333']
    };
    await ctx.render('news',{
        list:app
    });
})
```

## session

1.`npm install koa-session  --save`

2、`const session = require('koa-session');`

3、app.keys = ['some secret hurr'];

 const CONFIG = {
 key: 'koa:sess',
maxAge: 86400000,
    overwrite: true,
    httpOnly: true,
    signed: true,
    rolling: false,
    renew: false,
};

app.use(session(CONFIG, app));

设置 session
`ctx.session.username = "张三"`

获取 session
` ctx.session.username`
```js
var render = require('koa-art-template'),
    path=require('path'),
    session = require('koa-session');

//配置session的中间件
app.keys = ['some secret hurr'];   /*cookie的签名*/
const config = {
    key: 'koa:sess', /* 默认 */
    maxAge: 10000,  /*  cookie的过期时间        【需要修改】  */
    overwrite: true, /* (boolean) can overwrite or not (default true)    没有效果，默认 */
    httpOnly: true, /*  true表示只有服务器端可以获取cookie */
    signed: true, /* 默认 签名 */
    rolling: true, /* 每次请求时重置 cookie 过期时间（默认：false） 【需要修改】 */
    renew: false, /*  快过期时请求重置 cookie 过期时间  【需要修改】*/
};
app.use(session(config, app));

router.get('/news',async (ctx)=>{
    //获取session
    console.log(ctx.session.userinfo);
    ctx.body="登录成功";
})

router.get('/login',async (ctx)=>{
    //设置session
    ctx.session.userinfo='张三';
    ctx.body="登录成功";
})

app.use(router.routes());   /*启动路由*/
app.use(router.allowedMethods());
app.listen(3000);
```

## 数据库封装

```js
var MongoClient = require('mongodb').MongoClient;
var Config=require('./config.js');
class Db{
    static getInstance(){   /*1、单例  多次实例化实例不共享的问题*/
        if(!Db.instance){
            Db.instance=new Db();
        }
        return  Db.instance;
    }
    
    constructor(){
        this.dbClient=''; /*属性 放db对象*/
        this.connect();   /*实例化的时候就连接数据库*/
    }
    
    connect(){  /*连接数据库*/
      let _that=this;
      return new Promise((resolve,reject)=>{
          if(!_that.dbClient){         /*1、解决数据库多次连接的问题*/
              MongoClient.connect(Config.dbUrl,(err,client)=>{
                  if(err){
                      reject(err)
                  }else{
                      _that.dbClient=client.db(Config.dbName);
                      resolve(_that.dbClient)
                  }
              })
          }else{
              resolve(_that.dbClient);
          }
      })
    }
    find(collectionName,json){
       return new Promise((resolve,reject)=>{
           this.connect().then((db)=>{
               var result=db.collection(collectionName).find(json);
               result.toArray(function(err,docs){
                   if(err){
                       reject(err);
                       return;
                   }
                   resolve(docs);
               })
           })
       })
    }

module.exports=Db.getInstance();
```





# CTX

`Context` 具体方法和访问器.

### ctx.req

Node 的 `request` 对象.

### ctx.res

Node 的 `response` 对象.

绕过 Koa 的 response 处理是 **不被支持的**. 应避免使用以下 node 属性：

- `res.statusCode`
- `res.writeHead()`
- `res.write()`
- `res.end()`

### ctx.request

koa 的 `Request` 对象.

### ctx.response

koa 的 `Response` 对象.

### ctx.state

存放公共数据

推荐的命名空间，用于通过中间件传递信息和你的前端视图。

```
ctx.state.user = await User.find(id);
```

### ctx.app

应用程序实例引用

### ctx.cookies.get(name, [options])

通过 `options` 获取 cookie `name`:

- `signed` 所请求的cookie应该被签名

koa 使用 [cookies](https://github.com/jed/cookies) 模块，其中只需传递参数。

### ctx.cookies.set(name, value, [options])

通过 `options` 设置 cookie `name` 的 `value` :

- `maxAge` 一个数字表示从 Date.now() 得到的毫秒数
- `signed` cookie 签名值
- `expires` cookie 过期的 `Date`
- `path` cookie 路径, 默认是`'/'`
- `domain` cookie 域名
- `secure` 安全 cookie
- `httpOnly` 服务器可访问 cookie, 默认是 **true**
- `overwrite` 一个布尔值，表示是否覆盖以前设置的同名的 cookie (默认是 **false**). 如果是 true, 在同一个请求中设置相同名称的所有 Cookie（不管路径或域）是否在设置此Cookie 时从 Set-Cookie 标头中过滤掉。

koa 使用传递简单参数的 [cookies](https://github.com/jed/cookies) 模块。

### ctx.throw([status], [msg], [properties])

Helper 方法抛出一个 `.status` 属性默认为 `500` 的错误，这将允许 Koa 做出适当地响应。

允许以下组合：

```
ctx.throw(400);
ctx.throw(400, 'name required');
ctx.throw(400, 'name required', { user: user });
```

例如 `ctx.throw(400, 'name required')` 等效于:

```
const err = new Error('name required');
err.status = 400;
err.expose = true;
throw err;
```

请注意，这些是用户级错误，并用 `err.expose` 标记，这意味着消息适用于客户端响应，这通常不是错误消息的内容，因为您不想泄漏故障详细信息。

你可以根据需要将 `properties` 对象传递到错误中，对于装载上传给请求者的机器友好的错误是有用的。这用于修饰其人机友好型错误并向上游的请求者报告非常有用。

```
ctx.throw(401, 'access_denied', { user: user });
```

koa 使用 [http-errors](https://github.com/jshttp/http-errors) 来创建错误。

### ctx.assert(value, [status], [msg], [properties])

当 `!value` 时，Helper 方法抛出类似于 `.throw()` 的错误。这与 node 的 [assert()](http://nodejs.org/api/assert.html) 方法类似.

```
ctx.assert(ctx.state.user, 401, 'User not found. Please login!');
```

koa 使用 [http-assert](https://github.com/jshttp/http-assert) 作为断言。

### ctx.respond

为了绕过 Koa 的内置 response 处理，你可以显式设置 `ctx.respond = false;`。 如果您想要写入原始的 `res` 对象而不是让 Koa 处理你的 response，请使用此参数。

请注意，Koa *不* 支持使用此功能。这可能会破坏 Koa 中间件和 Koa 本身的预期功能。使用这个属性被认为是一个 hack，只是便于那些希望在 Koa 中使用传统的 `fn(req, res)` 功能和中间件的人。

## request

## 查询字符串

### request.querystring

根据 `?` 获取原始查询字符串.

### request.search

使用 `?` 获取原始查询字符串。

### request.query

获取解析的查询字符串, 当没有查询字符串时，返回一个空对象。请注意，此 getter *不* 支持嵌套解析。

例如 "color=blue&size=small":

```
{
  color: 'blue',
  size: 'small'
}
```

### request.path

获取请求路径名。

### request.url

获取请求 URL.

### request.originalUrl

获取请求原始URL。

## 设置字符串

### request.querystring=

设置原始查询字符串。

### request.search=

设置原始查询字符串。

### request.query=

将查询字符串设置为给定对象。 请注意，此 setter *不* 支持嵌套对象。

```
ctx.query = { next: '/login' };
```

### request.path=

设置请求路径名，并在存在时保留查询字符串。

### request.url=

设置请求 URL, 对 url 重写有用。

## 获取请求信息

### request.header

请求标头对象。

### request.header=

设置请求标头对象。

### request.headers

请求标头对象。别名为 `request.header`.

### request.headers=

设置请求标头对象。别名为 `request.header=`.

### request.method

请求方法。

### request.method=

设置请求方法，对于实现诸如 `methodOverride()` 的中间件是有用的。

### request.length

返回以数字返回请求的 Content-Length，或 `undefined`。

### request.origin

获取URL的来源，包括 `protocol` 和 `host`。

```
ctx.request.origin
// => http://example.com
```

### request.href

获取完整的请求URL，包括 `protocol`，`host` 和 `url`。

```
ctx.request.href;
// => http://example.com/foo/bar?q=1
```

### request.host

获取当前主机（hostname:port）。当 `app.proxy` 是 **true** 时支持 `X-Forwarded-Host`，否则使用 `Host`。

### request.hostname

存在时获取主机名。当 `app.proxy` 是 **true** 时支持 `X-Forwarded-Host`，否则使用 `Host`。

如果主机是 IPv6, Koa 解析到 [WHATWG URL API](https://nodejs.org/dist/latest-v8.x/docs/api/url.html#url_the_whatwg_url_api), *注意* 这可能会影响性能。

### request.URL

获取 WHATWG 解析的 URL 对象。

### request.type

获取请求 `Content-Type` 不含参数 "charset"。

```
const ct = ctx.request.type;
// => "image/png"
```

### request.charset

在存在时获取请求字符集，或者 `undefined`：

```
ctx.request.charset;
// => "utf-8"
```

### request.fresh

检查请求缓存是否“新鲜”，也就是内容没有改变。此方法用于 `If-None-Match` / `ETag`, 和 `If-Modified-Since` 和 `Last-Modified` 之间的缓存协商。 在设置一个或多个这些响应头后应该引用它。

```
// 新鲜度检查需要状态20x或304
ctx.status = 200;
ctx.set('ETag', '123');

// 缓存是好的
if (ctx.fresh) {
  ctx.status = 304;
  return;
}

// 缓存是陈旧的
// 获取新数据
ctx.body = await db.find('something');
```

### request.stale

相反与 `request.fresh`.

### request.protocol

返回请求协议，“https” 或 “http”。当 `app.proxy` 是 **true** 时支持 `X-Forwarded-Proto`。

### request.secure

通过 `ctx.protocol == "https"` 来检查请求是否通过 TLS 发出。

### request.ip

请求远程地址。 当 `app.proxy` 是 **true** 时支持 `X-Forwarded-Proto`。

### request.ips

当 `X-Forwarded-For` 存在并且 `app.proxy` 被启用时，这些 ips 的数组被返回，从上游 - >下游排序。 禁用时返回一个空数组。

### request.subdomains

将子域返回为数组。

子域是应用程序主域之前主机的点分隔部分。默认情况下，应用程序的域名假定为主机的最后两个部分。这可以通过设置 `app.subdomainOffset` 来更改。

例如，如果域名为“tobi.ferrets.example.com”：

如果 `app.subdomainOffset` 未设置, `ctx.subdomains` 是 `["ferrets", "tobi"]`. 如果 `app.subdomainOffset` 是 3, `ctx.subdomains` 是 `["tobi"]`.

### request.is(types...)

检查传入请求是否包含 `Content-Type` 头字段， 并且包含任意的 mime `type`。 如果没有请求主体，返回 `null`。 如果没有内容类型，或者匹配失败，则返回 `false`。 反之则返回匹配的 content-type。

```
// 使用 Content-Type: text/html; charset=utf-8
ctx.is('html'); // => 'html'
ctx.is('text/html'); // => 'text/html'
ctx.is('text/*', 'text/html'); // => 'text/html'

// 当 Content-Type 是 application/json 时
ctx.is('json', 'urlencoded'); // => 'json'
ctx.is('application/json'); // => 'application/json'
ctx.is('html', 'application/*'); // => 'application/json'

ctx.is('html'); // => false
```

例如，如果要确保仅将图像发送到给定路由：

```
if (ctx.is('image/*')) {
  // 处理
} else {
  ctx.throw(415, 'images only!');
}
```

### 内容协商

Koa的 `request` 对象包括由 [accepts](http://github.com/expressjs/accepts) 和 [negotiator](https://github.com/federomero/negotiator) 提供的有用的内容协商实体。

这些实用程序是：

- `request.accepts(types)`
- `request.acceptsEncodings(types)`
- `request.acceptsCharsets(charsets)`
- `request.acceptsLanguages(langs)`

如果没有提供类型，则返回 **所有** 可接受的类型。

如果提供多种类型，将返回最佳匹配。 如果没有找到匹配项，则返回一个`false`，你应该向客户端发送一个`406 "Not Acceptable"` 响应。

如果接收到任何类型的接收头，则会返回第一个类型。 因此，你提供的类型的顺序很重要。

### request.accepts(types)

检查给定的 `type(s)` 是否可以接受，如果 `true`，返回最佳匹配，否则为 `false`。 `type` 值可能是一个或多个 mime 类型的字符串，如 `application/json`，扩展名称如 `json`，或数组 `["json", "html", "text/plain"]`。

```
// Accept: text/html
ctx.accepts('html');
// => "html"

// Accept: text/*, application/json
ctx.accepts('html');
// => "html"
ctx.accepts('text/html');
// => "text/html"
ctx.accepts('json', 'text');
// => "json"
ctx.accepts('application/json');
// => "application/json"

// Accept: text/*, application/json
ctx.accepts('image/png');
ctx.accepts('png');
// => false

// Accept: text/*;q=.5, application/json
ctx.accepts(['html', 'json']);
ctx.accepts('html', 'json');
// => "json"

// No Accept header
ctx.accepts('html', 'json');
// => "html"
ctx.accepts('json', 'html');
// => "json"
```

你可以根据需要多次调用 `ctx.accepts()`，或使用 switch：

```
switch (ctx.accepts('json', 'html', 'text')) {
  case 'json': break;
  case 'html': break;
  case 'text': break;
  default: ctx.throw(406, 'json, html, or text only');
}
```

### request.acceptsEncodings(encodings)

检查 `encodings` 是否可以接受，返回最佳匹配为 `true`，否则为 `false`。 请注意，您应该将`identity` 作为编码之一！

```
// Accept-Encoding: gzip
ctx.acceptsEncodings('gzip', 'deflate', 'identity');
// => "gzip"

ctx.acceptsEncodings(['gzip', 'deflate', 'identity']);
// => "gzip"
```

当没有给出参数时，所有接受的编码将作为数组返回：

```
// Accept-Encoding: gzip, deflate
ctx.acceptsEncodings();
// => ["gzip", "deflate", "identity"]
```

请注意，如果客户端显式地发送 `identity;q=0`，那么 `identity` 编码（这意味着没有编码）可能是不可接受的。 虽然这是一个边缘的情况，你仍然应该处理这种方法返回 `false` 的情况。

### request.acceptsCharsets(charsets)

检查 `charsets` 是否可以接受，在 `true` 时返回最佳匹配，否则为 `false`。

```
// Accept-Charset: utf-8, iso-8859-1;q=0.2, utf-7;q=0.5
ctx.acceptsCharsets('utf-8', 'utf-7');
// => "utf-8"

ctx.acceptsCharsets(['utf-7', 'utf-8']);
// => "utf-8"
```

当没有参数被赋予所有被接受的字符集将作为数组返回：

```
// Accept-Charset: utf-8, iso-8859-1;q=0.2, utf-7;q=0.5
ctx.acceptsCharsets();
// => ["utf-8", "utf-7", "iso-8859-1"]
```

### request.acceptsLanguages(langs)

检查 `langs` 是否可以接受，如果为 `true`，返回最佳匹配，否则为 `false`。

```
// Accept-Language: en;q=0.8, es, pt
ctx.acceptsLanguages('es', 'en');
// => "es"

ctx.acceptsLanguages(['en', 'es']);
// => "es"
```

当没有参数被赋予所有接受的语言将作为数组返回：

```
// Accept-Language: en;q=0.8, es, pt
ctx.acceptsLanguages();
// => ["es", "pt", "en"]
```

### request.idempotent

检查请求是否是幂等的。

### request.socket

返回请求套接字。

### request.get(field)

返回请求标头。

## response

### response.header

响应标头对象。

### response.headers

响应标头对象。别名是 `response.header`。

### response.socket

请求套接字。

### response.status

获取响应状态。默认情况下，`response.status` 设置为 `404` 而不是像 node 的 `res.statusCode` 那样默认为 `200`。

### response.status=

通过数字代码设置响应状态：

- 100 "continue"
- 101 "switching protocols"
- 102 "processing"
- 200 "ok"
- 201 "created"
- 202 "accepted"
- 203 "non-authoritative information"
- 204 "no content"
- 205 "reset content"
- 206 "partial content"
- 207 "multi-status"
- 208 "already reported"
- 226 "im used"
- 300 "multiple choices"
- 301 "moved permanently"
- 302 "found"
- 303 "see other"
- 304 "not modified"
- 305 "use proxy"
- 307 "temporary redirect"
- 308 "permanent redirect"
- 400 "bad request"
- 401 "unauthorized"
- 402 "payment required"
- 403 "forbidden"
- 404 "not found"
- 405 "method not allowed"
- 406 "not acceptable"
- 407 "proxy authentication required"
- 408 "request timeout"
- 409 "conflict"
- 410 "gone"
- 411 "length required"
- 412 "precondition failed"
- 413 "payload too large"
- 414 "uri too long"
- 415 "unsupported media type"
- 416 "range not satisfiable"
- 417 "expectation failed"
- 418 "I'm a teapot"
- 422 "unprocessable entity"
- 423 "locked"
- 424 "failed dependency"
- 426 "upgrade required"
- 428 "precondition required"
- 429 "too many requests"
- 431 "request header fields too large"
- 500 "internal server error"
- 501 "not implemented"
- 502 "bad gateway"
- 503 "service unavailable"
- 504 "gateway timeout"
- 505 "http version not supported"
- 506 "variant also negotiates"
- 507 "insufficient storage"
- 508 "loop detected"
- 510 "not extended"
- 511 "network authentication required"

**注意**: 不用太在意记住这些字符串, 如果你写错了,可以查阅这个列表随时更正.

### response.message

获取响应的状态消息. 默认情况下, `response.message` 与 `response.status` 关联.

### response.message=

将响应的状态消息设置为给定值。

### response.length=

将响应的 Content-Length 设置为给定值。

### response.length

以数字返回响应的 Content-Length，或者从`ctx.body`推导出来，或者`undefined`。

### response.body

获取响应主体。

### response.body=

将响应体设置为以下之一：

- `string` 写入
- `Buffer` 写入
- `Stream` 管道
- `Object` || `Array` JSON-字符串化
- `null` 无内容响应

如果 `response.status` 未被设置, Koa 将会自动设置状态为 `200` 或 `204`。

#### String

Content-Type 默认为 `text/html` 或 `text/plain`, 同时默认字符集是 utf-8。Content-Length 字段也是如此。

#### Buffer

Content-Type 默认为 `application/octet-stream`, 并且 Content-Length 字段也是如此。

#### Stream

Content-Type 默认为 `application/octet-stream`。

每当流被设置为响应主体时，`.onerror` 作为侦听器自动添加到 `error` 事件中以捕获任何错误。此外，每当请求关闭（甚至过早）时，流都将被销毁。如果你不想要这两个功能，请勿直接将流设为主体。例如，当将主体设置为代理中的 HTTP 流时，你可能不想要这样做，因为它会破坏底层连接。

参阅: <https://github.com/koajs/koa/pull/612> 获取更多信息。

以下是流错误处理的示例，而不会自动破坏流：

```
const PassThrough = require('stream').PassThrough;

app.use(async ctx => {
  ctx.body = someHTTPStream.on('error', ctx.onerror).pipe(PassThrough());
});
```

#### Object

Content-Type 默认为 `application/json`. 这包括普通的对象 `{ foo: 'bar' }` 和数组 `['foo', 'bar']`。

### response.get(field)

不区分大小写获取响应标头字段值 `field`。

```
const etag = ctx.response.get('ETag');
```

### response.set(field, value)

设置响应标头 `field` 到 `value`:

```
ctx.set('Cache-Control', 'no-cache');
```

### response.append(field, value)

用值 `val` 附加额外的标头 `field`。

```
ctx.append('Link', '<http://127.0.0.1/>');
```

### response.set(fields)

用一个对象设置多个响应标头`fields`:

```
ctx.set({
  'Etag': '1234',
  'Last-Modified': date
});
```

### response.remove(field)

删除标头 `field`。

### response.type

获取响应 `Content-Type` 不含参数 "charset"。

```
const ct = ctx.type;
// => "image/png"
```

### response.type=

设置响应 `Content-Type` 通过 mime 字符串或文件扩展名。

```
ctx.type = 'text/plain; charset=utf-8';
ctx.type = 'image/png';
ctx.type = '.png';
ctx.type = 'png';
```

注意: 在适当的情况下为你选择 `charset`, 比如 `response.type = 'html'` 将默认是 "utf-8". 如果你想覆盖 `charset`, 使用 `ctx.set('Content-Type', 'text/html')` 将响应头字段设置为直接值。

### response.is(types...)

非常类似 `ctx.request.is()`. 检查响应类型是否是所提供的类型之一。这对于创建操纵响应的中间件特别有用。

例如, 这是一个中间件，可以削减除流之外的所有HTML响应。

```
const minify = require('html-minifier');

app.use(async (ctx, next) => {
  await next();

  if (!ctx.response.is('html')) return;

  let body = ctx.body;
  if (!body || body.pipe) return;

  if (Buffer.isBuffer(body)) body = body.toString();
  ctx.body = minify(body);
});
```

### response.redirect(url, [alt])

执行 [302] 重定向到 `url`.

字符串 “back” 是特别提供Referrer支持的，当Referrer不存在时，使用 `alt` 或“/”。

```
ctx.redirect('back');
ctx.redirect('back', '/index.html');
ctx.redirect('/login');
ctx.redirect('http://google.com');
```

要更改 “302” 的默认状态，只需在该调用之前或之后分配状态。要变更主体请在此调用之后:

```
ctx.status = 301;
ctx.redirect('/cart');
ctx.body = 'Redirecting to shopping cart';
```

### response.attachment([filename])

将 `Content-Disposition` 设置为 “附件” 以指示客户端提示下载。(可选)指定下载的 `filename`。

### response.headerSent

检查是否已经发送了一个响应头。 用于查看客户端是否可能会收到错误通知。

### response.lastModified

将 `Last-Modified` 标头返回为 `Date`, 如果存在。

### response.lastModified=

将 `Last-Modified` 标头设置为适当的 UTC 字符串。您可以将其设置为 `Date` 或日期字符串。

```
ctx.response.lastModified = new Date();
```

### response.etag=

设置包含 `"` 包裹的 ETag 响应， 请注意，没有相应的 `response.etag` getter。

```
ctx.response.etag = crypto.createHash('md5').update(ctx.body).digest('hex');
```

### response.vary(field)

在 `field` 上变化。

### response.flushHeaders()

刷新任何设置的标头，并开始主体。