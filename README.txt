# Smart Retail System

A PHP/MySQL retail web application that connects customers and sales associates through an online shopping, cart, checkout, order, inventory, and sales-management workflow.

Repository: [Alpheus-Lebepe/Smart-Retail-System](https://github.com/Alpheus-Lebepe/Smart-Retail-System)

## Overview

The Smart Retail System contains two main experiences:

- A customer storefront for browsing products, managing a cart, checking out, and viewing order history.
- A sales-associate dashboard for monitoring sales, managing products and inventory, restocking products, and updating order statuses.

The application is implemented as server-rendered PHP pages with MySQL persistence, PHP sessions, JavaScript enhancements, and local product imagery.

This README documents the current repository implementation. It is appropriate for local development, demonstration, and academic use. Review the security and production-readiness notes before using it with real customers, payment information, or inventory data.

## Features

### Customer features

- Browse the product catalogue.
- Search products by name.
- Sort products by newest, oldest, price ascending, or price descending.
- View product details and image thumbnails.
- See current product stock availability.
- Add products to a shopping cart as a guest.
- Register for a customer account.
- Log in and log out.
- Persist a logged-in customer's cart in the database.
- Update quantities, remove items, or clear the cart.
- Calculate subtotal, delivery fee, and grand total.
- Choose delivery or pickup.
- Select a payment method: Credit Card, EFT, or Cash.
- Place an order.
- View previous orders and their statuses.

### Sales-associate features

- Register a sales-associate account.
- Log in and log out.
- View sales totals.
- View the number of orders.
- View low-stock, available-stock, and out-of-stock counts.
- Browse inventory by product and category.
- Search inventory by product name.
- Filter inventory by stock status.
- View product details in a modal.
- Add new products.
- Restock products using an asynchronous update request.
- Search orders by order ID or customer.
- Filter orders by status.
- Change order statuses to Pending, Processing, Shipped, Delivered, or Cancelled.
- Record order-status changes in the order activity log.

## Technology Stack

- PHP
- MySQL 8.x
- PDO with the MySQL driver
- HTML5 and CSS3
- Vanilla JavaScript
- PHP sessions for customer and sales-associate state
- Boxicons loaded from the unpkg CDN
- Local product and interface images stored in `Pictures/`

The project does not currently use a PHP framework, Composer, Node package configuration, or a separate REST API.

## Requirements

Install or enable the following:

- PHP 7.4 or later; PHP 8.x is recommended.
- MySQL 8.x or a compatible MySQL server.
- PHP PDO MySQL support.
- A modern web browser.
- Apache, Nginx with PHP-FPM, or PHP's built-in development server.
- Internet access if the CDN-hosted Boxicons stylesheet is required.

Verify that PDO MySQL is enabled:

```bash
php -m | grep -i pdo_mysql
```

Expected output:

```text
pdo_mysql
```

## Installation

### 1. Clone the repository

```bash
git clone https://github.com/Alpheus-Lebepe/Smart-Retail-System.git
cd Smart-Retail-System
```

### 2. Create the database

The application expects a MySQL database named `smart_retail_db`.

Create it with UTF-8 support:

```sql
CREATE DATABASE smart_retail_db
  CHARACTER SET utf8mb4
  COLLATE utf8mb4_unicode_ci;
```

Or from the MySQL command line:

```bash
mysql -u root -p -e "CREATE DATABASE smart_retail_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;"
```

### 3. Import the schema and sample data

The repository includes:

```text
database/smart_retail_db.sql
```

Import it into the selected database:

```bash
mysql -u root -p smart_retail_db < database/smart_retail_db.sql
```

The SQL dump creates and seeds the tables required by the application, including customers, products, carts, orders, order items, payments, sales associates, and order activity logs.

The dump contains demonstration records. Do not use those records as production credentials or customer data.

### 4. Configure the database connection

Edit:

```text
database/dbConnection.php
```

Set the connection values for your local environment:

```php
$host = '127.0.0.1';
$dbname = 'smart_retail_db';
$username = 'root';
$password = 'your-database-password';
```

The current repository contains a database password directly in this PHP file. Rotate that password immediately if the repository is public, remove credentials from source control, and use environment variables or a local ignored configuration file instead.

### 5. Start the application

From the repository root, run:

```bash
php -S localhost:8000
```

Open the main dashboard:

```text
http://localhost:8000/Dashboard.php
```

The PHP built-in server is intended for local development. Use a properly configured Apache or Nginx/PHP-FPM deployment for production.

## Project Structure

```text
Smart-Retail-System/
├── Dashboard.php
├── Documentation.pdf
├── Pictures/
│   ├── Product images and thumbnails
│   ├── Customer.webp
│   ├── sales associate.jpg
│   ├── total sale.jpg
│   ├── orders.png
│   ├── low stock1.jpg
│   ├── in stock.jpg
│   └── out of stock.jpg
├── css/
│   └── style.css
├── database/
│   ├── dbConnection.php
│   └── smart_retail_db.sql
├── js/
│   └── script.js
├── php/
│   ├── Cart.php
│   ├── Checkout.php
│   ├── Customer_Login.php
│   ├── Customer_Logout.php
│   ├── Customer_Shop.php
│   ├── Orders.php
│   ├── Process_Order.php
│   ├── Product_selection.php
│   ├── Sales_Logout.php
│   ├── Sales_Management.php
│   ├── Sales_associate_Login.php
│   ├── update_cart.php
│   ├── update_order.php
│   └── update_stock.php
└── README.txt
```

### Important files

| File | Responsibility |
|---|---|
| `Dashboard.php` | Main entry page and customer/sales-associate role selection |
| `database/dbConnection.php` | Shared PDO connection to MySQL |
| `database/smart_retail_db.sql` | Database schema and sample records |
| `php/Customer_Login.php` | Customer registration and login |
| `php/Customer_Shop.php` | Product catalogue, search, sorting, and add-to-cart entry point |
| `php/Product_selection.php` | Product details, image gallery, stock display, and quantity selection |
| `php/Cart.php` | Cart display, totals, quantity updates, item removal, and clear-cart actions |
| `php/Checkout.php` | Shipping, delivery, payment form, and cart summary |
| `php/Process_Order.php` | Order, order-item, and payment-record creation |
| `php/Orders.php` | Customer order history |
| `php/Sales_associate_Login.php` | Sales-associate registration and login |
| `php/Sales_Management.php` | Sales overview, inventory, product creation, restocking, and order management |
| `php/update_cart.php` | JSON endpoint for asynchronous cart additions and cart counts |
| `php/update_stock.php` | JSON endpoint for inventory restocking |
| `php/update_order.php` | Sales-associate order-status update handler |
| `js/script.js` | Form switching, catalogue filtering/sorting, cart AJAX, alerts, and UI behavior |
| `css/style.css` | Shared application styling |
| `Documentation.pdf` | Additional project documentation included in the repository |

## User Workflows

### Customer workflow

1. Open `Dashboard.php`.
2. Select **Customer** or open the customer shop.
3. Browse or search the catalogue.
4. Select **View** to inspect a product.
5. Choose a quantity and add the product to the cart.
6. Continue shopping or open the cart.
7. Update quantities or remove products if required.
8. Select checkout.
9. Enter delivery, contact, address, and payment-method information.
10. Submit the order.
11. View the confirmation page and later review the order under **Orders**.

### Customer registration and login

Customer registration is handled by `php/Customer_Login.php`.

Required registration data includes:

- Full name between 3 and 100 characters.
- Valid email address.
- Password of at least 5 characters.
- A 10-digit phone number.
- Address between 5 and 500 characters.

Passwords are hashed with `password_hash()` during registration and verified with `password_verify()` during login.

On successful login, the application stores the customer identity in the session and reloads the customer's saved database cart into the session representation.

### Guest cart behavior

A visitor can add products to a session-based cart without registering. A logged-in customer's cart is stored in the `cart_items` table and is restored after login.

Before production use, test the guest checkout path carefully. The current order-processing code uses a customer ID of `0` for guest orders, while the database schema requires `orders.customer_id` to reference an existing customer. A robust production implementation should either require login before checkout or create a dedicated guest-order design.

### Sales-associate workflow

1. Open `Dashboard.php`.
2. Select **Sales Associate**.
3. Register or log in at `php/Sales_associate_Login.php`.
4. After login, open the sales dashboard.
5. Review sales, order, and stock summary cards.
6. Search or filter inventory.
7. Add products using **Add New Product**.
8. Restock products using the **Restock** action.
9. Search or filter recent orders.
10. Change an order status and submit the update.
11. Log out using the sales dashboard navigation.

The dashboard recognizes the following stock categories:

- **Out of Stock:** stock quantity equals 0.
- **Low Stock:** stock quantity is between 1 and 5.
- **Stock Available:** stock quantity is greater than 5.

## Cart and Pricing Rules

The current cart implementation calculates:

```text
Subtotal = sum(product price × quantity)
Shipping = R75 when subtotal is greater than R0 and less than R650
Shipping = R0 when subtotal is R650 or more
Grand Total = Subtotal + Shipping
```

The same shipping rule is used in `Cart.php`, `Checkout.php`, and `Process_Order.php`.

Cart quantities can be updated, removed, or cleared. Logged-in customers use database-backed cart items; guests use the PHP session cart.

## Checkout and Payments

The checkout form collects:

- Delivery or pickup selection
- Full name
- Email
- Phone number
- Address
- Payment method
- Card number, expiry, and CVC fields when Credit Card is selected
- Terms-and-conditions confirmation

The current implementation is **not connected to a real payment gateway**. `Process_Order.php` creates a payment record and marks it `Completed` after the form is submitted. The card fields are not sent to a payment provider, and the application must not be treated as handling real card payments.

Before accepting payments, integrate a PCI-compliant provider using tokenized checkout, never store card numbers or CVC values, and only mark payment records as completed after the provider confirms the transaction.

## Order Processing

When an order is submitted, `Process_Order.php`:

1. Confirms that a cart exists.
2. Reads checkout details.
3. Loads the cart items.
4. Calculates subtotal, shipping, and total amount.
5. Starts a database transaction.
6. Inserts a row into `orders` with status `Pending`.
7. Inserts each item into `order_items`.
8. Inserts a payment record.
9. Commits the transaction.
10. Clears the customer's database cart or guest session cart.
11. Displays an order confirmation message.

The current implementation does not visibly decrement `products.stock_quantity` when an order is placed. Add transactional stock validation and stock deduction before using the application as a real inventory system.

## Sales Management

`php/Sales_Management.php` is protected by the `$_SESSION['associate_id']` session value.

It displays:

- Total completed payment value.
- Total order count.
- Low-stock item count.
- In-stock item count.
- Out-of-stock item count.
- The latest inventory records.
- The latest 20 orders.

Product creation writes to the `products` table. Restocking calls `php/update_stock.php`, which returns JSON containing the new stock quantity. Order status changes are submitted to `php/update_order.php` and are recorded in `order_activity_log`.

## Database Schema

The SQL dump includes the following tables:

| Table | Purpose |
|---|---|
| `customers` | Customer accounts, contact details, and hashed passwords |
| `sales_associate` | Sales-associate accounts and hashed passwords |
| `products` | Product names, descriptions, prices, categories, stock, and dates |
| `cart_items` | Database-backed carts for logged-in customers |
| `orders` | Customer orders, totals, dates, and statuses |
| `order_items` | Products and quantities belonging to each order |
| `payments` | Payment method, amount, status, and transaction reference |
| `order_activity_log` | Associate actions applied to order records |

The `orders.status` field supports:

- `Pending`
- `Processing`
- `Shipped`
- `Delivered`
- `Cancelled`

The database uses foreign keys and indexes for customers, products, carts, orders, payments, and activity logs.

## Session Values

Typical customer session values include:

```php
$_SESSION['customer_id']
$_SESSION['customer_name']
$_SESSION['cart']
$_SESSION['cart_count']
```

Typical sales-associate session values include:

```php
$_SESSION['associate_id']
$_SESSION['associate_name']
```

The initial `Dashboard.php` role selection also stores:

```php
$_SESSION['role']
```

## AJAX and JSON Endpoints

The project is not a REST API, but it contains small request handlers used by the browser.

### `php/update_cart.php`

Adds one product to the current customer's database cart or guest session cart and returns JSON similar to:

```json
{
  "success": true,
  "cart_count": 3
}
```

### `php/update_stock.php`

Accepts a product ID and positive quantity, adds the quantity to the product stock, and returns JSON similar to:

```json
{
  "success": true,
  "new_quantity": 12
}
```

### `php/update_order.php`

Accepts an order ID and one of the allowed order statuses. It updates the order and creates an activity-log entry for the logged-in associate.

These handlers should be treated as internal application endpoints, not public APIs. Add authentication, authorization, CSRF protection, strict validation, and rate limiting before exposing them outside the application.

## Security and Production Notes

The following issues should be addressed before production deployment:

1. **Rotate the exposed database password.** The current `database/dbConnection.php` contains a plaintext database password in a public repository. Treat it as compromised.
2. **Move credentials out of source control.** Use environment variables or a protected configuration file excluded from Git.
3. **Protect `update_stock.php`.** The current handler does not verify that the request comes from a logged-in sales associate, so an unauthorized request could potentially alter stock.
4. **Add CSRF protection.** Add CSRF tokens to registration, login, cart mutation, checkout, product creation, restock, and order-status forms.
5. **Use server-side authorization consistently.** Each management endpoint should verify the appropriate role, not only the page that links to it.
6. **Do not process real card details directly.** Use a PCI-compliant payment provider and tokenized payment flow.
7. **Do not mark payments completed automatically.** Persist the provider result only after verified payment confirmation.
8. **Validate products server-side.** Do not trust product name, price, or quantity values submitted by the browser; reload price and stock from the database.
9. **Deduct and reserve inventory transactionally.** Check available stock and update it in the same transaction as order creation.
10. **Fix guest-order handling.** A guest order currently uses customer ID `0`, which conflicts with the customer foreign key unless a separate design is implemented.
11. **Regenerate sessions after authentication.** Use `session_regenerate_id(true)` after successful customer and associate login.
12. **Set secure session cookies.** Use `Secure`, `HttpOnly`, and appropriate `SameSite` settings over HTTPS.
13. **Avoid verbose production errors.** Do not expose SQL exception messages or database details to end users.
14. **Add rate limiting and login monitoring.** Protect customer and associate login forms against brute-force attempts.
15. **Review uploaded and stored media.** Restrict file types and permissions if product-image uploads are added later.
16. **Back up customer and order data securely.** Do not commit database dumps containing real customer records to a public repository.
17. **Review privacy obligations.** Customer names, email addresses, phone numbers, addresses, order history, and payment metadata require appropriate handling and retention controls.

## Troubleshooting

### Database connection failed

Check that:

- MySQL is running.
- The `smart_retail_db` database exists.
- `database/dbConnection.php` contains valid local credentials.
- PHP has the `pdo_mysql` extension enabled.
- The configured MySQL user can access the database.

### Tables are missing

Re-import the SQL dump:

```bash
mysql -u root -p smart_retail_db < database/smart_retail_db.sql
```

The dump drops and recreates its tables, so use it only when you understand the effect on existing data.

### Images or CSS do not load

Run the PHP server from the repository root and preserve the original directory names and capitalization:

```bash
php -S localhost:8000
```

The PHP pages use relative paths such as `../css/style.css` and `../Pictures/...`.

### Customer login fails

Confirm that:

- The customer account exists in `customers`.
- The password corresponds to the stored password hash.
- The login form is posting to `Customer_Login.php`.
- The database connection points to the expected database.

### Sales-associate login fails

Confirm that:

- The associate account exists in `sales_associate`.
- The password corresponds to the stored password hash.
- The login redirects to `Sales_Management.php`.
- The session is enabled and browser cookies are available.

### Cart count is incorrect

The application maintains cart state in either the session or `cart_items`, depending on whether the customer is logged in. Log in again to reload the database cart, and confirm that JavaScript can reach `update_cart.php`.

### Restocking does not work

Check the browser console and network request for `update_stock.php`. Also verify the product ID and quantity are valid positive integers. Before deployment, add associate authorization to this endpoint.

### Orders do not appear for the customer

`Orders.php` requires `$_SESSION['customer_id']` and only queries orders belonging to that customer. Log in again and confirm that the order transaction completed successfully.

### Order updates fail

Confirm that the sales associate is logged in, the order exists, and the submitted status is one of:

```text
Pending, Processing, Shipped, Delivered, Cancelled
```

## Backup and Restore

Back up the database with:

```bash
mysqldump -u root -p smart_retail_db > smart_retail_db_backup.sql
```

Restore a backup with:

```bash
mysql -u root -p smart_retail_db < smart_retail_db_backup.sql
```

Never publish a backup containing real customer information, order records, addresses, or payment metadata.

## Testing Checklist

Before accepting the application as ready for local demonstration, verify:

- [ ] MySQL is running.
- [ ] The `smart_retail_db` database exists.
- [ ] The SQL dump imports successfully.
- [ ] PDO MySQL is enabled.
- [ ] The database connection works.
- [ ] The main dashboard opens.
- [ ] Customer registration works.
- [ ] Customer login and logout work.
- [ ] Product search and sorting work.
- [ ] Product detail images load.
- [ ] Guest cart operations work.
- [ ] Logged-in cart persistence works.
- [ ] Cart quantity changes and item removal work.
- [ ] Checkout totals are correct.
- [ ] Order creation works with a logged-in customer.
- [ ] Customer order history displays correctly.
- [ ] Sales-associate registration works.
- [ ] Sales-associate login and logout work.
- [ ] Sales summary cards load.
- [ ] Product creation works.
- [ ] Inventory search and filters work.
- [ ] Restocking updates the displayed quantity.
- [ ] Order-status changes work.
- [ ] Order activity log entries are created.
- [ ] Production security changes have been applied before handling real data.

## License

No license is currently specified in this repository.


## Author

# Alpheus Lebepe
