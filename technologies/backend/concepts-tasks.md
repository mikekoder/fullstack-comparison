
# Tasks

## Task scheduling

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