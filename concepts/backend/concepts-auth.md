# Authentication / Authorization
A1 - Username+password: Users can be authenticated with username and password  
A2 - Social media: Users can authenticate with Facebook, Google and other social media accounts  
A3 - Azure AD: Users can authenticate using Azure AD accounts  
A4 - Roles/groups: Access can be restricted by roles, groups or similar  
A5 - JWT: Authentication and authorization information is handled by Authorization header and JWT bearer tokens  
A6 - Policies: E.g. checking claims from JWT and allowing only users having email address with domain *.example.com to certain endpoints.  
A7 - Conventions: Authorization can be applied to multiple endpoints. E.g. all routes behind /admin/ requires the user to be authenticated or having the admin role.

## Login with username & password

#### ASP.NET Core
Has templates with authentication
``` sh
dotnet new mvc -au Individual
dotnet new webapp -au Individual
```

#### Laravel
Laravel Fortify

#### NestJS
``` ts
@Injectable()
export class AuthService {
  async validateUser(username: string, pass: string): Promise<any> {
    const user = getUser(username);
    const hash = hashPassword(pass);
    if (user && user.passwordHash === hash) {
      return user;
    }
    return null;
  }
}

@Injectable()
export class UsernamePasswordStrategy extends PassportStrategy(Strategy) {
  constructor(private authService: AuthService) {
    super();
  }

  async validate(username: string, password: string): Promise<any> {
    const user = await this.authService.validateUser(username, password);
    if (!user) {
      throw new UnauthorizedException();
    }
    return user;
  }
}
```

## Login with OAuth
Logging in with 3rd party accounts (e.g. Google or Facebook) is a common scenario.

#### ASP.NET Core

Register authentication provider
``` csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddGoogle(options =>
    {
        options.ClientId = "...";
        options.ClientSecret = "...";
    })
```

Redirect to 3rd party and process response
``` csharp
// Redirect to 3rd party authentication endpoint
[HttpGet("google")]
public IActionResult GoogleLogin()
{
    var callbackUrl = Url.Action(nameof(GoogleLoginRedirect));
    var authenticationProperties = new AuthenticationProperties { RedirectUri = callbackUrl };
    return Challenge(authenticationProperties, "Google");
}

// Handle 3rd party authentication result
[HttpGet("google-redirect")]
public async Task<IActionResult> GoogleLoginRedirect()
{
    var result = await HttpContext.AuthenticateAsync(CookieAuthenticationDefaults.AuthenticationScheme);
    var email = result.Principal.FindFirstValue(ClaimTypes.Email);
    // ...
}
```

#### Laravel

Configure authentication provider
``` php
'google' => [
    'client_id' => '...',
    'client_secret' => '...',
    'redirect' => 'http://example.com/callback-url',
],
```


Redirect to 3rd party and process response
``` php
Route::get('/auth/redirect', function () {
    return Socialite::driver('google')->redirect();
});

Route::get('/auth/callback', function () {
    $user = Socialite::driver('google')->user();
    $email = $user->getEmail();
    // ...
});
```



## Login with AzureAD

#### ASP.NET Core
Has templates with authentication
``` sh
dotnet new mvc -au SingleOrg
dotnet new webapp -au SingleOrg
```

## JWT

#### Feathers.js

Register jwt authentication strategy
``` js
const authentication = new AuthenticationService(app);
authentication.register('jwt', new JWTStrategy());
```

#### ServiceStack

Register auth provider
``` csharp
Plugins.Add(new AuthFeature(...,
    new IAuthProvider[] {
        new JwtAuthProvider(AppSettings) { AuthKey = AesUtils.CreateKey() }
        //...
    }));
```

Create token
```csharp
var payload = JwtAuthProvider.CreateJwtPayload(new AuthUserSession
{
    //...
}, "issuer", TimeSpan.FromDays(1));

var token = JwtAuthProvider.CreateEncryptedJweToken(payload, privateKey);
```

#### Vert.x

Create provider
``` java
JWTAuthOptions config = new JWTAuthOptions()
  .setKeyStore(new KeyStoreOptions()
  .setPath("keystore.jceks")
  .setPassword("secret"));

AuthenticationProvider provider = JWTAuth.create(vertx, config);
```

Generate token
``` java
String token = provider.generateToken(new JsonObject().put("key", "value"), new JWTOptions());
```


## Roles

#### ASP.NET Core
``` csharp
// ASP.NET Core
[Authorize(Roles = "Admin")]
public class ProductController : ControllerBase { }
```

#### Django

#### Dropwizard
``` java
// DropWizard
@RolesAllowed("Contributor")
@Path("/example")
public class ExampleResource {

    @RolesAllowed("Admin")
    @PUT
    @Path("/{id}")
    public String updateProduct(@PathParam("id") Long id) {
        // ...
    }
}
```

#### Laravel
``` php
Gate::define('has-role', function (User $user, String $role) {
    return $user->role === $role;
});


public function delete(Request $request, Item $item)
{
    if (!Gate::allows('has-role', 'admin')) {
        // ...
    }
    // ...
}
```

#### Quarkus

#### ServiceStack

#### Symfony

## Policies

#### ASP.NET Core
``` csharp
// ASP.NET Core
services.AddAuthorization(options =>
{
    options.AddPolicy("OnlyExampleDotCom", policy =>
        policy.RequireAssertion(ctx =>
            ctx.User.Claims.First(c => c.Type == ClaimTypes.Email).Value.EndsWith("@example.com")));
});

[Authorize(Policy = "OnlyExampleDotCom")]
public class ProductController : ControllerBase { }
```

#### Laravel
```  php
class ExamplePolicy
{
    public function update(User $user, Example $example)
    {
        return $example->userId === $user->id;
    }
}
```


## Conventions

### ASP.NET Core
``` csharp
// ASP.NET Core
[Authorize(Roles = "reader")] // apply to all sub classes
public abstract class ExampleControllerBase : ControllerBase {}

[Authorize(Roles = "contributor")] // apply to all actions in this class
public class ExampleController : ControllerBase 
{ 
  [Authorize(Roles = "admin")] // apply to single action
  [HttpGet("")]
  public IActionResult SomeAction() {}
}

// apply globally
services.AddControllers(options =>
{
    options.Filters.Add(typeof(AuthorizeFilter));
});

[AllowAnonymous]
public IActionResult Login(string username, string password)
{
    // ...
}
```

### Laravel
```php
Route::put('/example', function (Example $example) {
    
})->middleware('can:update,example');
```

### ServiceStack

### Quarkus

### Symfony

### Dropwizard