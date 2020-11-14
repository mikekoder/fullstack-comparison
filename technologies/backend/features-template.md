# Requirements

# Creating project
- CLI
- GIT template
- GUI

# Tools
- IDE / plugin

# Concepts

## Routing

### Basic
- method
- pattern
- parameters
- constraints
- fallback

```
GET     /products/     -> get list of products
GET     /products/{id} -> get single product
POST    /products/     -> create product
PUT     /products/{id} -> update product
DELETE  /products/{id} -> delete product
```

### Advanced
- conventions
- prefix
- subdomain

- versioning

## Middleware / Pipeline
- before
- after
- global
- per route
- convention



## Model binding
- path
- query
- body
- headers

## Authentication
- Username/password
- OAuth
- global/per route/convention
- anonymous

## Authorization
- Roles
- Claims
- Policies
- global/per route/convention

## Databases
- Relational
- Document

## Content negotiation
- json
- xml
- ...

## Validation
- global/automatic
- convention based

## Exception handling
- global/automatic

## Logging
- global/automatic

## Static files
- SPA

## Templates

## Caching

## OpenAPI
- automatic

## GraphQL

## Broadcasting
- WebSocket
- SSE
- Long polling

## Background workers

## Testing

# Deployment
- Cloud web service (non Docker)
- Docker