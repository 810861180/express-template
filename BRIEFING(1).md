# Week 6 Alternate formal assessment

(This is a markdown .MD file, if you are reading this in vs-code, right click the file and select `Open Preview`)

#### Rules for this assessment:

See `WEEK_3&6_RULES.md`

#### Start time

START_TIME

#### DEADLINE

DEADLINE -> Thursday (April 22nd) at 22h

#### Collaborators to add

karlaevelize and matiasgarcia91

## Learning goals & some tips

For transparency and clarity, these are the learning goals we will be testing:

#### Frontend

- Basic knowledge of React
  - components
  - props
  - useState
  - useEffect
  - event listeners & handlers
- Making a reducers that transform the redux state
- Using selectors to take state from the redux store and use it in your components
- Dispatching actions from your components to change the redux state
- Separating reducers & actions & selectors
- Using async actions (redux thunk)
- Sending GET / POST / PATCH and DELETE requests using axios
- Setting an authorization header with a JWT to make an authorized request

#### Backend

- Generating models & migrations using sequelize-cli
- Doing database validation using sequelize models (e.g. allowNull: false, unique: true)
- Implementing hasMany, hasOne, belongsTo and belongsToMany relations
- Adding foreign keys to models in migrations
- Adding relations to sequelize models
- Generating seed data using sequelize-cli
- Creating, updating & deleting records from the database using sequelize models
- Querying the database using sequelize models
- Eager loading related models using sequelize `include`
- Implementing GET / POST / PATCH / DELETE routes in express
- Sending responses with express
- Setting status codes to responses in express
- Separating routes using the express Router
- Using the auth middleware to manage authorization for routes in express

**TIP: Read the assignment carefully!** It is easy to accidentally deviate from an assignment, resulting in a frustrating homework experience. Taking the time to read the exercise can save you time and effort.

**TIP: Don't get stuck!** If you feel stuck, try taking a small walk, continuing on to a next step, or talking out loud about the problem you're facing (programmers call this "rubber-ducking"). Everybody can get stuck, but don't let it stop you.

## What are we building?

We are building a webapp for a restaurant that lets diners reserve a table for a specific date: `await Table()`. It also has some admin functionality so restaurants can cancel reservations and block people who make fake reservations. It has multiple pages

- Signup & Login pages (already implemented in the starter kit)
- A page where diners can select a date and make a reservation for a table
- A page where the restaurant owner can see who has made reservations and can cancel reservations
- A page where the restaurant owner can block and unblock user accounts

As a starting point, you must use the following react-redux & express templates where the login / signup flow has already been implemented. Instructions on how to use the templates can be found on the repositories themselves.

[Frontend starter](https://github.com/Codaisseur/react-redux-jwt-bootstrap-template)
[Backend starter](https://github.com/Codaisseur/express-template)

## Wireframe

You will be provided with a wireframe that shows an overview of the app along with this README

## Entities

### Table

| key       | data type | required | notes                           |
| --------- | --------- | -------- | ------------------------------- |
| id        | Integer   | yes      | Already added by model:generate |
| seats     | Integer   | yes      |                                 |
| createdAt | Date      | yes      | Already added by model:generate |
| updatedAt | Date      | yes      | Already added by model:generate |

**Relations:**

- table has many reservation

### Reservation

| key       | data type | required | notes                            |
| --------- | --------- | -------- | -------------------------------- |
| id        | Integer   | yes      | Already added by model:generate  |
| tableId   | Integer   | yes      | Foreign key (references a table) |
| userId    | Integer   | yes      | Foreign key (references a user)  |
| date      | Dateonly  | yes      | DATE without time: eg 2020-06-27 |
| createdAt | Date      | yes      | Already added by model:generate  |
| updatedAt | Date      | yes      | Already added by model:generate  |

**note: do not store the date as a string, store it as the [DATEONLY](https://sequelize.org/master/manual/model-basics.html#dates) sequelize datatype**

**Relations:**

- reservation belongs to table
- reservation belongs to user

### User

| key            | data type | required | notes                              |
| -------------- | --------- | -------- | ---------------------------------- |
| id             | Integer   | yes      | Already implemented                |
| name           | String    | yes      | Already implemented                |
| email          | String    | yes      | Already implemented, unique        |
| password       | String    | yes      | Already implemented, password hash |
| isAdmin        | Boolean   | no       |                                    |
| accountBlocked | Boolean   | no       | defaultValue: false                |
| createdAt      | Date      | yes      | Already implemented                |
| updatedAt      | Date      | yes      | Already implemented                |

- user has many reservation

| Criteria                                                                                   | Points |
| ------------------------------------------------------------------------------------------ | ------ |
| Server contains sequelize models for Table and Reservation                                 | 2      |
| Records aren't allowed to be created if the fields marked as required contain empty values | 2      |
| User, Table and Reservation models are correctly related                                   | 2      |
| Seeders are present to create at least 4 tables and 6 reservations                         | 2      |
| Total                                                                                      | 8      |

## Features

### 1. As a diner I want to see what tables have been reserved for a specific date, so I know if I can go out to dinner

- The main page `/` contains a date input
- By changing the date input, users can see which tables have been reserved for that date
- Reserved table show up as red, available tables as green
- While we want to show data about our reservations we want to respect the privacy of our clients, so emails or names should remain private

| Criteria                                                                                                 | Points |
| -------------------------------------------------------------------------------------------------------- | ------ |
| The frontend route `/` contains an input of type `date`, the default value for the field is always today | 2      |
| Tables of the restaurant are displayed with id, and number of seats                                      | 2      |
| Redux is used to manage the data                                                                         | 2      |
| Reserved tables show up as red, available tables as green for the date currently selected                | 2      |
| Only the data for the currently selected date is fetched, not all reservations                           | 2      |
| The data is fetched form the server, but the user cannot see which tables are reserved                   | -1     |
| The reservation data fetched from the server contains email addresses or names of users                  | -1     |
| Total                                                                                                    | 10     |

**note: [MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/date) some documentation on the input type `date`**
**note: If you only want to fetch reservations for a specific date, consider using a query string read more in the [express.docs](http://expressjs.com/en/api.html#req.query)**

### 2. As a diner I want to make a reservation for a specific table for a specific date, so I can be sure of my spot

- You can make a reservation for a table for a specific date by clicking the "reserve table" button

| Criteria                                                                                             | Points |
| ---------------------------------------------------------------------------------------------------- | ------ |
| On frontend route `/` Logged in users see a button on each unreserved table with `reserve table`     | 1      |
| Users who are not logged in see a link with `login to reserve` which links to `/login`               | 1      |
| Clicking the button creates a reservation for that table by the logged in user for the selected date | 2.5    |
| The request makes use of JWT Authorization                                                           | 1.5    |
| When the reservation is successful a success message is shown to the user                            | 1      |
| A userId is sent in the body of the request, the userId is not determined using the token            | -1     |
| Clicking the reserve table button crashes your app                                                   | -2     |
| The success message is an alert, confirm or prompt popup or console.log                              | -0.5   |
| Total                                                                                                | 7      |

**Note: You can of course use an `Alert` bootstrap component as a message (just not window.alert())**

### 3. As a restaurant owner, I want to be able to login as an admin, so I can administrate my webapp

- If you're logged in as a restaurant owner you see a `View Reservations` & `Manage Users` tab in the navbar
- As the restaurant owner (a user with `isAdmin: true` and who is logged in) be able to go to a separate pages `/admin/reservations` & `/admin/users`

| Criteria                                                                                                                         | Points |
| -------------------------------------------------------------------------------------------------------------------------------- | ------ |
| A separate migration is created to add a columns `isAdmin` and `accountBlocked` to our users table                               | 3      |
| A user with username `ad@min.com` and password `admin` is seeded into the database with `isAdmin: true`                          | 0.5    |
| A user with email `blo@ck.com` and password `block` is seeded into the database with `accountBlocked: true`                      | 0.5    |
| There is are links with `View Reservations` and `Manage Users` in the navbar                                                     | 0.5    |
| A user can only see this link if they are an Admin and these link to the respective pages `/admin/reservations` & `/admin/users` | 1.5    |
| Total                                                                                                                            | 6      |

**Note: You can also add isAdmin and accountBlocked in the original users migration of the template, but you will not get the 3 points**
**Note: There is no way to sign up as an admin, admin users can only be seeded or manually added to the db**

### 4. As a restaurant owner I want to view who has reserved which table for a specific date, so I can contact them

- On the `/admin/reservations` page you see all reservations for today, and all the dates in the future
- For each reservation we see the date, tableId, the name and email of the user who reserved this table
- The reservations sorted by date
- We shouldn't see the reservations from the past
- Only admins can see the personal details of diners

| Criteria                                                                                                                                       | Points |
| ---------------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| Reservations are displayed with date, tableId, email & name of the user who made the reservation                                               | 1      |
| Reservations are sorted by date                                                                                                                | 1      |
| The request makes use of JWT Authorization                                                                                                     | 1      |
| An additional check is made on the server side to see if the user who made the request is an Admin and response with an appropiate status code | 2      |
| An `isAdmin` is sent in the body of the request, the `isAdmin` is not determined using the token                                               | -1     |
| Total                                                                                                                                          | 5      |

**Note: You can of course use an `Alert` bootstrap component as a message (just not window.alert())**

### 5. As a restaurant owner I want to be able to cancel reservations, so I can keep my reservations up to date

- On the `/admin/reservations` page you a `cancel` button next to each reservation
- Clicking the `cancel` button deletes the reservation from the database
- Only admins can do this

| Criteria                                                                                                           | Points |
| ------------------------------------------------------------------------------------------------------------------ | ------ |
| A `cancel` button is present next to each reservation. This button should cancel (delete) the selected reservation | 3      |
| The request makes use of JWT Authorization                                                                         | 0.5    |
| An additional check is made on the server side to see if the user who made the request is an Admin                 | 1      |
| The reservation is removed from the `/admin/reservations` page without manually refreshing (CMD + R / CTRL + R)    | 1.5    |
| Clicking `cancel` crashes the app                                                                                  | -2     |
| An `isAdmin` is sent in the body of the request, the `isAdmin` is not determined using the token                   | -1     |
| Total                                                                                                              | 6      |

### 6. As a restaurant owner I want to be able to block accounts, so I can prevent people from making fake reservations

- On the `/admin/users` page you see the name and emails of all registered users
- Each user has a button `block` next to them
- When clicked, it sets the `blocked` property to `true`
- Only admins can do this
- If users with `accountBlocked: true` try to login, it fails and they see a message that their account has been blocked

| Criteria                                                                                                       | Points |
| -------------------------------------------------------------------------------------------------------------- | ------ |
| On the `/admin/users` page, all users are displayed with their name and email                                  | 1      |
| Next to each user there is `block` or `unblock` button, depending on the status of their account               | 2      |
| These button sends an authorized request to update the status of the selected user account                     | 2      |
| An additional check is made on the server side to see if the user who made the request is an Admin             | 1      |
| Users with their accounts blocked should not be able to login anymore                                          | 2      |
| Users with `accountBlocked: true` see a message in the frontend informing them their account is accountBlocked | 2      |
| Thunk action and backend route are reused for this 2 functionalities, not duplicated                           | 2      |
| Clicking `block` deletes users from the database                                                               | -1     |
| Clicking `block` crashes the app                                                                               | -2     |
| An `isAdmin` is sent in the body of the request, the `isAdmin` is not determined using the token               | -1     |
| Total                                                                                                          | 12     |

**note: You can implement parts of the functionality by seeding blocked users**

### 7. Finishing up

- Self assess:
  - Make a file called `ASSESSMENT.md`
  - Copy the rubric below into it
  - Score your assessment in the column `Self`
  - Leave room for the evaluator to fill in the `Evaluator` column
- Write a reflection about this assessment & your learning process in `REFLECTION.md`:
  - What did you do well, process wise
  - What would you do differently next time to improve, process wise
- Commit your code and use messages when you commit, push it to your repository using `git push origin master`

| Criteria                                                                   | Points |
| -------------------------------------------------------------------------- | ------ |
| Student performed an accurate self assessment (max off by + or - 7 points) | 2      |
| Student can reflect on their process by writing a reflection of ~200 words | 2      |
| Student has regularly committed changes (at least 1 commit per feature)    | 1      |
| Student has written clear commit messages                                  | 1      |
| Total                                                                      | 6      |

### Self assessment

| Section                         | Max Points | Self | Evaluator |
| ------------------------------- | ---------- | ---- | --------- |
| 0 Migrations, models & seeds    | 8          | 0/8  | 0/8       |
| 1 Display reservations          | 10         | 0/10 | 0/10      |
| 2 Making a reservation          | 7          | 0/7  | 0/7       |
| 3 Logging in as admin           | 6          | 0/6  | 0/6       |
| 4 Displaying reservations admin | 5          | 0/5  | 0/5       |
| 5 Cancelling reservations       | 6          | 0/6  | 0/6       |
| 6 Blocking users                | 12         | 0/12 | 0/12      |
| 7 Finishing up                  | 6          | 0/6  | 0/6       |
| Total                           | 60         | 0/60 | 0/60      |

| 0 Migrations, models & seeds - Criteria                                                    | Points | Self | Evaluator |
| ------------------------------------------------------------------------------------------ | ------ | ---- | --------- |
| Server contains sequelize models and migrations for Table and Reservation                  | 2      | 2    |           |
| Records aren't allowed to be created if the fields marked as required contain empty values | 2      | 2    |           |
| User, Table and Reservation models are correctly related                                   | 2      | 2    |           |
| Seeders are present to create at least 4 tables and 6 reservations                         | 2      | 2    |           |
| Total                                                                                      | 8      | 8    |           |

| 1 Display reservations - Criteria                                                                        | Points | Self | Evaluator |
| -------------------------------------------------------------------------------------------------------- | ------ | ---- | --------- |
| The frontend route `/` contains an input of type `date`, the default value for the field is always today | 2      |      |           |
| Tables of the restaurant are displayed with id, and number of seats                                      | 2      |      |           |
| Redux is used to manage the data                                                                         | 2      |      |           |
| Reserved tables show up as red, available tables as green for the date currently selected                | 2      |      |           |
| Only the data for the currently selected date is fetched, not all reservations                           | 2      |      |           |
| The data is fetched form the server, but the user cannot see which tables are reserved                   | -1     |      |           |
| The reservation data fetched from the server contains email addresses or names of users                  | -1     |      |           |
| Total                                                                                                    | 10     |      |           |

| 2 Making a reservation - Criteria                                                                    | Points | Self | Evaluator |
| ---------------------------------------------------------------------------------------------------- | ------ | ---- | --------- |
| On frontend route `/` logged in users see a button on each unreserved table with `reserve table`     | 1      |      |           |
| Users who are not logged in see a link with `login to reserve` which links to `/login`               | 1      |      |           |
| The request makes use of JWT Authorization                                                           | 1.5    |      |           |
| Clicking the button creates a reservation for that table by the logged in user for the selected date | 2.5    |      |           |
| When the reservation is successful a success message is shown to the user                            | 1      |      |           |
| A userId is sent in the body of the request, the userId is not determined using the token            | -1     |      |           |
| Clicking the reserve table button crashes your app                                                   | -2     |      |           |
| The success message is an alert, confirm or prompt popup or console.log                              | -0.5   |      |           |
| Total                                                                                                | 7      |      |           |

| 3 Logging in as admin - Criteria                                                                               | Points | Self | Evaluator |
| -------------------------------------------------------------------------------------------------------------- | ------ | ---- | --------- |
| A separate migration is created to add a columns `isAdmin` and `accountBlocked` to our users table             | 3      |      |           |
| A user with username `ad@min.com` and password `admin` is seeded into the database with `isAdmin: true`        | 0.5    |      |           |
| A user with username `blo@ck.com` and password `block` is seeded into the database with `accountBlocked: true` | 0.5    |      |           |
| There is are links with `View Reservations` and `Manage Users` in the navbar                                   | 0.5    |      |           |
| A user can only see this link if he is an Admin                                                                | 1      |      |           |
| Clicking the links sends the user to `/admin/reservations` & `/admin/users` respectively                       | 0.5    |      |           |
| Total                                                                                                          | 6      |      |           |

| 4 Displaying reservations admin - Criteria                                                                 | Points | Self | Evaluator |
| ---------------------------------------------------------------------------------------------------------- | ------ | ---- | --------- |
| Reservations are displayed with date, tableId, email & name of the user who made the reservation           | 1      |      |           |
| Reservations are sorted by date                                                                            | 1      |      |           |
| An Authorization header is set in the request                                                              | 0.5    |      |           |
| The auth middleware is used on the server side to authorize the request                                    | 0.5    |      |           |
| An additional check is made on the server side to see if the user who made the request has `isAdmin: true` | 1      |      |           |
| Without `isAdmin: true`, the server should refuse & respond with an appropriate status code                | 1      |      |           |
| An `isAdmin` is sent in the body of the request, the `isAdmin` is not determined using the token           | -1     |      |           |
| Total                                                                                                      | 5      |      |           |

| 5 Cancelling reservations - Criteria                                                                               | Points | Self | Evaluator |
| ------------------------------------------------------------------------------------------------------------------ | ------ | ---- | --------- |
| A `cancel` button is present next to each reservation. This button should cancel (delete) the selected reservation | 3      |      |           |
| The request makes use of JWT Authorization                                                                         | 0.5    |      |           |
| An additional check is made on the server side to see if the user who made the request has `isAdmin: true`         | 1      |      |           |
| The reservation is removed from the `/admin/reservations` page without manually refreshing (CMD + R / CTRL + R)    | 1.5    |      |           |
| Clicking `cancel` crashes the app                                                                                  | -2     |      |           |
| An `isAdmin` is sent in the body of the request, the `isAdmin` is not determined using the token                   | -1     |      |           |
| Total                                                                                                              | 6      |      |           |

| 6 Blocking users - Criteria                                                                                    | Points | Self | Evaluator |
| -------------------------------------------------------------------------------------------------------------- | ------ | ---- | --------- |
| On the `/admin/users` page, all users are displayed with their name and email                                  | 1      |      |           |
| Next to each user there is `block` or `unblock` button, depending on the status of their account               | 2      |      |           |
| These button sends an authorized request to update the status of the selected user account                     | 2      |      |           |
| An additional check is made on the server side to see if the user who made the request has is an Admin         | 1      |      |           |
| Users with their accounts blocked should not be able to login anymore                                          | 2      |      |           |
| Users with `accountBlocked: true` see a message in the frontend informing them their account is accountBlocked | 2      |      |           |
| Thunk action and backend route are reused for this 2 functionalities, not duplicated                           | 2      |      |           |
| Clicking `block` deletes users from the database                                                               | -1     |      |           |
| Clicking `block` crashes the app                                                                               | -2     |      |           |
| An `isAdmin` is sent in the body of the request, the `isAdmin` is not determined using the token               | -1     |      |           |
| Total                                                                                                          | 12     |      |           |

| 7 Criteria - Finishing up                                                  | Points | Self | Evaluator |
| -------------------------------------------------------------------------- | ------ | ---- | --------- |
| Student performed an accurate self assessment (max off by + or - 7 points) | 2      |      |           |
| Student can reflect on their process by writing a reflection of ~200 words | 2      |      |           |
| Student has regularly committed changes (at least 1 commit per feature)    | 1      |      |           |
| Student has written clear commit messages                                  | 1      |      |           |
| Total                                                                      | 6      |      |           |
