# Meal diary

## Data
```
user
* id
...

food
* id
* name

nutrient
* id
* name
* unit

food_nutrient
* food_id
* nutrient_id
* amount

portion
* food_id
* name
* weight

meal
* id
* user_id
* time

meal_row
* meal_id
* food_id
* portion_id
* amount
* weight

mealplan
* id
* name

mealplan_meal
* id
* plan_id
* name

mealplan_goal
* plan_id
* nutrient_id
* min
* max

mealplan_meal_goal
* mealplan_meal_id
* nutrient_id
* min
* max
```

## API

```
POST    /users/register                       Register
POST    /users/login                          Login
PUT     /users/me                             Update logged in user details
GET     /users/me                             Get logged in user details

GET     /nutrients/                           Get nutrients

GET     /foods/?q=<query>                     Search foods
GET     /foods/<id>                           Get food by id
POST    /foods/                               Create food
PUT     /foods/<id>                           Update food
DELETE  /foods/<id>                           Delete food

GET     /meals/?start=<start>&end=<end>       Get meals within dates
POST    /meals/                               Create meal
POST    /meals/<id>/rows                      Add meal row
PUT     /meals/<id>/rows/<row_id>             Update meal row
DELETE  /meals/<id>/rows/<row_id>             Delete meal row
```

## Features
```
- User can create account with username & password
- User can login with username & password
- User can login with Facebook / Google account
- User can update personal settings
  - Visible nutrients
  - Min/max daily values for nutrients
- User can create own foods
- User can update own foods
- User can delete own foods
- User can create meals
- User can add rows to meals
- User can update meal rows
- User can delete meal rows
- User can delete meals
- User can see daily summary
  - Total nutrients / day
  - Total nutrients / meal
- User can see long term summary
```