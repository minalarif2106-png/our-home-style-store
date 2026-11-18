# Our Home Style: Online Store (Web + Android App)

A full-stack online store for a fictional furniture and home essentials business. Customers can browse products, search, build a cart and place orders. Staff can manage products and accounts. The same web client is also packaged as an Android app using Apache Cordova.

Built as a two-person university project for **ICT286 (Web and Mobile Computing), Murdoch University**.

## Features

**Customers**
- Browse the product catalogue by category and search by name, SKU or description
- Register, log in and update their profile
- Add items to a cart and submit an order
- Orders are checked against live stock: an order is **confirmed** and stock is reduced, or **rejected** if there isn't enough stock

**Staff**
- Log in with a staff role
- Add, edit and remove products (`staff_product_crud`)
- Create new staff accounts

## Tech stack

| Layer | Technology |
| --- | --- |
| Front end | HTML, CSS, JavaScript, jQuery |
| Back end | PHP (JSON API) |
| Database | MySQL (InnoDB, foreign keys, transactions) |
| Mobile | Apache Cordova (Android) |

## Project structure

```
Server/
  api/        PHP endpoints (auth, search, orders, profile, staff tools)
  lib/        Shared helpers (database connection, sessions, JSON responses)
  db/         schema.sql (tables) and seed.sql (sample data)
  config/     env.example.php (copy to env.php and add your own settings)
WebClient/    Web front end (catalogue, cart, staff pages)
Cordova/      Android app wrapper (our-home-style)
```

## Database design

Five tables: `users` (roles: visitor, customer, staff), `categories`, `products`, `orders` and `order_items`. Order items are linked to orders and products with foreign keys. Order submission runs inside a transaction and locks the product rows it reads (`SELECT ... FOR UPDATE`) so stock can't be oversold.

## Running it locally

**1. Database**
1. Create a MySQL database.
2. Run `Server/db/schema.sql`, then `Server/db/seed.sql`.

**2. Server**
1. Copy `Server/config/env.example.php` to `Server/config/env.php` and enter your MySQL details.
2. Serve the `Server` folder with PHP and Apache (for example XAMPP or MAMP).

**3. Web client**
1. In `WebClient/js/api-config.js`, set `API_BASE` to the URL of your `Server/api/` folder.
2. Open `WebClient/home.html` through your web server.

**4. Android app (optional)**
```bash
cd Cordova/our-home-style
npm install
cordova platform add android
cordova run android
```

## Demo accounts

`seed.sql` creates sample customer and staff accounts for testing. These are demo credentials for local use only.

## Screenshots

_Add screenshots here: home page, catalogue, cart, staff page, Android app._

## What I would improve

- Use prepared statements for every query. Search already does, but some order and stock queries still insert values directly into the SQL.
- Store passwords with `password_hash()` instead of SHA-256.
- Add a real payment step. This project handles orders and stock but not payment.
- Add input validation and automated tests for the API.

## Author

Muhammad Minal Arif: Bachelor of Information Technology (AI and Autonomous Systems), Murdoch University.
