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

#### FastAPI
``` python
async def get_service():
  # ...

@app.get("/")
async def get_data(service: ExampleService = Depends(get_service)):
  # ...
```


#### Laravel
No registration is needed for concrete classes
``` php
Route::get('/', function (Service $service) {
    // ...
});
```

Interfaces must be bound first
``` php
$this->app->bind(ExampleService::class, MyExampleService::class);
```

#### Micronaut
``` java
public class ProductsController {

    @Inject ProductsController(ExampleService service) { 
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

#### Phalcon
``` php
class ExampleComponent
{
    protected $container;

    public function __construct(DiInterface $container) {
        $this->container = $container;
    }

    public function view($id)
    {
        $service = $this
            ->container
            ->get('service')
        ;
        // ...
    }
}

$container = new Di();
$container->set(
    'service',
    function () {
        return new ExampleService();
    }
);
```


#### Play Framework
``` java
public class ExampleController {
  @Inject ExampleService service;

  @Inject
  public ExampleController(ExampleService service) {
    this.service = service;
  }
}

```
#### Quarkus
``` java

@ApplicationScoped
class ExampleService {
  // ...
}

@Path("/example")
public class ExampleResource {
    @Inject
    ExampleService service;

    private ExampleService2 service2;

    ExampleResource(ExampleService2 service2) { 
      this.service2 = service2;
    }
}
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

#### Slim
``` php
$container = new \Slim\Container;
$app = new \Slim\App($container);

$container = $app->getContainer();
$container['service'] = function ($container) {
    return new MyService();
};

$app->get('/example', function ($req, $res, $args) {
    $service = $this->get('service');
    // ...
});
```

#### Spring
``` java
@Configuration
public class Config {
    @Bean
    public ExampleService service() {
        return new ExampleService();
    }
}

class ProductController {
  @Autowired
  public ProductController(ExampleService service) {
      this.service = service;
  }
}
```

#### Symfony
``` php
class ExampleService
{
    // ...
}

$containerBuilder = new ContainerBuilder();
$containerBuilder->register('service', 'ExampleService');

class ExampleController
{
    private $service;

    public function __construct(\ExampleService $service)
    {
        $this->service = $service;
    }

    // ...
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

#### Dropwizard
``` java
@POST
public Response create(@NotNull @Valid Notification notification) {
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

#### Quarkus
``` java
// Quarkus
@Path("/products")
public class ProductResource {
    @PUT
    @Path("{id}")
    public Response update(@PathParam("id") Long id, Product product) {
      // ...
    }
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

#### Django
``` python
class ProductModel(models.Model):
    name = models.CharField(max_length=200, validators=[validate_required])


try:
    product.full_clean()
except ValidationError as e:
    # ...
    pass
```

#### Dropwizard
``` java
public class Product {

    @NotEmpty
    private final String name;

    @JsonCreator
    public Person(@JsonProperty("name") String name) {
        this.name = name;
    }

    @JsonProperty("name")
    public String getName() {
        return name;
    }
}

@PUT
public Person update(@NotNull @Valid Person person) {
    // ...
}
```

#### FastAPI
Automatically validated in the endpoint
``` python
class Product(BaseModel):
    name: str
    description: Optional[str] = None
    price: float

@app.post("/products/")
async def create_product(product: Product):
    # ...
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

#### Quarkus
``` java
public class Product {
    @NotBlank(message="Name may not be blank")
    public String name;
}

@Path("/products")
@POST
public Result createProduct(@Valid Product product) {
    // ...
}
```

#### ServiceStack
``` csharp
public class ProductValidator : AbstractValidator<Product>
{
    public ProductValidator()
    {
        RuleFor(x => x.Name).NotEmpty();
    }
}

public class ProductService : Service
{
    public IValidator<TestClass> Validator { get; set; }
    public object Post(Product request)
    {
        var result = this.Validator.Validate(request);
        if (!result.IsValid)
        {
        // ...
    }
}
```

#### Symfony
``` php
class Product
{
    private $name;

    public static function loadValidatorMetadata(ClassMetadata $metadata)
    {
        $metadata->addPropertyConstraint('name', new NotBlank());
    }
}

public function createProduct(ValidatorInterface $validator)
{
    $product = new Product();
    $errors = $validator->validate($author);
    // ...
}
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

#### CodeIgniter
``` php
$this->output->cache(5);
```

#### Django
``` python
@cache_page(5 * 60)
def most_read_news(request):
    # ...
```

#### Micronaut
``` java
public class NewsController {
    @Get("/most-read") 
    @Cacheable(cacheNames = ["most-read"])
    public List<String> mostRead() {
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

#### Quarkus
``` java
@Path("/example")
public class NewsResource {
    @GET
    @CacheResult(cacheName = "most-read-news")
    public Response getMostRead() {
      // ...
    }
}
```

#### ServiceStack
``` csharp
[CacheResponse(Duration = 300)]
public object Get(GetMostReadNews request)
{
    // ...
}
`` 

### Key-value cache

#### ASP.NET Core
MemoryCache can be utilized
``` csharp
// check cache for value
if(!_cache.TryGetValue<string>("key", out string value))
{
    value = "";
    // insert value to cache
    _cache.Set("key", value, TimeSpan.FromMinutes(5));
}
//
```
## CodeIgniter
``` php
$this->load->driver('cache', array('adapter' => 'apc', 'backup' => 'file'));

if (!$value = $this->cache->get('key'))
{
        $value = '';
        $this->cache->save('key', $value, 5 * 60);
}

return $value;
```


#### Django
``` python
cache.get_or_set('key', get_value)
```


#### Laravel
``` php
$value = Cache::get('key', function () {
    // insert value
    return '';
});
```


### Cache dependencies

#### ASP.NET Core
MemoryCache eviction callbacks can be utilized
``` csharp
var options = new MemoryCacheEntryOptions();
options.RegisterPostEvictionCallback(EvictionCallback, new[]{"related key 1", "related key 2"});
_cache.Set("key", value, options);
// ...

// this is called when "key" is evicted from cache
private void EvictionCallback(object key, object value, EvictionReason reason, object state)
{
    var relatedKeys = state as string[];
    foreach(var key in relatedKeys)
    {
        _cache.Remove(relatedKey);
    }
    // ...
}
```

#### Laravel
``` php
Cache::tags(['example-tag'])->put('key', $value, $seconds);

Cache::tags(['example-tag'])->flush();
```