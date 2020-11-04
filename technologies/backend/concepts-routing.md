# Routing
Routing is the process of mapping http requests to handlers

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

``` csharp
// .NET Core
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
}
```

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
}
```

```php
// Laravel
Route::get('products', function () {
    // return list of products
});

Route::get('products/{id}', function ($id) {
    // return single product
});
```

``` python
# FastAPI
@app.get("/products/{id}")
async def get_product(id: int):
    return #...
```

``` java
// Spring Boot
@RestController
class ProductController {

  @GetMapping("/products/{id}")
  Product getSingle(@PathVariable Long id) {
    // return single product
  }

  @DeleteMapping("/products/{id}")
  void deleteProduct(@PathVariable Long id) {
    // delete product
  }
}
```

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

``` php
// Symfony
return function (RoutingConfigurator $routes) {
    $routes->add('product_details', '/products/{id<\d+>}')
        ->controller([ProductController::class, 'get_details'])
        ->methods(['GET'])
    ;
};
```

``` js
// Express
app.get('/products/:id(\\d+)', function (req, res) {
  // ...
})
```

### Constraints
``` csharp
[HttpGet("{id:guid}")]
public Product GetDetails(Guid id)
{
    // ...
}  
```

``` php
// Laravel

Route::get('products/{id}', function ($id) {
    //
})->where('id', '[0-9]+');

Route::get('products/{code}', function ($code) {
    //
})->where('code', '[A-Za-z0-9]+');
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
// .NET Core

[Route("api/[controller]")]
public class CustomBaseController : ControllerBase
{  
}

public class ProductsController : CustomBaseController
{
    // all endpoints have route /api/products/...
}

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
//.NET Core
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
    FileProvider = new PhysicalFileProvider("/path/to/files"),
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
app.mount("/assets", StaticFiles(directory="assets"), name="assets")
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