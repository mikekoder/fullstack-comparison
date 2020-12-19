# Middleware
M1 - CORS: Can allow/restrict requests from another domains  
M2 - Before hook: Can run code before handler  
M3 - After hook: Can run code after handler  
M4 - Terminating handler: Can prevent handler from running  
M5 - Parameters: Middleware functionality can be changed using parameters  
M6 - Filters: Middleware can   
M7 - Decorators: Middleware can precalculate values   
M8 - Conventions: Middleware can be applied to many routes/handlers by convention  
M9 - Order: Middleware execution order can be defined  
M10 - Content negotiation: Can parse request payload and serialize response bodies based on Content-Type and Accept headers  
M11 - Error handling: Can prevent sending sensitive exception messages on error

```
app.middleware(handler1)
app.middleware(handler2)
```
```
handler1 start
  handler2 start
  handler2 end
handler1 end
```

```
app.middleware((request, response, next) => {
  log.info('before handler')
  next(); // call handler
  log.info('after handler');
})
```
## Cors

#### ASP.NET Core
``` csharp
// ASP.NET Core
services.AddCors(options =>
{
    // configure hosts etc.
});

app.UseCors();
```

#### FastAPI
``` python
# FastAPI
app.add_middleware(
    CORSMiddleware,
    allow_origins=#...
)
``` 

#### NestJS
``` ts
// NestJS
app.enableCors(/* configuration */);
```

#### ServiceStack
``` csharp
// ServiceStack
Plugins.Add(new CorsFeature(
  allowOriginWhitelist: new[] { "http://localhost" /* , ... */ },
  /* other configuration */
));
```

## Before / after / filter / decorate / terminate

#### AIOHTTP
``` python
# AIOHTTP
@middleware
async def example_middleware(request, handler):
  # before / filter / decorate / terminate
  resp = await handler(request)
  # after
  return resp
```

#### Akka HTTP
``` java
// Akka HTTP
public Route exampleDirective(String role, Supplier<Route> inner) {
  // before / filter / decorate / terminate
  Route result = inner.get();
  // after
  return result;
}

Route route = get(() ->
  path("api", () ->
    exampleDirective('admin', () => /* handler*/)
  )
);
```

#### ASP.NET Core
``` csharp
// ASP.NET Core
app.Use((context, next) => 
{
    // before / filter / decorate / terminate
    next.Invoke();
    // after
});
```
``` csharp
// ASP.NET Core
public class ExampleFilter : ActionFilterAttribute
{
    public override void OnActionExecuting(ActionExecutingContext context)
    {
        base.OnActionExecuting(context);
        // before / filter / decorate / terminate
    }
    public override void OnResultExecuting(ResultExecutingContext context)
    {
        base.OnResultExecuting(context);
        // after
    }
}

[ExampleFilter]
public class ProductsController : ControllerBase
{
}
```

``` csharp
// ASP.NET Core
public abstract class ExampleControllerBase : Controller
{
    public override void OnActionExecuting(ActionExecutingContext context)
    {
        base.OnActionExecuting(context);
        // before / filter / decorate / terminate
    }
    public override void OnActionExecuted(ActionExecutedContext context)
    {
        base.OnActionExecuted(context);
        // after
    }
}
```

#### CakePHP
``` php
// CakePHP
class ExampleMiddleware implements MiddlewareInterface
{
    public function process(
        ServerRequestInterface $request,
        RequestHandlerInterface $handler
    ): ResponseInterface
    {
        // before / filter / decorate / terminate
        $response = $handler->handle($request);
        // after

        return $response;
    }
}
```

#### CodeIgniter
#### Django / Django REST framework
``` python
# Django
class ExampleMiddleware:
    def __init__(self, get_response):
        self.get_response = get_response

    def __call__(self, request):
        # before / filter / decorate / terminate
        response = self.get_response(request)
        # after
        return response
```

#### Dropwizard
``` java
// Dropwizard (jersey filters)
@Provider
public class ExampleRequestFilter implements ContainerRequestFilter {

    @Override
    public void filter(ContainerRequestContext ctx) {
        // before / filter / decorate / terminate
    }
}
@Provider
public class ExampleResponseFilter implements ContainerResponseFilter {
 
    @Override
    public void filter(ContainerRequestContext reqCtx, ContainerResponseContext resCtx) {
        // after
    }
}
```

#### Express
``` js
// Express
var exampleMiddleware = function (req, res, next) {
  // before / filter / decorate / terminate
  next()
  // after
}

app.use(exampleMiddleware)
```

#### Falcon
``` python
# Falcon
class ExampleMiddleware(object):
  def process_request(self, req, resp):
    # before / filter / decorate / terminate 

  def process_response(self, req, resp, resource, req_succeeded):
    # after
```

``` python
# Falcon
def validate_data(req, resp, resource, params):
  # ...
def modify_response(req, resp, resource, params):
  # ...

@falcon.before(validate_data)
@falcon.after(modify_response)
def on_post(self, req, resp):
  # ...
```

#### FastAPI
``` python
# FastAPI
@app.middleware("http")
async def example_middleware(request: Request, call_next):
    # before / filter / decorate / terminate
    response = await call_next(request)
    # after
    return response
```

#### Feathers
``` js
// Feathers
const before_handler = async context => {
  // before / filter / decorate / terminate
  return context;
};
const after_handler = async context => {
  // after
  return context;
};

app.service('products').hooks({
  before: {
    create: [ before_handler ],
    update: [ before_handler ]
  },
  before: {
    create: [ after_handler ],
    update: [ after_handler ]
  }
});
```

#### Flask
``` python
# Flask
def process_request(sender, **extra):
  # before / filter / decorate / terminate

def process_response(sender, response, **extra):
  # after

request_started.connect(process_request, app)
request_finished.connect(process_response, app)
```

``` python
# Flask
def example_decorator(f):
  @wraps(f)
  def decorated_function(*args, **kwargs):
    # ...
    return f(*args, **kwargs)
  return decorated_function

@app.route('/path')
@example_decorator
def get_page():
  # ...
```

``` python
# Flask
class ExampleMiddleware(object):

  def __init__(self, app):
    self.app = app

  def __call__(self, environ, start_response):
    # before / filter / decorate / terminate 
    response = self.app(environ, start_response)
    # after
    return response

app.wsgi_app = ExampleMiddleware(app.wsgi_app)
```

#### hapi
``` js
// hapi
const examplePlugin = {
  name: 'example',
  version: '1.0.0',
  register: async function (server, options) {
    server.ext('onRequest', function (request, h) {
      // before / filter / decorate / terminate 
      return h.continue;
    });
    server.ext('onPreResponse', function (request, h) {
      // after
      return h.continue;
    });
  }
};
```

#### koa
``` js
// koa
app.use(async (ctx, next) => {
   // before / filter / decorate / terminate 
  await next();
  // after
});
```

#### Laravel
```php
// Laravel
class ExampleMiddleware
{
    public function handle($request, Closure $next)
    {
        // before / filter / decorate / terminate
        $next($request);
        // after
    }
}
```

#### Micronaut
``` java
// Micronaut
@Filter("/api/**") 
public class ExampleFilter implements HttpServerFilter { 
    
    ​@Override
   ​public Publisher<MutableHttpResponse<?>> doFilter(HttpRequest<?> request, ServerFilterChain chain) {
      // before / filter / decorate / terminate 
      Publisher<MutableHttpResponse<?>> responsePublisher = chain.proceed(request);
      return Publishers.map(
          chain.proceed(request),
          response ->  /* after */
      );
}
```

#### NestJS
``` ts
// NestJS
@Injectable()
export class ExampleMiddleware implements NestMiddleware {
  use(req: Request, res: Response, next: Function) {
    // before / filter / decorate
    // skip call / throw exception to terminate
    next();
    // after
  }
}
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer
      .apply(ExampleMiddleware)
      .forRoutes('products');
  }
}
```

#### Phalcon
``` php
// Phalcon
$app->before(
  function () use ($app) {
    // before / filter / decorate / terminate 
    return true;
  }
);
$app->after(
  function () use ($app) {
    // after
  }
);
```

#### Play Framework
``` java
// Play framework
public class ExampleFilter extends Filter {

  @Override
  public CompletionStage<Result> apply(
    Function<Http.RequestHeader, CompletionStage<Result>> nextFilter,
    Http.RequestHeader requestHeader) {
    // before / filter / decorate / terminate 
    return nextFilter
        .apply(requestHeader)
        .thenApply(
            result -> {
              // after
              return result;
            });
  }
}
```

#### Quarkus
``` java
// Quarkus (jersey filters)
@Provider
public class ExampleRequestFilter implements ContainerRequestFilter {

    @Override
    public void filter(ContainerRequestContext ctx) {
        // before / filter / decorate / terminate
    }
}
@Provider
public class ExampleResponseFilter implements ContainerResponseFilter {
 
    @Override
    public void filter(ContainerRequestContext reqCtx, ContainerResponseContext resCtx) {
        // after
    }
}
```

#### restify
``` js
// restify
server.use(function(req, res, next) {
    // before / filter / decorate / terminate
    next();
    // after
});
```

#### Sails.js
``` js
// Sails
module.exports.http = {

  middleware: {
    order: [
      // ...
      'example'
      // ...
    ],

    example: (function (){
      return function (req,res,next) {
        // before / filter / decorate / terminate 
        next();
        // after
      };
    })()
  }
}
```

#### ServiceStack
``` csharp
// ServiceStack
public class ExampleRequestFilterAttribute : RequestFilterAsyncAttribute 
{
    public override async Task ExecuteAsync(IRequest req, IResponse res, object requestDto) 
    {
        // before / filter / decorate / terminate 
    }
}

public class ExampleResponseFilterAttribute : ResponseFilterAsyncAttribute
{
    public override async Task ExecuteAsync(IRequest req, IResponse res, object responseDto) 
    {
        // after
    }
}
```

#### Slim
``` php
// Slim
$exampleMiddleware = function (Request $request, RequestHandler $handler) {
    // before / filter / decorate / terminate
    $response = $handler->handle($request);
    // after
    return $response;
};

$app->add($exampleMiddleware);
```

#### Spring (Boot)
``` java
// Spring Boot
@Component
public class ExampleInterceptor implements HandlerInterceptor {
  @Override
  public boolean preHandle(
    HttpServletRequest request, 
    HttpServletResponse response, 
    Object handler) throws Exception {
    // before / filter / decorate / terminate
    return true;
  }
  @Override
  public void postHandle(
    HttpServletRequest request,
    HttpServletResponse response, 
    Object handler, 
    ModelAndView modelAndView) throws Exception {
      // after
  }
}
```

#### Symfony
#### Tornado
#### Vert.x
``` java
// Vert.x
Route route = router.route("/path/");
route.handler(routingContext -> {
  // before / filter / decorate / terminate
  routingContext.next();
  // after
});
route.handler(routingContext -> {
  // main handler
});
```

## Parameters

#### AIOHTTP
``` python
# AIOHTTP
def example_middleware_factory(role):
  @middleware
  async def example_middleware(request, handler):
    # check role
    return await handler(request)
return sample_middleware

app = web.Application(middlewares=[example_middleware_factory('admin')])
```

#### ASP.NET Core
``` csharp
// ASP.NET Core
[Authorize(Roles = "Admin")]
public class ProductController : ControllerBase { }
```

#### CakePHP
``` php
// CakePHP
public function middleware(MiddlewareQueue $middlewareQueue): MiddlewareQueue
{
    $middlewareQueue->add(new AuthorizationMiddleware('admin'));
}
```

#### Express
``` js
// Express
var authorizationMiddleware = function(role){
  return function (req, res, next) {
    if(req.user.role === role){
      // allowed
      next()
    }
    else {
      // not allowed
    }
  }
}

app.use(authorizationMiddleware('admin'))
```

#### Falcon
``` python
# Falcon
def example_hook(req, resp, resource, params, role):
  # ...

@falcon.before(example_hook, ['admin'])
def on_post(self, req, resp):
  # 
```

#### FastAPI
``` python
# FastAPI
app.add_middleware(AuthorizationMiddleware, role="admin")
```

#### Laravel
``` php
// Laravel
class AuthorizeMiddleware
{
    public function handle($request, Closure $next, $role)
    {
        // ...
    }
}

Route::put('...', function ($id) {
    //
})->middleware('AuthorizeMiddleware:admin');
```




#### Slim
``` php
// Slim
$app->add(new AuthorizeMiddleware('admin'));
```

## Conventions
Applying middleware to some subset of routes using convention
- globally
- prefix
- per action


#### ASP.NET Core
``` csharp
// ASP.NET Core
[ExampleFilter] // apply to all sub classes
public abstract class ExampleControllerBase : ControllerBase {}

[ExampleFilter] // apply to all actions
public class ExampleController : ControllerBase 
{ 
  [ExampleFilter] // apply to single action
  [HttpGet("")]
  public IActionResult SomeAction() {}
}
```

``` csharp
// ASP.NET Core
// apply globally
services.AddControllers(options =>
{
    options.Filters.Add(typeof(ExampleFilter));
});
```

#### Laravel
``` php
// Laravel
protected $middlewareGroups = [
   'public' => [
        'throttle:60,1',
        \App\Http\Middleware\ExampleMiddleware::class,
    ],
    // ...
];

Route::group(['middleware' => ['public']], function () {
    // route definitions
});
```

#### ServiceStack
``` csharp
// ServiceStack
this.GlobalRequestFilters.Add((req, res, requestDto) => 
{
  // ...
});
this.GlobalResponseFilters.Add((req, res, responseDto) => 
{
  // ...
});

[Route("/example")]
[ExampleRequestFilter]
public class ExampleRequest
{
  // ...
}

[ExampleResponseFilter]
public class ExampleResponse : IHasResponseStatus
{
  // ...
}


[ExampleRequestFilter]
[ExampleResponseFilter]
public class ExampleService : Service
{
    public object Get(ExampleRequest request)
    {
        // ...
    }
}
```

#### Slim
``` php
// Slim

$middleware = function (Request $request, RequestHandler $handler) {
    // ...
};

$app->group('/api', function (RouteCollectorProxy $group) {
    // route config
})->add($middleware);
```

## Order

#### AIOHTTP
``` python
# AIOHTTP
app = web.Application(middlewares=[middleware1, middleware2])
```

#### ASP.NET Core
``` csharp
// ASP.NET Core
[ExampleFilter(Order = 0)]
public class ExampleController : ControllerBase {}
```

#### CakePHP
``` php
// CakePHP
$middleware = new \App\Middleware\ExampleMiddleware;

$middlewareQueue->add($middleware);       // last
$middlewareQueue->prepend($middleware);   // first
$middlewareQueue->insertAt(2, $middleware);
$middlewareQueue->insertBefore(
    'App\Middleware\OtherMiddleware',
    $middleware
);
$middlewareQueue->insertAfter(
    'App\Middleware\OtherMiddleware',
    $middleware
);
```

## Content negotiation
HTTP request headers include *Content-Type* which describes the type of the body and *Accept* which describes the desired type for the response. Usually it's possible to read header values in the handler code and use branching logic to decide which parsers and serializers to use.
```
if (request.headers.contentType == 'application/json'){
  input = parseJson(request.body)
}
else if (request.headers.contentType == 'application/xml'){
  input = parseXml(request.body)
}
...

if (request.headers.accept == 'application/json'){
  response.body = toJson(data)
}
else if (request.headers.contentType == 'application/xml'){
  response.body = toXml(data)
}
```
That of course is a lot of manual and repetive work. Content negotiation is a concept of automating parsing and serialization based on request headers.

```
// product is automatically parsed from request body
// based on Content-Type header
updateProduct(@fromBody()product: ProductModel) {
  updatedProduct = db.updateProduct(product)

  // updatedProduct is automatically serialized to corrent type
  // based on Accept header
  return updatedProduct
}
```

#### ASP.NET Core
JSON is supported out of the box

``` csharp
// ASP.NET Core
// Add XML support
services.AddControllers()
  .AddXmlSerializerFormatters();
```

Custom formats
``` csharp
public class CsvOutputFormatter : TextOutputFormatter
{
    public CsvOutputFormatter()
    {
        SupportedMediaTypes.Add(MediaTypeHeaderValue.Parse("text/csv"));
        SupportedEncodings.Add(Encoding.UTF8);
        SupportedEncodings.Add(Encoding.Unicode);
    }

    public override async Task WriteResponseBodyAsync(OutputFormatterWriteContext context, Encoding selectedEncoding)
    {
        var body = /* context.Object as csv string */

        await httpContext.Response.WriteAsync(body);
    }
}

services.AddControllers(options =>
{
    //options.InputFormatters.Insert(0, new ExampleInputFormatter());
    options.OutputFormatters.Insert(0, new CsvOutputFormatter());
});
```

## Exception handling
- Prevent sending sensitive system information

#### ASP.NET Core
``` csharp
// ASP.NET Core
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
}
```