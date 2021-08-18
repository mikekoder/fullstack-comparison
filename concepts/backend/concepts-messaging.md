# Messaging

## Broadcasting

#### ASP.NET Core
``` csharp
public class ExampleHub : Hub
{
    public async Task SendMessage(string message)
    {
        await Clients.All.SendAsync("ReceiveMessage", message);
    }
}

services.AddSignalR();

app.UseEndpoints(endpoints =>
{
    endpoints.MapHub<ExampleHub>("/example");
});
```

#### FastAPI
``` python
@app.websocket("/example")
async def websocket_endpoint(websocket: WebSocket):
    await websocket.accept()
    while True:
        data = await websocket.receive_text()
        await websocket.send_text("data")
```

#### Laravel
``` php
class ExampleUpdated implements ShouldBroadcast
{
    public $example;

    public function broadcastOn()
    {
        return new PrivateChannel('example.'.$this->example->id);
    }
}

Broadcast::channel('example.{exampleId}', function ($user, $exampleId) {
    // authorize
    return true;
});
```


#### NestJS
``` ts
@WebSocketGateway()
export class EventsGateway {
  @WebSocketServer()
  server: Server;

  @SubscribeMessage('example')
  async identity(@MessageBody() data: string): Promise<string> {
    // ...
  }
}
```

## Events

#### ASP.NET Core
``` csharp
public class ExampleService
{
    public EventHandler<string> ExampleEvent;

    public void DoWork()
    {
        this.ExampleEvent?.Invoke(this, "hello");
    }
}

var service = new ExampleService();
service.ExampleEvent += (sender, e) => 
{
  //...
}
```

#### Laravel
``` php
class ExampleEvent
{
    use Dispatchable, InteractsWithSockets, SerializesModels;

    public $example;

    public function __construct(Example $example)
    {
        $this->example = $example;
    }
}

class ExampleListener
{
    public function handle(ExamplEvent $event)
    {
        // ...
    }
}
public function boot()
{
    Event::listen(
        ExampleEvent::class,
        [ExampleListener::class, 'handle']
    );

}

class ExampleController extends Controller
{
  public function update(Request $request)
    {
        $example = //...

        ExampleEvent::dispatch($example);
    }
}
```

#### NestJS

``` ts
constructor(private eventEmitter: EventEmitter2) {}

this.eventEmitter.emit(
  'example-event',
  data,
);

@OnEvent('example-event')
handleOrderCreatedEvent(payload: ExampleEventModel) {
  // ...
}
```