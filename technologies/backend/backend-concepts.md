# Routing
Routing is the process of mapping http requests to handlers

## Basic routing (method, path & parameters)

Common CRUD endpoints
```
GET     /products/     -> get list of products
GET     /products/{id} -> get single product
POST    /products/     -> create product
PUT     /products/{id} -> update product
DELETE  /products/{id} -> delete product
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

  // products/abc123/
  @Get(':id')
  getSingle(@Param('id') id: string): ProductModel {
    // return single product
  }
}
```

```ts
// AdonisJS

Route.get('products', 'ProductsController.getAll')
Route.get('products/:id', 'ProductsController.getSingle')
```

```php
// Lavarel

Route::get('products', function () {
    // return list of products
});

Route::get('products/{id}', function ($id) {
    // return single product
});
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
## Advanced 
### Constraints
Multiple route patterns could match requested path.
E.g. path */products/ABC123* could match either of the route patterns below

```
/products/{id}  
/products/{code}
```

Therefore it's common to set constraints on route params.  
E.g. allowing only numbers for parameter *id* using regex pattern \d+

```
/products/{id:\d+}
```

``` ts
// AdonisJS

Route.get('/products/:id', 'ProductsController.getSingleById')
  .where('id', /^[0-9]+$/) 

Route.get('/products/:code', 'ProductsController.getSingleByCode')
  .where('code', /^[A-Za-z0-9]+$/) 
```

``` php
// Lavarel

Route::get('products/{id}', function ($id) {
    //
})->where('id', '[0-9]+');

Route::get('products/{code}', function ($code) {
    //
})->where('code', '[A-Za-z0-9]+');
```


Statically typed
``` csharp
// .NET Core

[HttpGet("{id}")]
public Product GetSingle(int id)
{
    // ...
}   

[HttpGet("{id}")]
public Product GetSingle(Guid id)
{
    // ...
}   
```

### Method
Same URL can have different handlers based on HTTP method

```
GET  /products/ -> get list of users  
POST /products/ -> create new user
```

## Advanced

### Route prefix
Multiple routes can have a common prefix.  
E.g. all endpoints start with "/api/*"  
```
/products/{id} -> /api/products/{id}
```


``` ts
// NestJS

const app = await NestFactory.create(AppModule);
app.setGlobalPrefix('api');
```

``` ts
// AdonisJS

Route.group(() => {
  Route.get('products', 'ProductsController.getAll') 
}).prefix('api')
```


```php
// Lavarel

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

``` ts
// AdonisJS

Route.group(() => {
  Route.get('/', ({ subdomains }) => {
    const client = subdomains.client
    // ...
  })
}).domain(':client.example.com')
```

``` php
// Lavarel

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

### Reverse routing
Reverse routing is creating an URL based on route and its parameters

Route definition
```
{ name: 'user-details', path: 'users/{id}' }
```

```
url = router.url('user-details', { id: 123} )
```

```ts
// AdonisJS

Route.get('products/:id', 'ProductsController.getSingle').as('product-details')

Route.makeUrl('product-details', { params: { id: 123 } })
```

``` php
// Lavarel

Route::get('products/{id}', function () {
    //
})->name('product-details');

// generating url by route
$url = route('product-details', ['id' => 123]);
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

# Middleware
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

``` ts
// AdonisJS

class ExampleMiddleware {
  async handle ({ request, response }, next) {

    // Do something before

    await next()

    // Do something after
  }
}
```


```php
// Lavarel

class ExampleMiddleware
{
    public function handle($request, Closure $next)
    {
        // Do something before

        $response = $next($request);

        // Do something after

        return $response;
    }
}
```

``` csharp
// .NET Core

 app.Use((context, next) => 
{
    // Do something before

    next.Invoke();

    // Do something after
});
```

# Authentication

## Login with username & password

## Login with OAuth
Logging in with 3rd party accounts (e.g. Google or Facebook) is a common scenario.

## JWT


# Authorization
- Restrict access based on user roles etc.

Features:
- Basic: handler requires special role
- Advanced: 

# Model binding
Model binding is the process of reading values from HTTP request and converting them to given type.

```
GET /orders?startDate=2020-09-01&endDate=2020-09-30
```
```
getOrders(startDate: Date, endDate: Date) {

}
```


```ts
// NestJS

@Put(':id')
updateProduct(@Param('id') id: string, @Body() product: ProductModel) {
  // ...
}
```

# Validation
- Validate handler parameters

Features:
- Basic: 


``` ts
// NestJS

// add validation pipe globally
app.useGlobalPipes(new ValidationPipe());

// decorate model with validation rules
import { IsNotEmpty } from 'class-validator';
export class ProductModel {
  @IsNotEmpty()
  name: string;
  // ...
}
```

# Content negotiation
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

# Cache
- Store expensive processing results for faster access

# View templates
- Render HTML server side

# Exception handling
- Prevent sending sensitive system information

# Logging
- Log exceptions

# Broadcasting
Broadcasting is a concept of noti

# Background workers
- Process data periodically


