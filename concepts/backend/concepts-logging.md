
# Logging
## Automatic
- ASP.NET Core
- Dropwizard
- NestJS

## Log level

#### ASP.NET Core
``` json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  }
}
```

#### Dropwizard
``` yaml
logging:
  level: INFO

  # ...
```

#### FastAPI
``` python
logger.setLevel(logging.DEBUG)
```

#### Laravel
``` php
'channels' => [
    'syslog' => [
        'driver' => 'syslog',
        'level' => 'debug',
    ]
],
```

#### NestJS
``` ts
const app = await NestFactory.create(AppModule, {
  logger: ['error', 'warn'],
});
```