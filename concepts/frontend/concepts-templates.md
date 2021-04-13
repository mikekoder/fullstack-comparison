# Interpolation

## Angular
``` html
<span>Today is {{ formatDate(today) }}</span>
```

## Aurelia
``` html
<span>Today is ${formatDate(today)}</span>
``` 

## Blazor
``` html
<span>Today is @formatDate(today)</span>
```

## React
 ``` xml
<span>Today is { formatDate(today) }</span>
 ```

## Svelte
 ``` xml
<span>Today is { formatDate(today) }</span>
 ```

## Vue
``` html
<span>Today is {{ formatDate(today) }}</span>
```

# Raw HTML

## Angular
``` html
<div [innerHTML]="text"></div>
```

## Blazor
``` csharp
var html = new MarkupString(text);
```
``` html
<div>@html</div>
```

## React
``` jsx
<div
  dangerouslySetInnerHTML={{
    __html: text
  }}>
</div>
```
## Svelte
``` html
{@html text}
```

## Vue
``` html
<div v-html="text"></div>
```

# Conditionals

## Angular
``` html 
<div>
  <span *ngIf*="someCondition">This is rendered when condition is true</span>
</div>
```

## Aurelia
``` html
<div>
  <span if.bind="someCondition">This is rendered when condition is true</span>
</div>
```

## Blazor
``` html
<div>
@if(someCondition)
{
  <span>This is rendered when condition is true</span>
}
</div>
```

## React
``` xml
<div>
{someCondition &&
  <span>
    This is rendered when condition is true
  </span>
}
<div>

<div>
    {someCondition
      ? <span>This is rendered when condition is true</span>
      : <span>This is rendered when condition is false</span>
    }
  </div>
```

## Svelte
``` html
{#if someCondition}
	<span>This is rendered when condition is true</span>
{:else}
	<span>This is rendered when condition is false</span>
{/if}
```

## Vue
``` html 
<div>
  <span v-if="someCondition">This is rendered when condition is true</span>
</div>
```
# Loops

## Angular
``` html
<ul>
  <li *ngFor*="let item of items">{{ item.text }}</li>
</ul>
```

## Aurelia
``` html
<ul>
  <li repeat.for="item of items">${item.text}</li>
</ul>
```

## Blazor
``` html
<ul>
@foreach (var item in items)
{
  <li>@item.text</li>
}
</ul>
```

## React
``` xml
<ul>
{items.map((item) =>
  <li>{item.text}</li>
)}
</ul>
```

## Svelte
``` html
<ul>
	{#each items as item}
		<li>{item.text}</li>
	{/each}
</ul>
```

## Vue
``` html
<ul>
  <li v-for="item in items">{{ item.text }}</li>
</ul>
```

# Input / Model binding
## Angular
``` ts
export class SomeComponent {
  name = new FormControl('');
}
```
``` html
<input type="text" [formControl]="name">
```


## Aurelia
``` ts
export class SomeComponent {
  name: string;
}
```
``` html
<input type="text" value.bind="name">
```

## Blazor
``` csharp
private string name;
```
``` html
<input type="text" @bind="name" />
```

## React
``` jsx
nameChanged(event) {
  this.setState({name: event.target.value});
}

<input type="text" value={this.state.name} onChange={this.nameChanged} />
```

## Svelte
``` html
<input bind:value={name}>
```

## Vue
``` html
<input type="text" v-model="name" />
```

# Input validation
## Angular
``` ts
name: new FormControl(this.hero.name, [Validators.required]);
```
``` html
<input id="name" class="form-control" formControlName="name" />
<div *ngIf="name.errors.required">
  Name is required
</div>
```
## react-validation
``` ts
const required = (value) => {
  if (!value) {
    return 'Field is required';
  }
};
```
``` jsx
<Input validations={[required]}/>
```

## Svelte
``` ts
 const [ validity, validate ] = createFieldValidator(requiredValidator())
```
``` html
<input type="text" bind:value={name} use:validate={name}/>
{#if !$validity.valid}
<span>Field is required</span>
{/if}
```

## Vuelidate (Vue)
Define validations
``` js
export default {
  validations: {
    name: {
      required
    }
  }
}
```
Check validility
``` html
<input v-model.trim="$v.name.$model"/>
<div v-if="!$v.name.required">Name is required</div>
```
## Vuetify (Vue)
``` html
<v-text-field v-model="name" :rules="[val => !!val || 'Name is required']"></v-text-field>
```

## Quasar (Vue)
``` html
<q-input v-model="name" :rules="[val => !!val || 'Field is required']" />
```

# Attributes
## Angular
``` html
<img [src]="product.imageUrl" />
```

## Aurelia
``` html
<img src.bind="product.imageUrl" />
```

## React
``` xml
<img src={product.imageUrl} />
```

## Svelte
``` html
<img src={product.imageUrl} />
```

## Vue
``` html
<img :src="product.imageUrl" />
```

# Class

## Angular
``` html
<div [ngClass]="{ active: selected === 'first' }"></div>
<div [ngClass]="{ active: selected === 'second' }"></div>
```

## Aurelia
``` html
<div class.bind="{ active: selected === 'first' }"></div>
<div class.bind="{ active: selected === 'second' }"></div>
```

## React
``` jsx
<div className={ selected === 'first' ? 'active' : '' }></div>
<div className={ selected === 'second' ? 'active' : '' }></div>
```

## Svelte
``` html
<div class:active="{selected === 'first'}"></div>
<div class:active="{selected === 'second'}"></div>
```

## Vue
``` html
<div v-bind:class="{ active: selected === 'first' }"></div>
<div v-bind:class="{ active: selected === 'second' }"></div>
```

# Events / Event modifiers

## Angular
``` html
<button (click)="handleClick()">...</button>
```

## Aurelia
``` html
<button click.trigger="handleClick()">...</button>
```

## Blazor
``` html
<button @onclick="HandleClick">...</button>
```

## React
``` html
<button onClick={handleClick}>...</button>
```

## Svelte
``` html
<button on:click={handleClick}>...</button>
<button on:click|once={handleClick}>...</button>
```

## Vue
``` html
<button @click="handleClick">...</button>
<button @click.once="handleClick">...</button>
```


# Scoped styles
## Angular
## React
## Vue


# Pipes

## Angular
``` ts
import {formatDate} from './utils'
@Pipe({name: 'formatDate'})
export class FormatDatePipe implements PipeTransform {
  transform(value: DateTime, format: string): string {
    return formatDate(value, format);
  }
}
```
Value before the pipe is the first argument of the function
``` html
{{ published | formatDate:'dd.MM.yyyy' }}
```

## React
``` ts
import { formatDate} from './utils'
// ...
{formatDate(published, 'dd.MM.yyyy')}
```

## Svelte

## Vue
``` ts
import {formatDate} from './utils'
Vue.filter('formatDate', (value: DateTime, format: string) => {
  return formatDate(value, format);
})
```
Value before the pipe is the first argument of the function
``` html
{{ published | formatDate('dd.MM.yyyy') }}
```

# Directives

## Angular
``` ts
@Directive({
  selector: '[exampleDirective]'
})
export class ExampleDirective {
  @Input('exampleDirective') arg: number;

  constructor(el: ElementRef) {
    // do something with the element
    el.focus
  }

  @HostListener('mouseenter') 
  onMouseEnter() {
    // react to mouse enter event
  }
}
```
``` html
<div [exampleDirective]="123">...</div>
```

## React
``` ts
export function example(node, argument) {
  // react to events
	const handleMousedown = () => {
		
	};
}
```

``` html
<div use:example={123}>...</div>
```

## Svelte

## Vue
``` ts
Vue.directive('example', {
  inserted: (el, binding) => {
    // do something with the element
    el.focus()
    // can use arguments
    const arg = binding.argument,
  }
})
```
``` html
<div v-example="123">...</div>
```
