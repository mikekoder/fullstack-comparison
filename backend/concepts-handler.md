# Handlers
H1 - Dependency injection: Can inject services to handlers  
H2 - Schema based model binding: Request values can be converted to certain type  
H3 - Automatic model binding: Values can be converted on every request automatically  
H4 - Schema based validation: Request values can be validated by schema  
H5 - Automatic validation: Validation can be executed on every request automatically  
H6 - Response cache: Result from handler can be cached for defined time  
H7 - In-memory cache: Small amounts of data can be stored in memory for subsequent request  
H8 - Cache dependencies: Cached values can be cleared by some convention (e.g. related data has changed)

## Dependency injection

#### ASP.NET Core
``` csharp
// ASP.NET Core
services.AddTransient<IExampleService, ExampleService>();
services.AddSingleton<AnotherService>();

public class ExampleController : Controller 
{
    private readonly IExampleService service;
    public ExampleController(ExampleService service)
    {
        this.service = service;
    }

    public ExampleModel GetData([FromService]AnotherService service)
    {
        // ...
    }
}
```

#### NestJS
``` ts
// NestJS

@Injectable()
export class ProductService {
  findAll(): Product[] {
    // ...
  }
}

export class ProductsController {
  constructor(private productService: ProductService) {}

}

@Module({
  controllers: [ProductsController],
  providers: [ProductService],
})
export class AppModule {}
```

#### ServiceStack
``` csharp
public void Configure(Container container)
{
    container.AddTransient<IExampleService, ExampleService>();
    container.AddSingleton<AnotherService>();
}

public class ExampleService : Service
{
    public ExampleService(IExampleService service)
    {
        // ...
    }
}
```

## Model binding
Model binding is the process of reading values from HTTP request and converting them to given type.

```
GET /orders?startDate=2020-09-01&endDate=2020-09-30
```
```
getOrders(startDate: Date, endDate: Date) {

}
```

#### ASP.NET Core
``` csharp
// ASP.NET Core
[HttpPut("{id}")]
public void UpdateProduct(int id, [FromBody] ProductModel product)
{
  // ...
}
```

#### FastAPI
``` python
# FastAPI
@app.put("/products/{id}")
async def update_product(id: int, product: ProductModel):
    # ...
```

#### NestJS
```ts
// NestJS
@Put(':id')
updateProduct(@Param('id') id: string, @Body() product: ProductModel) {
  // ...
}
```

#### Spring
``` java
// Spring
@PutMapping("/products/{id}")
public void updateProduct(@PathVariable Long id, @RequestBody ProductModel product) {
  // ...
}
```

## Validation

#### ASP.NET Core
``` csharp
// ASP.NET Core
public class ProductModel
{
    [Required]
    public string Name { get; set; }
    // ...
}

public class ValidateModelFilter : ActionFilterAttribute
{
    public override void OnActionExecuting(ActionExecutingContext context)
    {
        base.OnActionExecuting(context);
        if(!ModelState.IsValid)
        {
            context.Result = new BadRequestResult();
        }
    }
}
```

#### NestJS
``` ts
// NestJS

export class ProductModel {
  @IsNotEmpty()
  name: string;
  // ...
}

// add validation pipe globally
app.useGlobalPipes(new ValidationPipe());
```

## Cache

### Cache response

#### ASP.NET Core
``` csharp
// ASP.NET Core
public class NewsController : Controller
{
  [HttpGet]
  [ResponseCache(VaryByQueryKeys = new[] { "category" },  Duration = 300)]
  public NewsModel GetMostReadNews(string category)
  {
    // ...
  }
}
```

#### NestJS
``` ts
// NestJS
@Controller()
@UseInterceptors(CacheInterceptor)
export class AppController {
  @CacheKey('most_read_news')
  @CacheTTL(300)
  @Get()
  getMostReadNews(): NewsModel {
    //...
  }
}
```

### Key-value cache

### Cache dependencies