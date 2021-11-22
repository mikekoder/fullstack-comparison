# Component

## Angular
Components are split to template (.html), code (.ts) and style (.scss) files.
Template is html with special Angular markup
``` html
<div>Markup</div>
```
Script exports component class.
@Component decorator is used to configure tag name and template and style files.
``` ts
import { Component, Input, OnInit } from '@angular/core';

@Component({
  selector: 'example',
  templateUrl: './example.component.html',
  styleUrls: ['./example.component.scss']
})
export class ExampleComponent implements OnInit {
  // state, methods etc.
  ngOnInit(): void { }
}
```

## Blazor
Components are in .razor files which contains template, code and possibly some attributes like route
``` csharp
@page "/"

<div>Markup</div>

@code {
  // state, methods etc.
}
``` 

## React
Components are in .tsx files
``` ts
import React, { useState } from 'react';

function Example() {
  // state, functions etc.
  
  return (
    <div>
      Markup
    </div>
  );
}

export default App;
```

## Vue
Components can be in single .vue files which has three parts: template, script, style.  
Any of those parts can also be separated to distinct files (.html, .ts, .scss).

### Class API
``` ts
<template>
  Markup
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component';

export default class Test extends Vue {
  // state, methods etc.
}
</script>
<style lang="scss>
// styles
</style>
```
### Composition API
``` ts
<template>
  Markup
</template>

<script lang="ts">
export default {
  setup(){
    // state etc.
  }
}
</script>
<style lang="scss>
// styles
</style>
```

### Many files
```
<template src="example.html">
</template>
<script src="example.ts">
</script>
<style src="example.scss">
</style>
```


# State

## Angular
Public properties are accessible in the view
``` ts
firstName: string;
```

## Aurelia
Public properties are accessible in the view
```
firstName: string;
```

## Blazor
Variables are accessible in the view
```csharp
private string firstName;
```

## React
``` ts
const [firstName, setFirstName] = useState('');
```

## Svelte
``` ts
let firstName = '';
```

## Vue options api
```
data() {
  return {
    firstName: ''
  }
}
```

## Vue class api
Public properties are accessible in the view
``` ts
firstName: string;
```

## Vue hooks
``` ts
const firstName = ref('')
const product = reactive({ id: 1, name: 'some product' })
```

# Computed state / Memoisation

## Angular

Not available

## Aurelia
``` ts
@computedFrom('firstName', 'lastName')
get fullName(): string {
  return `${this.firstName} ${this.lastName}`;
}
```

## Blazor
Just C# getters
``` csharp
public string FullName => $"{FirstName} {LastName}";
```

## React
``` ts
const name = useMemo(() => (firstName, lastName) => `${firstName} ${lastName}`, [firstName, lastName]);
```


## Svelte
``` ts
$: name = `${firstName} ${lastName}`;
```

## Vue options api
``` ts
computed: {
  name: (){
    return `${this.firstName} ${this.lastName}`;
  }
}
```

## Vue class api
Property accessors
``` ts
get name() {
  return `${this.firstName} ${this.lastName}`;
}
```

## Vue hooks
``` ts
const firstName = ref('')
const lastName = ref('')
const name = computed(() =>`${firstName.value} ${lastName.value}`)
```

# State change detection

## Angular

Changes in inputs can be detected, but not changes in properties
``` ts
ngOnChanges(changes: SimpleChanges) {
    // ...
  }
```

## Aurelia
``` ts
@observable category: string;

// property name + "Changed"
categoryChanged(newValue, oldValue) {
  this.loadProducts();
}
```

## Blazor
``` csharp
@code
{
   Person person = new Person();
   protected override void OnInitialized()
   {
        EditContext = new EditContext(person);
        EditContext.OnFieldChanged += EditContext_OnFieldChanged;
       
        base.OnInitialized();
   }     

   
    // Note: The OnFieldChanged event is raised for each field in the 
     // model
    private void EditContext_OnFieldChanged(object sender,  FieldChangedEventArgs e)
    {
        Console.WriteLine(e.FieldIdentifier.FieldName);

    }
    
}
```

## React class
``` ts
componentDidUpdate(prevProps, prevState) {
  if(this.state.category !== prevState.category){
    this.loadProducts();
  }
}
```

## React function
``` ts
useEffect(() => {
  // Update the document title using the browser API
  document.title = `You clicked ${count} times`;
});
```

## Svelte
Can react to changes, but the old value isn't known
``` ts
$: if (category) {
  // ...
}
```

## Vue options api
``` ts
watch: {
  category() {
    this.loadProducts();
  }
}
```

## Vue class/decorator api
``` ts
@Watch('category')
onCategoryChanged(val: string, oldVal: string) {
  this.loadProducts();
}
```

## Vue composition api
``` ts
watch(category, (newValue, oldValue) => {
  loadProducts();
})
```


# Passing data to component

## Angular
Defining inputs
```ts
@Input() data: number;
```
Passing data
``` html

<MyComponent [data]="123" />
```

## Aurelia
Defining bindable properties
``` ts
@bindable data: number;
```
Passing data
``` html
<require from='my-component'></require>
  
<my-component data.bind="123"></my-component>
```

## Blazor
Defining public property
``` csharp

[Parameter]
public int Data { get; set; } = 1;
```

Passing data
``` html


<MyComponent Data="123" />
```

## React
Defining props
``` ts
function MyComponent(props) {
  const number = props.data;
}
```
Passing data
``` jsx


<MyComponent data={123} />


```

## Svelte
Define prop
``` ts
export let data;
```

Passing data
``` html
<MyComponent data={42}/>
```

## Vue class/decorator api

Defining props
``` ts
@Prop({ default: 123 }) readonly data: number
```

Passing data
``` html
<MyComponent :data="123" />
```


## Vue hooks

Defining props
```ts
setup (props) {
  const number = props.data;
}
```

Passing data
``` html
<MyComponent :data="123" />
```

# Passing data from component

## Angular
Define output
``` ts
@Output() change = new EventEmitter<string>();
```
Calling output
``` ts
valueChanged(newVal): void {
  this.change.emit(val);
}
```
Define handler
``` ts
handleChange(newVal): void {
  // ...
}
```
Bind handler to event
``` html
<MyComponent (change)="handleChange" />
```

## Aurelia
Publish events
``` ts
constructor(private bus: EventAggregator) { }

valueChanged(newVal): void {
  this.bus.publish(new MyMessage(val));
}
```
Subscribe to events
``` ts
constructor(private bus: EventAggregator) {
  bus.subscribe(MyMessage, msg => {
    //
  });
}
```

## Blazor
Define callback
``` csharp
[Parameter]
public EventCallback<int> OnValueChanged { get; set; }
```
Invoke callback
``` csharp
await OnValueChanged.InvokeAsync(123)
```
Listen events
``` csharp
<MyComponent OnValueChanged="@HandleChange" />

@code {
  private void HandleChange(int i)
  {
      //...
  }
}
```

## React

Props include callback function
``` ts
function MyComponent(props) {
  
  //...
  const valueChanged = (val) => {
    props.onchange(val);
  }
}
```
Define handler
``` ts
const handleChange = (newVal) => {
  //...
}
```

Pass handler as prop
``` jsx
<MyComponent onchange={handleChange} >
```

## Svelte

Dispatch events
``` ts
import { createEventDispatcher } from 'svelte';
const dispatch = createEventDispatcher();

function valueChanged(newVal) {
  dispatch('change', newVal);
}
```
Define handler
``` ts
handleChange(val) {
  // ...
}
```

Bind handler to event
``` html
<MyComponent on:change={handleChange}/>
```

## Vue class/decorator api

Define emitting method
``` ts
@Emit()
change(val) {
  return  val;
}
```

Define handler
``` ts
handleChange(newVal): void {
  // ...
}
```

Bind handler to event
``` html
<MyComponent @change="handleChange" />
```

## Vue hooks
``` ts
setup(props, { emit }) {

  const valueChanged = (newVal: string) => {
    emit('change', newVal)
  }
}
```

# Passing data deep down

## Angular
Not available

## Blazor
Define cascading value
``` csharp

// pass value
<CascadingValue Value="@ExampleValue" Name="someData">
  @Body
</CascadingValue>
```

Read cascading value
``` csharp

// read value
@code {
    [CascadingParameter(Name = "someData")]
    protected ExampleType ExampleParameter { get; set; }
}
```


## React
Define context
``` ts

// define context
const ExampleContext = React.createContext({ number: 123 });
```
Assign data
``` jsx

// pass value
<ExampleContext.Provider value={overrideData}>
  <SomeComponent />
</ExampleContext.Provider>
```
Consume data in descendant component
``` jsx

// read value
<ExampleContext.Consumer>
  {contextData => (
    <MyComponent data={contextData} />
  )}
</ExampleContext.Consumer>
```

## Svelte

Define context values
``` ts

// ancestor component
setContext('someData', {
	number: 123
});

// descendant component
const { number } = getContext('someData');

```

Read value from context
``` ts
const { number } = getContext('someData');
```
## Vue class/decorator api

Define data that is passed to descendant components
``` ts
@Provide() someData = { number: 123 }
```

Read data provided by ancestor component
``` ts
@Inject readonly someData: { number: number }
```
## Vue hooks
Define data that is passed to descendant components
``` ts


// pass data
setup() {
  provide('someData', { number: 123 })
}

// read data
setup() {
  const userLocation = inject('someData')
}


```
Read data provided by ancestor component
``` ts
// read data
setup() {
  const userLocation = inject('someData')
}
```

# Transclusion

## Angular
Place ng-content where inner content should go
``` html
<!-- LayoutComponent -->
<div class="menu">
</div>
<div class="main">
  <ng-content></ng-content>
</div>
```
ng-content above is replaced with component's inner markup
``` html
<LayoutComponent>
  <div>
    <div>Main content</div>
  </div>
</LayoutComponent>
```
To use multiple "placeholders" define selector for ng-content
``` html
<!-- LayoutComponent -->
<div class="menu">
  <ng-content select="[sidebar]"></ng-content>
</div>
<div class="main">
  <ng-content select="[content]"></ng-content>
</div>
```
ng-content is replaced with elements mathing those selectors
``` html
<LayoutComponent>
  <div sidebar>
    Sidebar content
  </div>
  <div content>
    Main content
  </div>
</LayoutComponent>
```

## Blazor
Catch & render
``` html

<div class="menu">
  @Sidebar
</div>
<div class="main">
  @Content
</div>

```
``` csharp
@code {
    [Parameter]
    public RenderFragment Sidebar { get; set; }

    [Parameter]
    public RenderFragment Content { get; set; }
}
```
Pass values by using parameter names as tag names
``` jsx
<ExampleComponent>
    <Sidebar>
      Sidebar content
    </Sidebar>
    <Content>
      Main content
    </Content>
</ExampleComponent>
```


## React
Render props.children where inner content should go
``` jsx
const Layout = (props) => {
  return (
    <div>
      <div class="menu">
      </div>
      <div class="main">
        {props.children}
      </div>
    </div>
  )
}
```
Inner markup is assigned to props.children
``` jsx
<Layout>
  <div>Main content</div>
</Layout>
```

More "placeholders" can be rendered from props
``` jsx
<div class="menu">
  {props.menuContent}
</div>
<div class="main">
  {props.children}
</div>
```
Define variable with markup
``` jsx
const menu = <div>Menu content</div>
```
Pass it as prop
``` jsx
<Layout menuContent={menu}>
  <div>Main content</div>
</Layout>
```

## Svelte
Define slot where inner content should go
``` html
<!-- LayoutComponent -->
<div class="menu">
</div>
<div class="main">
  <slot></slot>
</div>
```

Slot is replaced with component's inner markup
``` html
<LayoutComponent>
  <div>
    <div>Main content</div>
  </div>
</LayoutComponent>
```

To use multiple "placeholders" define names for slots (slot without name is default)
``` html
<!-- LayoutComponent -->
<div class="menu">
  <slot name="sidebar"></slot>
</div>
<div class="main">
  <slot name="content"></slot>
</div>
```

Slots are replaced with elements having matching slot name (elements without slot go to the default slot)
``` html
<LayoutComponent>
  <div slot="sidebar">
    Sidebar content
  </div>
  <div slot="content">
    Main content
  </div>
</LayoutComponent>
```

Checking whether slot has content
``` html
{#if $$slots.sidebar}
	<div class="menu">
		<slot name="sidebar"></slot>
	</div>
{/if}
```
## Vue
Define slot where inner content should go
``` html
<!-- LayoutComponent -->
<div class="menu">
</div>
<div class="main">
  <slot></slot>
</div>
```

Slot is replaced with component's inner markup
``` html
<LayoutComponent>
  <div>
    <div>Main content</div>
  </div>
</LayoutComponent>
```
To use multiple "placeholders" define names for slots (slot without name is default)
``` html
<!-- LayoutComponent -->
<div class="menu">
  <slot name="sidebar"></slot>
</div>
<div class="main">
  <slot></slot>
</div>
```
slots are replaced with elements having matching slot name (elements without slot go to the default slot)
``` html
<LayoutComponent>
  <div slot="sidebar">
    Sidebar content
  </div>
  <div>
    Main content
  </div>
</LayoutComponent>
```


# Lifecycle events

## Angular
``` ts
ngOnInit() {
  // fetch initial data
}
ngOnChanges() {
  // react to changes
}
ngOnDestroy() {
  // clean resources
}
```

## Aurelia
``` ts
created() {

}
bind() {

}
attached() {
  // component is attached to DOM
}
detached() {
  // component is removed from the DOM
}
unbind() {

}
```

## Blazor
``` csharp
protected override void OnInitialized()
{
  // initial parameters are set
}
protected override void OnParametersSet()
{
  // react to changes
}
protected override void OnAfterRender(bool firstRender)
{
  // dom is ready
}
public void Dispose()
{
  // component is disposed
}
```

## React class api
``` ts
componentDidMount() {
  // dom ready
}
componentDidUpdate() {
  // react to changes
}
componentWillUnmount() {
  // clean resources
}
```

## React function
``` ts
useEffect(() => {
    // Update the document title using the browser API
    document.title = `You clicked ${count} times`;
  });
```

## Svelte
``` ts
onMount(async () => {
  // fetch initial data
});

onDestroy(() => {
  // clean resources
});
```

## Vue class/decorator api
``` ts
created() {
  // fetch initial data
}
mounted() {
  // dom ready
}
updated() {
  // react to changes
}
destroyed() {
  // clean resources
}
```
## Vue hooks
``` ts
export default {
  setup() {
    // fetch initial data

    onMounted(() => {
      // dom ready
    })

    onUpdated(() => {
      // react to changes
    });

    onUnmounted(() => {
      // clean resources
    });
  }
}
```

# Element reference

## Angular
Define selector to element
``` html
<canvas #myRef></canvas>
```

``` ts
 @ViewChild('myRef') canvas: ElementRef;
 ```

## Blazor
```
<MyComponent @ref="myRef" />

@code {
    private MyComponent myRef;
}
```


## React
Create ref
``` jsx
this.myRef = React.createRef();
```

Assign ref to element
``` jsx
<canvas ref={this.myRef} />;
```
## Svelte
``` ts
let myRef;
```

``` html
<canvas bind:this={myRef}></canvas>
```

## Vue
Define ref-attribute to element
``` html
<canvas ref="myRef"></canvas>
```

References are accessible from $refs
``` ts
const el = this.$refs.myRef;
```

# Dynamic component selection

## Angular
https://angular.io/guide/dynamic-component-loader

## Blazor
``` csharp
@componentToDisplay

@code {
    var componentToDisplay = someCondition ? ComponentA : ComponentB;

    static readonly RenderFragment ComponentA = _ =>
    {
        <ComponentA />
    };

    static readonly RenderFragment ComponentB = _ =>
    {
        <ComponentB />
    };
}
```


## React
Components can be assigned to variables
``` ts
const componentToDisplay = someCondition ? <ComponentOne /> : <ComponentTwo>
```

``` jsx
<div>{component}</div>
```
## Svelte
Component type can be assigned to a variable
``` ts
const componentToDisplay = someCondition ? ComponentOne : ComponentTwo
```
Special attribute is used to dynamically display the component
``` html
<svelte:component this={componentToDisplay}/>
```

## Vue
Component tag name can be assigned to a variable
``` ts
const componentToDisplay = someCondition ? 'component-one' : 'component-two';
```
Special attribute is used to dynamically display the component
``` html
<component :is="componentToDisplay"></component>
```

# DOM reference
``` Angular

```


## Vue.js
``` jsx
<template>
  <div ref="map"></div>
</template>
<script>
  import { ref, onMounted } from 'vue'

  export default {
    setup() {
      const map = ref(null)
      onMounted(() => {
        // render map
      })

      return {
        map
      }
    }
  }
</script>
```