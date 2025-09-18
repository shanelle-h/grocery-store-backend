# 🛒 Grocery Store Backend - FSD

[![Node.js](https://img.shields.io/badge/Node.js-18.x-green.svg)](https://nodejs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.x-blue.svg)](https://www.typescriptlang.org/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15.x-blue.svg)](https://www.postgresql.org/)
[![Express](https://img.shields.io/badge/Express-4.x-lightgrey.svg)](https://expressjs.com/)
[![Docker](https://img.shields.io/badge/Docker-supported-blue.svg)](https://www.docker.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

> A comprehensive RESTful API for managing a grocery store system with customer authentication, product management, order processing, and administrative capabilities.

## Table of Contents

- [Features](#features)
- [Tech Stack](#tech-stack)
- [Prerequisites](#prerequisites)
- [Installation & Setup](#installation--setup)
- [Configuration](#configuration)
- [Database Setup](#database-setup)
- [Running the Application](#running-the-application)
- [Testing](#testing)
- [API Documentation](#api-documentation)
- [Database Schema](#database-schema)
- [Project Structure](#project-structure)
- [Contributing](#contributing)

## ✨ Features

### 🔐 Core Functionality
- **Authentication & Authorization**: JWT-based auth with role-based access control
- **Category Management**: Hierarchical category tree structure
- **Product Management**: Full CRUD operations with bulk upload support
- **Customer Management**: Profile management and admin oversight
- **Order Management**: Complete order processing system
- **Notifications**: Mock SMS/email notifications for orders

### 🛡️ Security Features
- Password hashing with bcrypt
- JWT token authentication
- Input validation and sanitization
- Role-based authorization (Customer/Admin)

## 🚀 Tech Stack

| Category | Technology |
|----------|------------|
| **Runtime** | Node.js with TypeScript |
| **Framework** | Express.js |
| **Database** | PostgreSQL |
| **Query Builder** | Knex.js |
| **Authentication** | JWT |
| **Testing** | Jest |
| **Containerization** | Docker |
| **Validation** | Joi/express-validator |

## 📋 Prerequisites

- **Node.js** (v16 or higher)
- **PostgreSQL** (v12 or higher)
- **Docker & Docker Compose** (optional)
- **npm** or **yarn**

## 🔧 Installation & Setup


## Configuration


## Database Setup


## Running the Application



## Testing



## API Documentation

### Base URL
```
http://localhost:3000/api/v1
```

### Authentication
All authenticated endpoints require a JWT token in the Authorization header:
```
Authorization: Bearer <jwt_token>
```

---

### 1. Authentication & Authorization

### Register Customer
**POST** `/auth/register`

Register a new customer account.

**Request Body:**
```json
{
  "name": "John Doe",
  "email": "john.doe@example.com",
  "password": "securePassword123",
  "phone": "+1234567890"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Customer registered successfully",
  "data": {
    "id": 1,
    "name": "John Doe",
    "email": "john.doe@example.com",
    "phone": "+1234567890",
    "role": "customer",
    "created_at": "2025-01-15T10:30:00Z"
  },
 "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

### Login
**POST** `/auth/login`

Authenticate user and receive JWT token.

**Request Body:**
```json
{
  "email": "john.doe@example.com",
  "password": "securePassword123"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Login successful",
  "data": {
    "user": {
      "id": 1,
      "name": "John Doe",
      "email": "john.doe@example.com",
      "role": "customer"
    },
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }
}
```

---

## 2. Category Management

### Create Category
**POST** `/categories` 🔒 *Admin Only*

Create a new product category.

**Request Body:**
```json
{
  "name": "Fresh Produce",
  "description": "Fresh fruits and vegetables",
  "parent_id": null
}
```

**Response:**
```json
{
  "success": true,
  "message": "Category created successfully",
  "data": {
    "id": 1,
    "name": "Fresh Produce",
    "description": "Fresh fruits and vegetables",
    "parent_id": null,
    "is_active": true,
    "created_at": "2025-01-15T10:30:00Z"
  }
}
```

### Get All Categories
**GET** `/categories`

Retrieve all categories with optional hierarchy.

**Query Parameters:**
- `tree` (optional): Set to `true` to get hierarchical tree structure

**Response (Flat):**
```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "name": "Fresh Produce",
      "description": "Fresh fruits and vegetables",
      "parent_id": null,
      "is_active": true
    },
    {
      "id": 2,
      "name": "Fruits",
      "description": "Fresh fruits",
      "parent_id": 1,
      "is_active": true
    }
  ]
}
```

**Response (Tree Structure):**
```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "name": "Fresh Produce",
      "description": "Fresh fruits and vegetables",
      "parent_id": null,
      "is_active": true,
      "children": [
        {
          "id": 2,
          "name": "Fruits",
          "description": "Fresh fruits",
          "parent_id": 1,
          "is_active": true,
          "children": []
        }
      ]
    }
  ]
}
```

### Get Category by ID
**GET** `/categories/:id`

**Response:**
```json
{
  "success": true,
  "data": {
    "id": 1,
    "name": "Fresh Produce",
    "description": "Fresh fruits and vegetables",
    "parent_id": null,
    "is_active": true
  }
}
```

### Update Category
**PUT** `/categories/:id` 🔒 *Admin Only*

**Request Body:**
```json
{
  "name": "Updated Fresh Produce",
  "description": "Updated description",
  "is_active": true
}
```

### Delete Category
**DELETE** `/categories/:id` 🔒 *Admin Only*

Soft delete a category (sets is_active to false).

**Response:**
```json
{
  "success": true,
  "message": "Category deleted successfully"
}
```

---

## 3. Product Management

### Create Product
**POST** `/products` 🔒 *Admin Only*

**Request Body:**
```json
{
  "name": "Red Apples",
  "sku": "APPLE-RED-001",
  "description": "Fresh red apples from local farms",
  "price": 299,
  "currency": "USD",
  "category_id": 2,
  "stock_quantity": 100
}
```

**Response:**
```json
{
  "success": true,
  "message": "Product created successfully",
  "data": {
    "id": 1,
    "name": "Red Apples",
    "sku": "APPLE-RED-001",
    "description": "Fresh red apples from local farms",
    "price": 299,
    "currency": "USD",
    "category_id": 2,
    "stock_quantity": 100,
    "is_active": true,
    "created_at": "2025-01-15T10:30:00Z"
  }
}
```

### Bulk Upload Products
**POST** `/products/bulk` 🔒 *Admin Only*

Upload multiple products with their categories.

**Request Body:**
```json
{
  "products": [
    {
      "name": "Green Apples",
      "sku": "APPLE-GREEN-001",
      "description": "Fresh green apples",
      "price": 329,
      "currency": "USD",
      "category_name": "Fruits",
      "stock_quantity": 50
    },
    {
      "name": "Bananas",
      "sku": "BANANA-001",
      "description": "Ripe bananas",
      "price": 199,
      "currency": "USD",
      "category_name": "Fruits",
      "stock_quantity": 75
    }
  ]
}
```

**Response:**
```json
{
  "success": true,
  "message": "Products uploaded successfully",
  "data": {
    "created": 2,
    "failed": 0,
    "errors": []
  }
}
```

### Get All Products
**GET** `/products`

**Query Parameters:**
- `category_id` (optional): Filter by category
- `page` (optional, default: 1): Page number
- `limit` (optional, default: 20): Items per page
- `search` (optional): Search in name and description

**Response:**
```json
{
  "success": true,
  "data": {
    "products": [
      {
        "id": 1,
        "name": "Red Apples",
        "sku": "APPLE-RED-001",
        "description": "Fresh red apples from local farms",
        "price": 299,
        "currency": "USD",
        "category_id": 2,
        "category_name": "Fruits",
        "stock_quantity": 100,
        "is_active": true
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 20,
      "total": 1,
      "pages": 1
    }
  }
}
```

### Get Product by ID
**GET** `/products/:id`

### Update Product
**PUT** `/products/:id` 🔒 *Admin Only*

### Delete Product
**DELETE** `/products/:id` 🔒 *Admin Only*

### Get Average Price by Category
**GET** `/categories/:id/average-price`

Get average product price for a category including subcategories.

**Response:**
```json
{
  "success": true,
  "data": {
    "category_id": 1,
    "category_name": "Fresh Produce",
    "average_price": 264,
    "currency": "USD",
    "product_count": 5,
    "includes_subcategories": true
  }
}
```

---

## 4. Customer Management

### Get Customer Profile
**GET** `/customers/profile` 🔒 *Customer*

Get authenticated customer's profile.

**Response:**
```json
{
  "success": true,
  "data": {
    "id": 1,
    "name": "John Doe",
    "email": "john.doe@example.com",
    "phone": "+1234567890",
    "is_active": true,
    "created_at": "2025-01-15T10:30:00Z"
  }
}
```

### Update Customer Profile
**PUT** `/customers/profile` 🔒 *Customer*

**Request Body:**
```json
{
  "name": "John Updated Doe",
  "phone": "+1987654321"
}
```

### Get All Customers
**GET** `/customers` 🔒 *Admin Only*

**Query Parameters:**
- `page` (optional, default: 1)
- `limit` (optional, default: 20)
- `search` (optional): Search by name or email

**Response:**
```json
{
  "success": true,
  "data": {
    "customers": [
      {
        "id": 1,
        "name": "John Doe",
        "email": "john.doe@example.com",
        "phone": "+1234567890",
        "is_active": true,
        "created_at": "2025-01-15T10:30:00Z"
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 20,
      "total": 1,
      "pages": 1
    }
  }
}
```

---

## 5. Orders

### Create Order
**POST** `/orders` 🔒 *Customer*

**Request Body:**
```json
{
  "delivery_address": "123 Main St, City, State 12345",
  "notes": "Please deliver in the evening",
  "items": [
    {
      "product_id": 1,
      "quantity": 3
    },
    {
      "product_id": 2,
      "quantity": 2
    }
  ]
}
```

**Response:**
```json
{
  "success": true,
  "message": "Order created successfully",
  "data": {
    "id": 1,
    "user_id": 1,
    "total_price": 1295,
    "currency": "USD",
    "status": "pending",
    "delivery_address": "123 Main St, City, State 12345",
    "notes": "Please deliver in the evening",
    "created_at": "2025-01-15T10:30:00Z",
    "items": [
      {
        "id": 1,
        "product_id": 1,
        "product_name": "Red Apples",
        "quantity": 3,
        "unit_price": 299,
        "total_price": 897
      },
      {
        "id": 2,
        "product_id": 2,
        "product_name": "Bananas",
        "quantity": 2,
        "unit_price": 199,
        "total_price": 398
      }
    ]
  }
}
```

### Get Customer Orders
**GET** `/orders` 🔒 *Customer*

Get authenticated customer's orders.

**Query Parameters:**
- `page` (optional, default: 1)
- `limit` (optional, default: 10)
- `status` (optional): Filter by order status

**Response:**
```json
{
  "success": true,
  "data": {
    "orders": [
      {
        "id": 1,
        "total_price": 1295,
        "currency": "USD",
        "status": "pending",
        "delivery_address": "123 Main St, City, State 12345",
        "created_at": "2025-01-15T10:30:00Z",
        "items_count": 2
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 10,
      "total": 1,
      "pages": 1
    }
  }
}
```

### Get Order Details
**GET** `/orders/:id` 🔒 *Customer/Admin*

Customers can only view their own orders.

**Response:**
```json
{
  "success": true,
  "data": {
    "id": 1,
    "user_id": 1,
    "customer_name": "John Doe",
    "total_price": 1295,
    "currency": "USD",
    "status": "pending",
    "delivery_address": "123 Main St, City, State 12345",
    "notes": "Please deliver in the evening",
    "created_at": "2025-01-15T10:30:00Z",
    "items": [
      {
        "id": 1,
        "product_id": 1,
        "product_name": "Red Apples",
        "sku": "APPLE-RED-001",
        "quantity": 3,
        "unit_price": 299,
        "total_price": 897
      }
    ]
  }
}
```

### Get All Orders (Admin)
**GET** `/admin/orders` 🔒 *Admin Only*

**Query Parameters:**
- `page` (optional, default: 1)
- `limit` (optional, default: 20)
- `status` (optional): Filter by status
- `customer_id` (optional): Filter by customer

---

## 6. Notifications

### Get Customer Notifications
**GET** `/notifications` 🔒 *Customer*

Get authenticated customer's notifications.

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "type": "sms",
      "recipient": "+1234567890",
      "message": "Your order #1 has been confirmed. Total: $12.95",
      "status": "sent",
      "created_at": "2025-01-15T10:30:00Z",
      "sent_at": "2025-01-15T10:31:00Z"
    },
    {
      "id": 2,
      "type": "email",
      "recipient": "admin@grocerystore.com",
      "message": "New order #1 received from John Doe",
      "status": "sent",
      "created_at": "2025-01-15T10:30:00Z",
      "sent_at": "2025-01-15T10:31:00Z"
    }
  ]
}
```

### Mark Notification as Read
**PUT** `/notifications/:id/read` 🔒 *Customer*

**Response:**
```json
{
  "success": true,
  "message": "Notification marked as read"
}
```

---

## Error Responses

All error responses follow this format:

```json
{
  "success": false,
  "message": "Error description",
  "errors": [
    {
      "field": "email",
      "message": "Email is required"
    }
  ]
}
```

### HTTP Status Codes
- `200` - Success
- `201` - Created
- `400` - Bad Request (validation errors)
- `401` - Unauthorized (invalid or missing token)
- `403` - Forbidden (insufficient permissions)
- `404` - Not Found
- `409` - Conflict (duplicate data)
- `500` - Internal Server Error

---

## Request/Response Headers

### Common Request Headers
```
Content-Type: application/json
Authorization: Bearer <jwt_token>
```

### Common Response Headers
```
Content-Type: application/json
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 99
X-RateLimit-Reset: 1642678800
```

---

## Testing with Postman

Import the Postman collection: `docs/grocery-store-api.postman_collection.json`

### Environment Variables
Set up these variables in your Postman environment:
- `base_url`: `http://localhost:3000/api/v1`
- `auth_token`: JWT token from login response (auto-populated)
- `customer_id`: Customer ID (auto-populated)
- `admin_token`: Admin JWT token

### Testing Flow
1. Register a new customer
2. Login to get JWT token
3. Create categories (as admin)
4. Create products (as admin)
5. Place an order (as customer)
6. View notifications

---

## Rate Limiting
- **General endpoints**: 100 requests per hour per IP
- **Auth endpoints**: 10 requests per hour per IP
- **Bulk operations**: 5 requests per hour per authenticated user

## 🗃️ Database Schema

### Entity Relationship Diagram

![ERD Diagram](ERD.png)

### Key Tables


### :adult: `USERS`
Stores user information including customers and administrators.

| Column | Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | `INT` | `PRIMARY KEY`<br>`AUTO_INCREMENT` | Unique user identifier |
| `name` | `VARCHAR(255)` | `NOT NULL` | User's full name |
| `email` | `VARCHAR(255)` | `UNIQUE`<br>`NOT NULL` | User's email address |
| `password` | `VARCHAR(255)` | `NOT NULL` | Hashed password |
| `phone` | `VARCHAR(20)` | `NULL` | Contact phone number |
| `role` | `ENUM('customer', 'admin')` | | User role |
| `is_active` | `BOOLEAN` | `DEFAULT TRUE` | Account status |
| `created_at` | `TIMESTAMP` | `AUTO_GENERATED` | Account creation time |
| `updated_at` | `TIMESTAMP` | `AUTO_UPDATED` | Last update time |

### :file_folder: `CATEGORIES`
Hierarchical product categories supporting a tree structure.

| Column | Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | `INT` | `PRIMARY KEY`<br>`AUTO_INCREMENT` | Unique category identifier |
| `name` | `VARCHAR(255)` | `NOT NULL` | Category name |
| `parent_id` | `INT` | `FOREIGN KEY`<br>`NULL` | Parent category reference |
| `description` | `TEXT` | `NULL` | Category description |
| `is_active` | `BOOLEAN` | `DEFAULT TRUE` | Category status |
| `created_at` | `TIMESTAMP` | `AUTO_GENERATED` | Creation time |
| `updated_at` | `TIMESTAMP` | `AUTO_UPDATED` | Last update time |

### :package: `PRODUCTS`
Product catalog with pricing and categorization.

| Column | Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | `INT` | `PRIMARY KEY`<br>`AUTO_INCREMENT` | Unique product identifier |
| `name` | `VARCHAR(255)` | `NOT NULL` | Product name |
| `sku` | `VARCHAR(100)` | `UNIQUE`<br>`NOT NULL` | Stock keeping unit |
| `description` | `TEXT` | `NULL` | Product description |
| `price` | `INT` | `NOT NULL` | Price in cents |
| `currency` | `VARCHAR(3)` | `DEFAULT 'USD'` | Currency code |
| `category_id` | `INT` | `FOREIGN KEY` | Category reference |
| `is_active` | `BOOLEAN` | `DEFAULT TRUE` | Product availability |
| `stock_quantity` | `INT` | `DEFAULT 0` | Available stock |
| `created_at` | `TIMESTAMP` | `AUTO_GENERATED` | Creation time |
| `updated_at` | `TIMESTAMP` | `AUTO_UPDATED` | Last update time |

### :shopping_trolley: `ORDERS`
Customer orders with status tracking.

| Column | Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | `INT` | `PRIMARY KEY`<br>`AUTO_INCREMENT` | Unique order identifier |
| `user_id` | `INT` | `FOREIGN KEY`<br>`NOT NULL` | Customer reference |
| `total_price` | `INT` | `NOT NULL` | Total amount in cents |
| `currency` | `VARCHAR(3)` | `DEFAULT 'USD'` | Currency code |
| `status` | `VARCHAR(20)` | `DEFAULT 'pending'` | Order status |
| `delivery_address` | `TEXT` | `NULL` | Delivery address |
| `notes` | `TEXT` | `NULL` | Order notes |
| `created_at` | `TIMESTAMP` | `AUTO_GENERATED` | Order creation time |
| `updated_at` | `TIMESTAMP` | `AUTO_UPDATED` | Last update time |

### :package: `ORDER_ITEMS`
Individual items within orders.

| Column | Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | `INT` | `PRIMARY KEY`<br>`AUTO_INCREMENT` | Unique item identifier |
| `order_id` | `INT` | `FOREIGN KEY`<br>`NOT NULL` | Order reference |
| `product_id` | `INT` | `FOREIGN KEY`<br>`NOT NULL` | Product reference |
| `quantity` | `INT` | `NOT NULL` | Item quantity |
| `unit_price` | `INT` | `NOT NULL` | Unit price in cents |
| `total_price` | `INT` | `NOT NULL` | Line total in cents |
| `created_at` | `TIMESTAMP` | `AUTO_GENERATED` | Creation time |

### :bell: `NOTIFICATIONS`
System notifications for users and orders.

| Column | Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | `INT` | `PRIMARY KEY`<br>`AUTO_INCREMENT` | Unique notification ID |
| `user_id` | `INT` | `FOREIGN KEY`<br>`NOT NULL` | User reference |
| `order_id` | `INT` | `FOREIGN KEY`<br>`NULL` | Order reference |
| `type` | `VARCHAR(10)` | `ENUM('sms', 'email')` | Notification type |
| `recipient` | `VARCHAR(255)` | `NOT NULL` | Phone or email address |
| `message` | `TEXT` | `NOT NULL` | Notification content |
| `status` | `VARCHAR(20)` | `DEFAULT 'pending'` | Delivery status |
| `created_at` | `TIMESTAMP` | `AUTO_GENERATED` | Creation time |
| `sent_at` | `TIMESTAMP` | `NULL` | Delivery time |

---

### :link: Relationships
- **Users** :left_right_arrow: **Orders**: One-to-many relationship – a user can have multiple orders.
- **Users** :left_right_arrow: **Notifications**: One-to-many relationship for user notifications.
- **Categories** :left_right_arrow: **Categories**: Self-referencing for hierarchical structure.
- **Categories** :left_right_arrow: **Products**: One-to-many relationship for product categorization.
- **Products** :left_right_arrow: **Order Items**: One-to-many relationship for order line items.
- **Orders** :left_right_arrow: **Order Items**: One-to-many relationship for order composition.
- **Orders** :left_right_arrow: **Notifications**: One-to-many relationship for order-related notifications.

### :jigsaw: Key Features
- **User Management**: Supports both customers and administrators.
- **Hierarchical Categories**: Tree structure for product organization.
- **Inventory Tracking**: Stock quantity management.
- **Order Processing**: Complete lifecycle with status tracking.
- **Notification System**: Multi-channel (SMS/Email) notifications.
- **Audit Trail**: Timestamps for creation and updates.
- **Soft Deletes**: Logical deletion using `is_active` flags.
- **Currency Support**: Multi-currency pricing enabled.





## Project Structure


## Environment Variables Reference


## Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/amazing-feature`
3. Make your changes and add tests
4. Ensure tests pass: `npm test`
5. Commit your changes: `git commit -m 'Add amazing feature'`
6. Push to the branch: `git push origin feature/amazing-feature`
7. Open a Pull Request

### Code Standards
- Use TypeScript for all new code
- Follow ESLint configuration
- Maintain test coverage above 80%
- Write meaningful commit messages
- Update documentation for API changes

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Support

For questions or issues, please:
1. Check existing GitHub issues
2. Create a new issue with detailed description
3. Include steps to reproduce any bugs

---

**Happy coding! 🛒**
