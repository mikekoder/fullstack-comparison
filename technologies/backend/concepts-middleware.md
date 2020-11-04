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


## Parameters
``` csharp
// ASP.NET Core
[Authorize(Roles = "Admin")]
public class ProductController : ControllerBase { }
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

## Conventions

## Order

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