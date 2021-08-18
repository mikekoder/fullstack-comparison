
# Tasks

## Task scheduling
#### ASP.NET Core
``` csharp
// ASP.NET Core
public class TimedHostedService : IHostedService, IDisposable
{
    private Timer _timer;

    public Task StartAsync(CancellationToken stoppingToken)
    {
        _timer = new Timer(RefreshData, null, TimeSpan.Zero, TimeSpan.FromMinutes(5));
        return Task.CompletedTask;
    }   
    public Task StopAsync(CancellationToken stoppingToken)
    {
        _timer?.Change(Timeout.Infinite, 0);
        return Task.CompletedTask;
    }
    public void Dispose()
    {
        _timer?.Dispose();
    }
    private void RefreshData(object state)
    {
        // ...
    }
}
```

#### Laravel
``` php
class Kernel extends ConsoleKernel
{
    protected function schedule(Schedule $schedule)
    {
        $schedule->call(function () {
            // ...
        })->cron('* */5 * * *');
    }
}
```

#### NestJS
``` ts
// NestJS
@Injectable()
export class TasksService {

  @Cron('0 */5 * * * *')
  handleCron() {
    // ...
  }
}
```

#### Spring / Spring Boot

``` java
@Scheduled(fixedDelay = 1000)
public void handleFixedDelayTask() {
    // ...
}

@Scheduled(cron = "0 */5 * * * *")
public void handleCronTask() {
    // ...
}
```