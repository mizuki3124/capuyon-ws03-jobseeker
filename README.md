# JobSeeker

A PHP MVC web application for browsing and managing job listings. Users can register, log in, post job listings, and search for opportunities by keyword and location.

## Features

- Browse all job listings sorted by newest first
- Search listings by keyword (title, description, tags, company) and location
- View full listing details including salary, requirements, and benefits
- Register and log in to a user account
- Create, edit, and delete your own job listings (authenticated users only)
- Authorization — users can only modify listings they own
- Flash messages for success and error feedback
- Input validation and data sanitization

## Tech Stack

- **Backend:** PHP 8 (no framework — custom MVC)
- **Database:** MySQL 8 via PDO
- **Frontend:** HTML, CSS (custom + Tailwind-based stylesheet)
- **Routing:** Custom `Router` class with support for GET, POST, PUT, DELETE
- **Auth:** Session-based authentication with middleware guards (`auth`, `guest`)
- **Autoloading:** Composer PSR-4

## Project Structure

```
ws03/
├── App/
│   ├── controllers/
│   │   ├── HomeController.php
│   │   ├── ListingController.php
│   │   ├── UserController.php
│   │   └── ErrorController.php
│   └── views/
│       ├── listings/        # index, show, create, edit views
│       ├── users/           # register, login views
│       └── partials/        # navbar, footer, head, banners, messages
├── Framework/
│   ├── Router.php           # Custom HTTP router
│   ├── Database.php         # PDO wrapper
│   ├── Session.php          # Session and flash message handling
│   ├── Authorization.php    # Ownership checks
│   ├── Validation.php       # Input validation helpers
│   └── middleware/
│       └── Authorize.php    # Auth/guest middleware
├── config/
│   └── db.php               # Database credentials (git-ignored)
├── public/
│   ├── index.php            # Application entry point
│   ├── .htaccess            # URL rewriting
│   └── css/
├── routes.php               # All application routes
├── helpers.php              # Global helper functions
└── composer.json
```

## Getting Started

### Requirements

- PHP 8.0 or higher
- MySQL 8.0 or higher
- Composer
- A web server with `mod_rewrite` enabled (Apache) or equivalent

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/your-username/ws03.git
   cd ws03
   ```

2. **Install dependencies**
   ```bash
   composer install
   ```

3. **Set up the database**

   Create a MySQL database named `jobseeker`, then import the schema:
   ```bash
   mysql -u root -p jobseeker < jobseeker.sql
   ```

4. **Configure the database connection**

   Create `config/db.php` (this file is git-ignored):
   ```php
   <?php

   return [
       'host'     => 'localhost',
       'port'     => 3306,
       'dbname'   => 'jobseeker',
       'username' => 'root',
       'password' => 'your_password',
       'charset'  => 'utf8mb4'
   ];
   ```

5. **Configure your web server**

   Point the document root to the `public/` directory. The included `public/.htaccess` handles URL rewriting for clean URLs.

   **Apache virtual host example:**
   ```apache
   <VirtualHost *:80>
       DocumentRoot /path/to/ws03/public
       DirectoryIndex index.php
       <Directory /path/to/ws03/public>
           AllowOverride All
       </Directory>
   </VirtualHost>
   ```

6. **Visit the app** at `http://localhost` (or your configured domain).

## Routes

| Method | URI | Description | Middleware |
|--------|-----|-------------|------------|
| GET | `/` | Home page | — |
| GET | `/listings` | All job listings | — |
| GET | `/listings/search` | Search listings | — |
| GET | `/listings/{id}` | View a listing | — |
| GET | `/listings/create` | Create listing form | auth |
| POST | `/listings` | Store new listing | auth |
| GET | `/listings/{id}/edit` | Edit listing form | auth |
| PUT | `/listings/{id}` | Update listing | auth |
| DELETE | `/listings/{id}` | Delete listing | auth |
| GET | `/auth/register` | Register form | guest |
| POST | `/auth/register` | Store new user | guest |
| GET | `/auth/login` | Login form | guest |
| POST | `/auth/login` | Authenticate user | guest |
| POST | `/auth/logout` | Logout | auth |

## Database Schema

**users**
- `id`, `name`, `email`, `password` (bcrypt), `city`, `state`, `created_at`

**listings**
- `id`, `user_id` (FK), `title`, `description`, `salary`, `tags`, `company`, `address`, `city`, `state`, `phone`, `email`, `requirements`, `benefits`, `created_at`

## Author

**Joshua Enrico Capuyon** — [joshuaenricocapuyon@gmail.com](mailto:joshuaenricocapuyon@gmail.com)
