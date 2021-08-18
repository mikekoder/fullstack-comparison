## Angular
Create service with subject
``` ts
@Injectable({
  providedIn: 'root',
})
export class ProfileService {
  private currentUserSubject = new BehaviorSubject<UserProfile>(null);
  // limit other components to subscribing only
  public currentUser: Observable<UserProfile> = this.currentUserSubject;

  public updateProfile(profile: UserProfile): void {
    // save somewhere
    // notify subscribers
    this.currentUserSubject.next(profile);
  }
}
```

Inject service to component
``` ts 
constructor(private profileService: ProfileService) {
}

ngOnInit(): void {
  // subscibe to changes
  this.profileService.currentUser.subscribe(profile => {
    // react to new value
    this.name = profile.name;
  });
}

save(): void {
  // call methods to alter service state
  this.profileService.updateProfile({/*...*/});
}

```

## Blazor
Libraries like [Blazor-State](https://github.com/TimeWarpEngineering/blazor-state)


Create service
``` csharp
public class ProfileService
{
  public event Action OnChange;

  private Profile _profile;
  public Profile Profile
  {
    get => profile;
    set 
    {
      _profile = value;
      OnChange?.Invoke();
    }
  }
}
```
Register service
``` csharp
builder.Services.AddSingleton<ProfileService>();
```
Use service
``` csharp
@implements IDisposable
@inject ProfileService ProfileService

<span>@ProfileService.Profile.Email</span>

@code {
    protected override void OnInitialized()
    {
        ProfileService.OnChange += StateHasChanged;
    }
    private void UpdateProfile()
    {
        var profile = new Profile{ /* ... */ };
        ProfileService.Profile = profile;
    }

    public void Dispose()
    {
        ProfileService.OnChange -= StateHasChanged;
    }
}
```

## React (redux)

Define actions
``` ts
import { UPDATE_PROFILE } from './actionTypes'

export const updateProfile = profile=> ({
  type: UPDATE_PROFILE,
  profile
})
```

Define action handlers
``` ts
export function profile(state = [], action) {
  switch (action.type) {
    case ActionTypes.UPDATE_PROFILE:
      // modify state
      const currentUser = { ...action.profile }
      return [...state, currentUser]
    default:
      return state
  }
}
```

Make store available to the app
``` jsx
<Provider store={store}>
  <ExampleApp />
</Provider>
```
Map state and actions to component
``` jsx
const mapStateToProps = (state, ownProps) => ({
  user: state.currentUser
  // ...
})

const mapDispatchToProps = {
  updateProfile // imported from actions
  // ...
}

connect(
  mapStateToProps,
  mapDispatchToProps
)(ExampleComponent)
```
## Svelte

Define store data
``` ts
// store.js
export const count = writable(0);
```

Read the value ($ prefix)
``` ts
import { count } from './store.js';
let doubled =  2 * $count:
```

Update value
``` ts
function increment() {
	count.update(n => n + 1);
}
```
## Vue (vuex)
https://github.com/championswimmer/vuex-module-decorators

Store contains state, mutations and actions
``` ts
@Module({ namespaced: true, dynamic: true, store, name: 'user' })
export default class UserModule extends VuexModule {
  currentUser: UserProfile;

  @Action
  async updateProfile(profile: UserProfile) {
    // send to api
    // mutate state
    this.SET_PROFILE(profile);
  }
  @Mutation
  private SET_PROFILE(profile: UserProfile): void {
    this.currentUser = profile;
  }
}
```
Components can access store's state and call actions
``` ts
export default class ExampleComponent extends Vue {
  // load module
  userStore = getModule(UserModule);
  // use getter to return value from store state
  get currentUser() {
    return this.userStore.currentUser;
  }

  async save() {
    // call store action
    await this.userStore.updateProfile({/* ... */ });
  }
}
```