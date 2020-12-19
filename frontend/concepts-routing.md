# Routing

Angular (built-in)


## Basic
#### Angular

#### react-router-dom
``` xml
<Router>
  <div>
    <nav>
      <!-- links-->
    </nav>

    <Switch>
      <Route path="/products">
        <ProductList />
      </Route>
      <Route path="/products/:id">
        <ProductDetails />
      </Route>
      <Route path="/">
        <Home />
      </Route>
    </Switch>
  </div>
</Router>
```
``` ts
let { id } = useParams();
```

#### vue-router


## Meta
#### Angular
``` ts
```
#### react-router-dom
``` xml
```
#### vue-router
``` ts
```

## Guards
#### Angular
``` ts
```
#### react-router-dom
``` xml
```
#### vue-router
``` ts
```

## Before / after
#### Angular
``` ts
```
#### react-router-dom
``` xml
```
#### vue-router
``` ts
```

## Reverse / generate url
#### Angular
``` ts
```
#### react-router-dom
``` ts
import { generatePath } from "react-router";

generatePath("/products/:id", {
  id: 123,
});
```
#### vue-router
``` ts
```

## Nested routes
#### Angular
``` ts
```

#### react-router-dom
``` xml
<Switch> 
  <Route path="/products/:id">
    <ProductDetails />
  </Route>
  <!---->
</Switch>
```
``` xml
<!-- ProductDetails -->
<Switch>
  <Route exact path="/">
    <!-- /products/:id -->
  </Route>
  <Route path="/reviews">
    <!-- /products/:id/reviews -->
  </Route>
</Switch>
```

#### vue-router
``` ts
```

## Named placeholders
#### Angular
``` ts
```
#### react-router-dom
``` xml
```
#### vue-router
``` ts
```

## Wildcards / no match
#### Angular
``` ts
```
#### react-router-dom
``` xml
<Route path="*">
  <NotFound />
</Route>
```
#### vue-router
``` ts
```

## Redirect
#### Angular
``` ts
```
#### react-router-dom
``` xml
<Route
  render={({ location }) =>
    someCondition ? (
      children
    ) : (
      <Redirect to={{ pathname: "/login" }} />
    )
  }
/>
```
#### vue-router
``` ts
```