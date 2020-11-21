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

``` csharp
// ASP.NET Core
services.AddCors(options =>
{
    // configure hosts etc.
});

app.UseCors();
```

``` ts
// NestJS
app.enableCors(/* configuration */);
```

``` python
# FastAPI
app.add_middleware(
    CORSMiddleware,
    allow_origins=#...
)
``` 

## Before / after / filter / decorate / terminate

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

``` python
# FastAPI
@app.middleware("http")
async def example_middleware(request: Request, call_next):
    # before / filter / decorate / terminate
    response = await call_next(request)
    # after
    return response
```


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

``` js
// Express
var exampleMiddleware = function (req, res, next) {
  // before / filter / decorate / terminate
  next()
  // after
}

app.use(exampleMiddleware)
```

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

``` js
// restify
server.use(function(req, res, next) {
    // before / filter / decorate / terminate
    next();
    // after
});
```

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
``` js
// koa
app.use(async (ctx, next) => {
   // before / filter / decorate / terminate 
  await next();
  // after
});
```

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
``` csharp
// ASP.NET Core
[Authorize(Roles = "Admin")]
public class ProductController : ControllerBase { }
```

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


``` python
# FastAPI
app.add_middleware(AuthorizationMiddleware, role="admin")
```

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

``` php
// CakePHP
public function middleware(MiddlewareQueue $middlewareQueue): MiddlewareQueue
{
    $middlewareQueue->add(new AuthorizationMiddleware('admin'));
}
```

## Conventions

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

## Exception handling
- Prevent sending sensitive system information