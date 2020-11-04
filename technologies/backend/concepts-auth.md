# Authentication / Authorization
A1 - Username+password: Users can be authenticated with username and password  
A2 - Social media: Users can authenticate with Facebook, Google and other social media accounts  
A3 - Azure AD: Users can authenticate using Azure AD accounts  
A4 - Roles/groups: Access can be restricted by roles, groups or similar  
A5 - JWT: Authentication and authorization information is handled by Authorization header and JWT bearer tokens  
A6 - Policies: E.g. checking claims from JWT and allowing only users having email address with domain *.example.com to certain endpoints.  
A7 - Conventions: Authorization can be applied to multiple endpoints. E.g. all routes behind /admin/ requires the user to be authenticated or having the admin role.

## Login with username & password

## Login with OAuth
Logging in with 3rd party accounts (e.g. Google or Facebook) is a common scenario.

## JWT

## Roles
- Restrict access based on user roles etc.

Features:
- Basic: handler requires special role
- Advanced: 

