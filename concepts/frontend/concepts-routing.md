# Basic
## Angular
Defining routes
``` ts
const routes: Routes = [
  { path: 'products/:id', component: ProductDetails },
  // ...
];
```
Reading route parameters
``` ts
const id = this.route.snapshot.paramMap.get('id');
```
Generating link
``` html
<a [routerLink]="['/products', product.id]">Product details</a>
```
Navigating
``` ts
this.router.navigate(['/products', { id: 123 }]);
```

## Aurelia
Defining routes
``` ts
config.map([
  { route: 'products/:id', name: 'product-details', moduleId: 'products/detail' },
]);
```
Reading route parameters
``` ts
// special activate lifecycle method
activate(params, routeConfig) {
  const id = params.id
}
```

Navigating
``` ts
this.router.navigateToRoute('product-details', { id: 123});
```


## react-router-dom
Defining routes
``` xml
<Router>
  <Switch>
    <Route name="product-details" path="/products/:id">
      <ProductDetails />
    </Route>
    <!-- -->
  </Switch>
</Router>
```

Reading route parameters
``` ts
let { id } = useParams();
```

Generating link
``` jsx
<Link to={'/products/' + product.id}>Product details</Link> 
```

Generating url
``` ts
import { generatePath } from "react-router";

generatePath("/products/:id", {
  id: 123,
});
```

## svelte-router
Defining routes
``` ts
const routes = [
  { name: 'products/:id', component: ProductDetails },
  // ...
]
```
Route values can be mapped to props
``` ts
{ 
  path: '/products/:id(\\d+)',
  name: 'PRODUCT_DETAILS',
  component: ProductDetails,
  props: (route) => {
    return {
      id: route.params.id,
    }
  },
}
```

Generating link
``` html
<RouterLink to={{name: 'PRODUCT_DETAILS', params:{id: 123}}>Product details</RouterLink>
```

Navigating
``` ts
$router.push({
  name: 'PRODUCT_DETAILS',
  params: {
    id: 123,
  }
});
```

## vue-router
Defining routes
``` ts
const router = new VueRouter({
  routes: [
    {
      path: '/products/:id',
      name: 'product-details',
      component: ProductDetails
    },
    // ...
  ]
})
```
Reading route parameters
``` ts
const productId = this.$route.params.id;
```
Generating link
``` html
<router-link :to="{ name: 'product-details', params: { id: product.id }}">Product details</router-link>
```
Navigating
```ts
this.$router.push({ name: 'product-details', params: { id: 123 }})
```

# Metadata
## Angular
Defining metadata
``` ts
const routes: Routes = [
  { 
    path: 'bugs', 
    component: IssueList,
    data: {
      issueType: 'bug'
    },
  },
  { 
    path: 'stories', 
    component: IssueList,
    data: {
      issueType: 'story'
    },
  },
];
```
Reading metadata
``` ts
const issueType = this.activatedRoute.snapshot.data.issueType;
```

## Aurelia
``` ts
{ route: 'bugs', name: 'bugs', moduleId: 'issues/index', settings: {issueType: 'bug'} },
{ route: 'stories', name: 'stories', moduleId: 'issues/index', settings: {issueType: 'story'} }
```


## react-router-dom
Not available

## svelte-router
Routes can have meta
``` ts
routes: [
  { 
    path: 'bugs',
    component: IssueList,
    meta: {
      issueType: 'bug'
    }
  },
  { 
    path: 'stories',
    component: IssueList,
    meta: {
      issueType: 'story'
    }
  }
]
```

How to access this in component is unknown
## vue-router
Defining meta to routes
``` ts
const router = new VueRouter({
  routes: [
    {
      path: '/bugs',
      component: IssueList,
      meta: { issueType: 'bug' }
    },
    {
      path: '/stories',
      component: IssueList,
      meta: { issueType: 'story' }
    }
  ]
})
```
Using meta in guards
``` ts
router.beforeEach((to, from, next) => {
  if (to.matched.some(r => r.meta.adminOnly)) {
    // check role
  } else {
    next()
  }
})
```
Reading meta in components
```ts
const issueType = this.$route.meta.issueType;
```


# Before / after hooks & guards
## Angular
``` ts
const routes: Routes = [
  { 
    path: 'products/:id', 
    component: ProductDetails,
    canActivate: [CanActivateProductDetails]
  },
];
```

## Aurelia
``` ts
function step() {
  return step.run;
}
step.run = (navigationInstruction: NavigationInstruction, next: Next): Promise<any> {
  return next();
};
config.addPreActivateStep(step);
```

## react-router-dom
Requires some work  
https://reactrouter.com/web/example/auth-workflow


## svelte-router
``` ts
$router.navigationGuard((from, to, next) => {
  // next()         to continue
  // next(false)    to abort
  // next('/path')  to redirect
});
```

## vue-router
Global hooks
``` ts
const router = new VueRouter({ ... })

router.beforeEach((to, from, next) => {
  // next()         to continue
  // next(false)    to abort
  // next('/path')  to redirect
})
router.afterEach((to, from) => {
  // ...
})
```
Per route hooks
``` ts
const router = new VueRouter({
  routes: [
    {
      path: '/path',
      component: SomeComponent,
      beforeEnter: (to, from, next) => {
        // ...
      }
    }
  ]
})
```


# Nested routes
## Angular
``` ts
const routes: Routes = [
  {
    path: 'products/:id',
    component: ProductLayout,
    children: [
      {
        path: '',
        component: ProductDetails
      },
      {
        path: 'reviews',
        component: ProductReviews
      },
    ],
  },
];
```

## react-router-dom
``` xml
<Switch> 
  <Route path="/products/:id">
    <ProductLayout />
  </Route>
  <!---->
</Switch>
```
``` xml
<!-- ProductDetails -->
<Switch>
  <Route exact path="/">
    <ProductDetails />
  </Route>
  <Route path="/reviews">
    <ProductReviews />
  </Route>
</Switch>
```

## svelte-router
``` ts
{ 
  path: '/products/:id(\\d+)',
  children: [
    {
      path: '/',
      component: ProductDetails,
    },
    {
      path: '/reviews',
      component: ProductReviews,
    }
  ],
}
```

## vue-router
``` ts
const router = new VueRouter({
  routes: [
    { 
      path: '/products/:id', 
      component: ProductLayout,
      children: [
        {
          path: '',
          component: ProductDetails
        },
        {
          path: 'reviews',
          component: ProductReviews
        }
      ]
    }
  ]
})
```

# Named placeholders
## Angular
Define outlets in template
``` html
<router-outlet name="sidebar"></router-outlet>
<router-outlet></router-outlet>
```
Define which component goes to which outlet
``` ts
const routes: Routes = [
  {
    path: 'products',
    component: ProductLayout,
    children: [
      {
        path: '',
        component: ProductList  // outlet without name
      },
      {
        path: '',
        component: ProductMenu,
        outlet: 'sidebar'       // outlet named "sidebar"
      },
    ],
  },
];
```

## Aurelia
Define viewports
``` html
<router-view name="sidebar"></router-view>
<router-view name="main"></router-view>
```
Define what component is shown in each viewport
``` ts
{ 
  route: 'products', 
  name: 'products', 
  viewPorts: { 
    sidebar: { moduleId: 'products/menu' }, 
    main: { moduleId: 'products/list' } 
  } 
}
```

## react-router-dom
Define route components
``` ts
const routes = [
  {
    path: "/products",
    sidebar: () => <ProductMenu />,
    main: () => <ProductList />
  }
];
```
Render components
``` jsx
<Switch>
{routes.map(route => (
  <Route
    path={route.path}
    exact={route.exact}
    children={<route.sidebar />}
  />
  <Route
    path={route.path}
    exact={route.exact}
    children={<route.main />}
  />
))}
</Switch>
```
## vue-router
Define views in templates
``` html
<router-view name="sidebar"></router-view>
<router-view></router-view>
```
Define which component goes to which view
``` ts
const router = new VueRouter({
  routes: [
    {
      path: '/products',
      components: {
        default: ProductList, // router-view without name
        sidebar: ProductMenu  // router-view named "sidebar"
      }
    }
  ]
})
```

# Wildcards / no match

## Angular
``` ts
const routes: Routes = [
  // ...
  { path: '**', component: NotFound }
];
```

## Aurelia
``` ts
{ route: '*path', name: 'not-found', moduleId: 'notfound/index' }
```

## react-router-dom
``` xml
<!-- -->
<Route path="*">
  <NotFound />
</Route>
```

## svelte-router
``` ts
routes: [
  // ...
  { path: '*', component: NotFound }
]
```

## vue-router
``` ts
routes: [
  // ...
  { path: '*', component: NotFound }
]
```

# Redirect
## Angular
``` ts
const routes: Routes = [
  { path: 'old',   redirectTo: '/new', pathMatch: 'full' }
];
```

## Aurelia
``` ts
{ route: 'old', redirect: 'new' },
```

## react-router-dom
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

## svelte-router
``` ts
routes: [
  { path: '/old', redirect: '/new' }
]
```

## vue-router
``` ts
const router = new VueRouter({
  routes: [
    { path: '/old', redirect: '/new' }
  ]
})
```