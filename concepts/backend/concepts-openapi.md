# OpenAPI

#### ASP.NET Core
Following is added by default when the project is created.
Api documentation is reflected from the controllers automatically.
``` csharp
// ASP.NET Core

services.AddSwaggerGen(c =>
{
    c.SwaggerDoc("v1", new OpenApiInfo { Title = "My API", Version = "v1" });
});

app.UseSwagger();
app.UseSwaggerUI(c => c.SwaggerEndpoint("/swagger/v1/swagger.json", "My API v1"));
```

#### FastAPI
Enabled by default


#### NestJS
``` ts
// NestJS
const options = new DocumentBuilder()
  .setTitle('My API')
  .setVersion('1.0')
  .build();
  
const document = SwaggerModule.createDocument(app, options);
SwaggerModule.setup('api', app, document);

```