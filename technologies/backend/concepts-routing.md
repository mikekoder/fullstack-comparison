# Routing

## Basic routing

Common CRUD endpoints
```
GET     /products/     -> get list of products
GET     /products/{id} -> get single product
POST    /products/     -> create product
PUT     /products/{id} -> update product
DELETE  /products/{id} -> delete product
```

R1 - HTTP method: Can route GET/POST/PUT/DELETE to different handlers  
R2 - Path: Can route different path patterns to different handlers  
R3 - Parameters: Can capture variables from path patterns  
R4 - Wildcard parameter: Parameter in path can be  
R5 - Constraint: Parameter in path can be constrained to match certain rules (e.g. only numbers)

#### AIOHTTP
``` python
# AIOHTTP
app.add_routes([
  web.get(r'products/{id:\d+}', getProductDetails)
  #...
]
```

#### Akka HTTP


#### ASP.NET Core
``` csharp
// ASP.NET Core
[Route("products")]
[ApiController]
public class ProductsController : ControllerBase
{
  [HttpGet]
  public Product[] GetAll()
  {
      // ...
  }   

  [HttpPut("{id}")]
  public Product Update(int id, [FromBody]ProductModel product)
  {
      // ...
  } 

  [HttpGet("files/{*path}")]
  public FileResult GetFile(string path)
  {
    // ...  
  } 

  [HttpGet("{id:guid}")]
  public Product GetDetails(Guid id)
  {
      // ...
  }   
}
```

#### CakePHP
``` php
// CakePHP
$routes->get(
    '/products/{id}',
    ['controller' => 'Products', 'action' => 'details'],
)
->setPatterns(['id' => '\d+'])
->setPass(['id']);

$routes->get(
    '/files/**',
    ['controller' => 'Files', 'action' => 'get']
);
```

#### CodeIgniter
``` php
// CodeIgniter
$routes->get('products/(\d+)', 'Products::details/$1');
```

#### Django / Django REST Framework
``` python
# Django REST Framework
@api_view(['GET'])
def product_list(request):
  return #...

@api_view(['PUT'])
def update_product(request, id):
  # ...

urlpatterns = [
    path('products/', views.product_list),
    path('products/<int:id>', views.update_product),
    # ...
]
```

#### DropWizard
``` java
// DropWizard
@Path("/products")
public class ProductResource {
    @GET
    @Path("/{id}")
    public String getProduct(@PathParam("id") Long id) {
        // ...
    }

    @GET
    @Path("files/{path:.+")
    public String getProduct(@PathParam("path") String path) {
        // ...
    }
}
```

#### Express
``` js
// Express
app.get('/products/:id(\\d+)', function (req, res) {
  // ...
})
```

#### Falcon
``` python
# Falcon
class ProductResource(object):
    def on_get(self, req, resp):
        # ...

product_resource = ProductResource()

api.add_route(
    '/products/{id:int}',
    product_resource
)
```

#### FastAPI
``` python
# FastAPI
@app.get("/products/{id}")
async def get_product(id: int):
    #...

@app.get("/files/{path:path}")
async def get_file(path: str):
    # ...
```

#### Feathers.js

#### Flask
``` python
# Flask
@app.route('/products/<int:id>', methods=['GET'])
def get_product(id):
  # ...

@app.route('/products/<path:file>', methods=['GET'])
def get_file(file):
    # ...
```

#### hapi
``` js
// hapi
server.route({
    method: 'GET',
    path: '/files/{path*}',
    handler: function (request, h) {
        // ...
    }
});
```

#### koa

#### Laravel
```php
// Laravel
Route::get('products', function () {
    // return list of products
});

Route::get('products/{id}', function ($id) {
    // return single product
})->where('id', '[0-9]+');

Route::get('files/{path}', function ($path) {
    // ...
})->where('path', '.*');
```

#### Micronaut
``` java
// Micronaut
@Controller("/products") 
public class ProductsController {

    @Get("/{id:\\d+}") 
    public String details(@PathVariable Integer id) { 
        // ...
    }
    @Put("/{id:\\d+}")
    public Single<HttpResponse<Person>> update(@PathVariable Integer id, @Body Single<Product> product) { 
        // ...
    }
    @Get("/files/{path:.+}") 
    public String file(@PathVariable String path) { 
        // ...
    }
}
```

#### NestJS
``` ts
// NestJS
@Controller('products')
export class ProductsController {
  // /products/
  @Get()
  getAll(): ProductModel[] {
    // return list of products
  }

  // products/123/
  @Get(':id')
  getSingle(@Param('id') id: number): ProductModel {
    // return single product
  }

  @Get('files/:path(*)')
  getFile(@Param('path') path: string) {
    // ...
  }
}
```

#### Phalcon
``` php
// Phalcon
$router->addGet(
    '/products/{id:[0-9]+}',
    [
        'controller' => 'products',
        'action'     => 'details'
);
$router->addGet(
    '/files/{path:.*}',
    [
        'controller' => 'files',
        'action'     => 'details'
);
```

#### Play Framework
``` ini
# Play Framework
GET   /products/$id<[0-9]+>   controllers.Products.details(id: Long)
GET   /files/*path            controllers.Files.getFile(path: String)
```

#### Quarkus
``` java
// Quarkus
@Path("/products")
public class ProductResource {

    @GET
    public Response getAll() {
      // ...
    }

    @PUT
    @Path("{id}")
    public Response update(@PathParam("id") Long id, Product product) {
      // ...
    }
}
```

#### restify
``` js
// Restify
server.get('products/:id', function rm(req, res, next) {
  // ...
});

server.get(/files\/(.*)/, function(req, res, next) {
  // ...
});

```

#### Sails.js
``` js
// Sails
module.exports.routes = {
  'GET /products/:id': 'ProductsController.details',
  'PUT /products/:id': { controller: 'products', action: 'update' }
}
```

#### ServiceStack
``` csharp
// ServiceStack
[Route("/products/{Id}", "GET")]
public class GetProduct : IReturn<Product>
{
    public int Id { get; set; }
}

public class ProductService : Service
{
    public object Get(GetProduct request)
    {
        // ...
    }
}

[Route("/files/{Path*}")]
public class GetFile : IReturn<File>
{
    public string Path { get; set; }
}

[Route("/users/{Id}", Matches = "**/{int}")]
public class GetUser
{
    public int Id { get; set; }
}
```

#### Slim
``` php
// Slim
$app->get('/products/{id:[0-9]+}', function (Request $request, Response $response, array $args) {
    // ...
});
$app->get('/files/{path:.+}', function ($request, $response, $args) {
    // ...
});
```

#### Spring (Boot)
``` java
// Spring Boot
@RestController
class ProductController {

  @GetMapping("/products/{id:\\d+}")
  Product getSingle(@PathVariable Long id) {
    // return single product
  }

  @DeleteMapping("/products/{id}")
  void deleteProduct(@PathVariable Long id) {
    // delete product
  }

  @GetMapping("/files/{*path}")
  File getSingle(@PathVariable String path) {
    // return single product
  }

}
```

#### Symfony
``` php
// Symfony
return function (RoutingConfigurator $routes) {
    $routes->add('product_details', '/products/{id<\d+>}')
        ->controller([ProductController::class, 'get_details'])
        ->methods(['GET'])
    ;
    $routes->add('get_file', '/files/{path}')
        ->controller([FileController::class, 'get_file'])
        ->requirements([
            'path' => '.+',
        ])
    ;
};
```

#### Tornado
#### Vert.x
``` java
// Vert.x
Route route = router.routeWithRegex("\\/products\\/(?<id>[0-9]+)").method(HttpMethod.PUT)
  .handler(routingContext -> {
    String productId = routingContext.request().getParam("id");
  // ...
  }
);
Route route = router.routeWithRegex("\\/files\\/(?<path>.+)").method(HttpMethod.PUT)
  .handler(routingContext -> {
    String productId = routingContext.request().getParam("id");
  // ...
  }
);
```

## Advanced

R6 - Prefix: Certain prefix can be applied and changed to many routes at once (e.g. all endpoints start with /api/)  
R7 - Route reversing: Can generate urls using route pattern and parameter values  
R8 - Subdomain: Can route apiv1.example.com and apiv2.example.com to different handlers  
R9 - Versioning: Can call different handlers based on version number defined in request headers  
R10 - Static files: Can serve static files from defined directory  
R11 - Rate limiting: Can define how many calls are allowed in certain time period  
R12 - Redirect: Can redirect request without creating a  handler  
R13 - Fallback route: Handler can be assigned to requests that don’t match any route


### Route prefix
Multiple routes can have a common prefix.  
E.g. all endpoints start with "/api/*"  
```
/products/{id} -> /api/products/{id}
```

``` csharp
// ASP.NET Core
[Route("api/[controller]")]
public class CustomBaseController : ControllerBase
{  
}

public class ProductsController : CustomBaseController
{
    // all endpoints have route /api/products/...
}
```
``` ts
// NestJS
const app = await NestFactory.create(AppModule);
app.setGlobalPrefix('api');
```

```php
// Laravel
Route::prefix('api')->group(function () {
    Route::get('products', function () {
      // ...
    });
});
```
 
``` java
// Spring Boot

@RestController
@RequestMapping(path = "${prefix}/products")
public class ProductsController {
  // ...
}

// application.properties
prefix=/api
```

``` js
// Express
app.use('/api', router);
```

``` php
// CakePHP
$routes->prefix('api', function (RouteBuilder $routes) {
    $routes->get(/* ... */)
});
```

``` csharp
// ServiceStack
public class AppHost : AppHostBase
{
    //...
    public override RouteAttribute[] GetRouteAttributes(Type requestType)
    {
        var routes = base.GetRouteAttributes(requestType);
        routes.Each(x => x.Path = "/api" + x.Path);
        return routes;
    }
}
```

``` php
// Symfony
$routes->import('../api/Controller/', 'annotation')
  ->prefix('/api')
;
```


### Reverse routing
Reverse routing is creating an URL based on route and its parameters

Route definition
```
{ name: 'user-details', path: 'users/{id}' }
```

```
url = router.url('user-details', { id: 123} )
```


```csharp
// ASP.NET Core
[HttpGet("{id}", Name = "product-details")]
public IActionResult GetSingle(int id)
{
  // ...
}

// generate url
Url.Link("product-details", new { id = 123 });
```

``` php
// Laravel
Route::get('products/{id}', function () {
    //
})->name('product-details');

// generating url by route
$url = route('product-details', ['id' => 123]);
```

``` php
// CakePHP
$routes->get(
    '/products/{id}',
    ['controller' => 'Products', 'action' => 'details'],
    ['_name' => 'product-details']
)
->setPatterns(['id' => '\d+'])
->setPass(['id']);

Router::pathUrl('Products::details', [123]);
Router::url(['_name' => 'product-details', 'id' => 123]);
``` 

``` csharp
// ServiceStack
[Route("/products/{Id}", "GET")]
public class GetProduct : IReturn<Product>
{
    public int Id { get; set; }
}

var relativeUrl = new GetProduct { Id = 123 }.ToGetUrl();
var absoluteUrl = new GetProduct { Id = 123 }.ToAbsoluteUri();
```

### Subdomain
- Multitenancy ```{client}.example.com/products```
- Api versioning ```apiv1.example.com/products``` 


``` ts
// NestJS
@Controller({ host: ':client.example.com' })
export class ProductsController {
  @Get()
  getProducts(@HostParam('client') client: string) {
    // ...
  }
}

```

``` php
// Laravel
Route::domain('{client}.example.com')->group(function () {
    Route::get('products', function ($client) {
        // ...
    });
});
```

``` php
// Symfony
$routes->add('product-list', '/products')
    ->controller([ProductsController::class, 'list'])
    ->host('{client}.example.com');
```

``` php
// CodeIgniter
$routes->add('products', 'Products::list_client1', ['subdomain' => 'client1']);
$routes->add('products', 'Products::list_client2', ['subdomain' => 'client2']);
```

### Versioning
When updating clients in sync with the api is not possible and new version contains breaking changes, api versioning must be handled somehow.

Using subdomain or route prefix
```
apiv1.example.com/products/
api.example.com/v1/products/
```
Client must know the exact version of each api endpoint.

Headers: 
```
X-Accept-Version: 1
```

Defined versions
```
products
1.0
1.1
2.0

users
1.0
1.1
```

Resolved versions for 2.0
```
products -> 2.0
users    -> 1.1
```


``` ts
// Restify
server.get('/products/:id', restify.plugins.conditionalHandler([
  { version: '1.1.0', handler: getProductByIdV1 },
  { version: '2.0.0', handler: getProductByIdV2 }
]));
```

``` csharp
// ASP.NET Core
// Nuget: Microsoft.AspNetCore.Mvc.Versioning
// startup.cs
services.AddApiVersioning(config =>
{
    config.DefaultApiVersion = new ApiVersion(1, 0);
    config.AssumeDefaultVersionWhenUnspecified = true;
    config.ApiVersionReader = new HeaderApiVersionReader("X-api-version");
});

// endpoints
[Route("[controller]")]
[ApiController]
[ApiVersion("1.0")]
[ApiVersion("1.1")]
public class ProductsController : ControllerBase
{
    [HttpGet]
    public ProductModel[] GetAll()
    {
        //... default version 1.0
    }


    [HttpGet]
    [MapToApiVersion("1.1")]
    public ProductModel[] GetAll_V1_1()
    {
        // ...
    }
}
```






### Static files

``` csharp
// ASP.NET Core
app.UseStaticFiles(new StaticFileOptions
{
    FileProvider = new PhysicalFileProvider("path/to/files"),
    RequestPath = "/assets"
});
```

``` ts
// NestJS
@Module({
  imports: [
    ServeStaticModule.forRoot({
      rootPath: join(__dirname, '..', 'assets'),
    }),
  ],
  // ...
})
export class AppModule {}
```

``` python
# FastAPI
app.mount("/assets", StaticFiles(directory="path/to/files"), name="assets")
```

``` js
// Express
app.use('/assets', express.static('path/to/files'))
```

### Rate limiting

``` php
// Laravel
RateLimiter::for('global', function (Request $request) {
    return Limit::perMinute(1000);
});
Route::middleware(['throttle:global'])->group(function () {
    Route::get('/products', function () {
        //
    });
});
```

### Redirect
``` php
// Laravel
Route::redirect('/old', '/new');
```


### Fallback / catch all route

If a client application is hosted by the same server as the api, server needs to handle client side routing.  
E.g. all api endpoints are behind */api/...* then all client side routes must point to the same *index.html* file.

``` csharp
// ASP.NET Core
app.UseEndpoints(endpoints =>
{
    endpoints.MapFallbackToFile("/index.html");
});
```

``` php
// Laravel
Route::fallback(function () {
    // ...
});
```

``` csharp
// ServiceStack
[FallbackRoute("/{Path}")]
public class Fallback
{
    public string Path { get; set; }
}
```