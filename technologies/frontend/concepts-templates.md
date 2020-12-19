# Templates

## Interpolation

#### React
 ``` xml
<span>Today is {formatDate(today)}</span>
 ```

#### Angular
``` html
<span>Today is {{ formatDate(today) }}</span>
```

#### Vue
``` html
<span>Today is {{ formatDate(today) }}</span>
```
## Conditionals

#### React
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

#### Angular
``` html 
<div>
  <span *ngIf*="someCondition">This is rendered when condition is true</span>
</div>
```

#### Vue
``` html 
<div>
  <span v-if="someCondition">This is rendered when condition is true</span>
</div>
```
## Loops

#### React
``` xml
<ul>
{items.map((item) =>
  <li>{item.text}</li>
)}
</ul>
```
#### Angular
``` html
<ul>
  <li *ngFor*="let item of items">{{ item.text }}</li>
</ul>
```

#### Vue
``` html
<ul>
  <li v-for="item in items">{{ item.text }}</li>
</ul>
```
## Input / Model binding

#### React
``` xml
nameChanged(event) {
  this.setState({name: event.target.value});
}

<input type="text" value={this.state.name} onChange={this.nameChanged} />
```

#### Angular

#### Vue
``` html
<input type="text" v-model="name" />
```

## Input validation

#### React

#### Angular

#### Vue

## Attributes

#### React
``` xml
<img src={product.imageUrl} />
```

#### Angular
``` html
<img [src]="product.imageUrl" />
```

#### Vue
``` html
<img :src="product.imageUrl" />
```

## Class

#### React

#### Angular

#### Vue

## Events

#### React
``` html
<button onClick={handleClick}>...</button>
```

#### Angular
``` html
<button (click)="handleClick">...</button>
```

#### Vue
``` html
<button @click="handleClick">...</button>
```

## Dynamic component selection

#### React

#### Angular

#### Vue

## Scoped styles

#### React

#### Angular

#### Vue