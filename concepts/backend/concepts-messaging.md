# Messaging

## Broadcasting

#### ASP.NET Core
``` csharp
// ASP.NET Core
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

#### Quarkus
``` java

@ServerEndpoint("/ws")         
@ApplicationScoped
public class ChatSocket {

    Map<String, Session> sessions = new ConcurrentHashMap<>(); 

    @OnOpen
    public void onOpen(Session session) {
        sessions.put(username, session);
    }

    @OnClose
    public void onClose(Session session) {
        sessions.remove(username);
    }

    @OnMessage
    public void onMessage(String message) {
        sessions.values().forEach(s -> {
            s.getAsyncRemote().sendObject(message, result ->  {
                // ...
            });
        });
    }
}
```

#### ServiceStack
``` csharp
Plugins.Add(new ServerEventsFeature());

ServerEvents.NotifyChannel("channel", "selector", data);
```

#### Spring
``` java
@Controller
public class ExampleController {
  @MessageMapping("/example")
  @SendTo("/topic/example")
  public OutgoingMessage broadcast(IncomingMessage message) throws Exception {
    // ...
    return new OutgoingMessage();
  }
}

@Configuration
@EnableWebSocketMessageBroker
public class WebSocketConfig implements WebSocketMessageBrokerConfigurer {

  @Override
  public void configureMessageBroker(MessageBrokerRegistry config) {
    config.enableSimpleBroker("/topic");
    config.setApplicationDestinationPrefixes("/app");
  }

  @Override
  public void registerStompEndpoints(StompEndpointRegistry registry) {
    registry.addEndpoint("/gs-guide-websocket").withSockJS();
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

#### CakePHP
``` php
$event = new Event('Example_event', $this, [
    'data' => $data
]);
$this->getEventManager()->dispatch($event);


$this->Example->getEventManager()->on('Example_event', function ($event) {
    // ...
});
```

#### Flask
``` python
# Flask
my_signals = Namespace()
example_signal = my_signals.signal('example')

# subscribe
def handle_signal(sender, template, context, **extra):
    # ...

example_signal.connect(handle_signal, app)

# emit
def save(self):
    example_signal.send(self)
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

#### ServiceStack
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

#### Spring
``` java
public class ExampleEvent extends ApplicationEvent {
    private String message;

    public ExampleEvent(Object source, String message) {
        super(source);
        this.message = message;
    }
    public String getMessage() {
        return message;
    }
}

@Component
public class ExampleEventPublisher {
    @Autowired
    private ApplicationEventPublisher applicationEventPublisher;

    public void publishEvent(final String message) {
        ExampleEvent exampleEvent = new ExampleEvent(this, message);
        applicationEventPublisher.publishEvent(exampleEvent);
    }
}

@Component
public class ExampleEventListener implements ApplicationListener<ExampleEvent> {
    @Override
    public void onApplicationEvent(ExampleEvent event) {
        // ...
    }
}
```

#### Symfony
``` php
class ExampleEvent extends Event
{
    public const NAME = 'example.event';
    // ...
}

class ExampleListener
{
    public function onExampleAction(Event $event)
    {
        // ...
    }
}

$dispatcher = new EventDispatcher();
$listener = new ExampleListener();
$dispatcher->addListener('example.event', [$listener, 'onExampleAction']);

$event = new ExampleEvent();
$dispatcher->dispatch($event, ExampleEvent::NAME);
```