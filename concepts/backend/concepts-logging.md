
# Logging
## Automatic
- ASP.NET Core
- Dropwizard
- NestJS


#### hapi
``` js
server.events.on('log', (event, tags) => {
  // ...
});
```

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

#### CakePHP
``` php
Log::setConfig('info', [
    'className' => FileLog::class,
    'path' => LOGS,
    'levels' => ['info'],
    'file' => 'info',
]);
```

#### Django
``` python
LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'handlers': {
        'console': {
            'class': 'logging.StreamHandler',
        },
    },
    'root': {
        'handlers': ['console'],
        'level': 'WARNING',
    },
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
# FastAPI
logger.setLevel(logging.DEBUG)
```

#### Flask
``` python
dictConfig({
    # ...
    'root': {
        'level': 'INFO',
        'handlers': ['wsgi']
    }
})
```

#### hapi
``` js
server.events.on('log', (event, tags) => {
  if (tags.error) {
    // ...
  }
});
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

#### play Framework
``` xml
<configuration>
  <!-- ... -->
  <logger name="play" level="INFO" />
  <root level="WARN">
    <appender-ref ref="ASYNCSTDOUT" />
  </root>
</configuration>
```

#### Quarkus
``` plain
quarkus.log.level=INFO
quarkus.log.category."org.hibernate".level=DEBUG
```

#### Spring
``` plain
logging.level.root=WARN
```

#### Symfony
``` php
return static function (MonologConfig $monolog) {
    $monolog->handler('file_log')
        ->type('stream')
        ->path('%kernel.logs_dir%/%kernel.environment%.log')
        ->level('debug');
};
```