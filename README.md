# Movies Store

Movies Store is a full-stack web application built with Django that simulates an online movie marketplace. Users can browse a catalog of films, add titles to a shopping cart, complete purchases, manage their order history, and interact with a community review system. The application is structured as a modular Django project with separate apps for home pages, movie catalog management, user accounts, and cart functionality.

This project was developed as part of **CS 2340: Objects and Design** at the Georgia Institute of Technology.

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Technology Stack](#technology-stack)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Running the Application](#running-the-application)
- [Database Configuration](#database-configuration)
- [Administration Panel](#administration-panel)
- [Application Architecture](#application-architecture)
- [Data Models](#data-models)
- [URL Reference](#url-reference)
- [Static and Media Files](#static-and-media-files)
- [Authentication and Authorization](#authentication-and-authorization)
- [Shopping Cart Implementation](#shopping-cart-implementation)
- [Review and Reporting System](#review-and-reporting-system)
- [Testing](#testing)
- [Deployment Considerations](#deployment-considerations)
- [Author](#author)
- [License](#license)

---

## Overview

Movies Store provides a complete e-commerce experience for digital movie purchases. The application follows the Model-View-Template (MVT) architectural pattern used by Django and separates concerns across four distinct applications:

| Application | Responsibility |
|-------------|----------------|
| `home` | Landing page and informational content |
| `movies` | Movie catalog, detail pages, reviews, and reporting |
| `accounts` | User registration, login, logout, and order history |
| `cart` | Session-based shopping cart and checkout flow |

The front end is rendered using Django templates with Bootstrap 5 for responsive layout and styling, supplemented by custom CSS and Font Awesome icons.

---

## Features

### Public Features
- Browse a searchable catalog of movies with thumbnail images
- View detailed movie pages including description, price, and image
- Read user-submitted reviews on movie detail pages
- Access informational About page

### Authenticated User Features
- Create an account and log in securely
- Add movies to a session-based shopping cart with configurable quantity
- View, clear, and purchase cart contents
- Receive purchase confirmation with order ID
- View complete order history with itemized breakdowns
- Submit reviews on movie detail pages
- Edit and delete personal reviews
- Report inappropriate reviews submitted by other users

### Administrative Features
- Django admin interface for managing movies, reviews, reports, orders, and order items
- Searchable movie administration with name-based ordering

---

## Technology Stack

| Layer | Technology |
|-------|------------|
| Backend Framework | Django 5.0 |
| Language | Python 3 |
| Database | SQLite (default), MySQL (optional) |
| Frontend | HTML, CSS, Bootstrap 5.3.3 |
| Icons | Font Awesome 6.1.1 |
| Typography | Google Fonts (Poppins) |
| Authentication | Django built-in auth system |
| Session Management | Django sessions (cart storage) |
| Media Handling | Django ImageField with local file storage |

---

## Project Structure

```
moviesstore/
├── manage.py                     # Django management script
├── db.sqlite3                    # SQLite database (generated after migrate)
├── media/                        # User-uploaded files (movie images)
├── moviesstore/                  # Project configuration package
│   ├── settings.py               # Application settings
│   ├── urls.py                   # Root URL routing
│   ├── wsgi.py                   # WSGI entry point
│   ├── asgi.py                   # ASGI entry point
│   ├── static/
│   │   └── css/
│   │       └── style.css         # Custom stylesheet
│   └── templates/
│       └── base.html             # Shared layout template
├── home/                         # Home and About pages
│   ├── views.py
│   ├── urls.py
│   └── templates/home/
│       ├── index.html
│       └── about.html
├── movies/                       # Movie catalog and reviews
│   ├── models.py                 # Movie, Review, Report models
│   ├── views.py
│   ├── urls.py
│   ├── admin.py
│   ├── migrations/
│   └── templates/movies/
│       ├── index.html
│       ├── show.html
│       ├── edit_review.html
│       └── report_review.html
├── accounts/                     # User authentication
│   ├── views.py
│   ├── urls.py
│   ├── forms.py                  # Custom signup form
│   └── templates/accounts/
│       ├── login.html
│       ├── signup.html
│       └── orders.html
└── cart/                         # Shopping cart and checkout
    ├── models.py                 # Order, Item models
    ├── views.py
    ├── urls.py
    ├── utils.py                  # Cart total calculation
    ├── templatetags/
    │   └── cart_filters.py       # Custom template filters
    └── templates/cart/
        ├── index.html
        └── purchase.html
```

---

## Prerequisites

Before setting up the project, ensure the following are installed on your system:

- **Python 3.10+** (recommended for Django 5.0 compatibility)
- **pip** (Python package manager)
- **Git** (optional, for version control)

Optional (only if using MySQL instead of SQLite):

- **MySQL Server**
- **mysqlclient** Python package

---

## Installation

### 1. Clone the Repository

```bash
git clone <repository-url>
cd moviesstore
```

### 2. Create and Activate a Virtual Environment

**Windows (PowerShell):**

```powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
```

**macOS / Linux:**

```bash
python3 -m venv venv
source venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install django
```

For MySQL support (optional):

```bash
pip install mysqlclient
```

For image upload support, Pillow is required:

```bash
pip install Pillow
```

### 4. Apply Database Migrations

```bash
python manage.py makemigrations
python manage.py migrate
```

### 5. Create a Superuser (Admin Account)

```bash
python manage.py createsuperuser
```

Follow the prompts to set a username, email, and password for the Django admin panel.

### 6. Add Static Assets

Place the following image files in `moviesstore/static/img/`:

| File | Purpose |
|------|---------|
| `logo.png` | Navigation bar logo |
| `background.jpg` | Home page hero background |
| `about.jpg` | About page illustration |

These assets are referenced in templates and CSS but are not included in the repository by default.

---

## Running the Application

Start the development server:

```bash
python manage.py runserver
```

The application will be available at:

```
http://127.0.0.1:8000/
```

To run on a different port:

```bash
python manage.py runserver 8080
```

---

## Database Configuration

### SQLite (Default)

The project uses SQLite out of the box. No additional configuration is required. The database file `db.sqlite3` is created automatically in the project root after running migrations.

### MySQL (Optional)

A MySQL configuration block is included in `settings.py` but is currently commented out. To switch to MySQL:

1. Create a MySQL database named `moviesstore`.
2. Uncomment the MySQL `DATABASES` configuration in `settings.py`.
3. Update the `USER`, `PASSWORD`, `HOST`, and `PORT` values to match your environment.
4. Comment out or remove the SQLite configuration.
5. Run migrations:

```bash
python manage.py migrate
```

---

## Administration Panel

The Django admin interface is available at:

```
http://127.0.0.1:8000/admin/
```

Log in with the superuser credentials created during setup. The following models are registered for administration:

| Model | Application | Admin Features |
|-------|-------------|----------------|
| Movie | movies | Search by name, ordered alphabetically |
| Review | movies | Standard CRUD |
| Report | movies | Standard CRUD |
| Order | cart | Standard CRUD |
| Item | cart | Standard CRUD |

Use the admin panel to add movie catalog entries, including name, price, description, and cover image.

---

## Application Architecture

### Request Flow

```
Browser Request
      |
      v
moviesstore/urls.py  (root URL dispatcher)
      |
      +-- home.urls       --> Home and About pages
      +-- movies.urls     --> Catalog, detail, reviews
      +-- accounts.urls   --> Auth and order history
      +-- cart.urls       --> Cart and checkout
      |
      v
View Function (business logic)
      |
      v
Template Rendering (HTML response)
```

### Template Inheritance

All page templates extend `moviesstore/templates/base.html`, which provides:

- Responsive navigation bar with authentication-aware links
- Bootstrap and Font Awesome asset loading
- Custom CSS via `style.css`
- Shared footer with contact information

Child templates override the `content` block to inject page-specific markup.

---

## Data Models

### Movie

Represents a film available for purchase.

| Field | Type | Description |
|-------|------|-------------|
| id | AutoField | Primary key |
| name | CharField(255) | Movie title |
| price | IntegerField | Price in dollars |
| description | TextField | Full description |
| image | ImageField | Cover image (stored in `media/movie_images/`) |

### Review

User-submitted review tied to a specific movie.

| Field | Type | Description |
|-------|------|-------------|
| id | AutoField | Primary key |
| comment | CharField(255) | Review text |
| date | DateTimeField | Auto-set on creation |
| movie | ForeignKey(Movie) | Associated movie |
| user | ForeignKey(User) | Review author |

### Report

Stores reported reviews for administrative review.

| Field | Type | Description |
|-------|------|-------------|
| id | AutoField | Primary key |
| comment | CharField(255) | Copied review text |
| date | DateTimeField | Auto-set on creation |
| movie | ForeignKey(Movie) | Associated movie |
| user | ForeignKey(User) | Original review author |

### Order

Represents a completed purchase transaction.

| Field | Type | Description |
|-------|------|-------------|
| id | AutoField | Primary key |
| total | IntegerField | Order total in dollars |
| date | DateTimeField | Auto-set on creation |
| user | ForeignKey(User) | Purchasing user |

### Item

Individual line item within an order.

| Field | Type | Description |
|-------|------|-------------|
| id | AutoField | Primary key |
| price | IntegerField | Unit price at time of purchase |
| quantity | IntegerField | Number of units purchased |
| order | ForeignKey(Order) | Parent order |
| movie | ForeignKey(Movie) | Purchased movie |

---

## URL Reference

### Home

| URL | Name | Description |
|-----|------|-------------|
| `/` | `home.index` | Landing page |
| `/about` | `home.about` | About page |

### Movies

| URL | Name | Description |
|-----|------|-------------|
| `/movies/` | `movies.index` | Movie catalog with search |
| `/movies/<id>/` | `movies.show` | Movie detail page |
| `/movies/<id>/review/create/` | `movies.create_review` | Submit a review (POST, login required) |
| `/movies/<id>/review/<review_id>/edit/` | `movies.edit_review` | Edit a review (login required) |
| `/movies/<id>/review/<review_id>/delete/` | `movies.delete_review` | Delete a review (login required) |
| `/movies/<id>/review/<review_id>/report/` | `movies.report_review` | Report a review (login required) |

### Accounts

| URL | Name | Description |
|-----|------|-------------|
| `/accounts/signup` | `accounts.signup` | User registration |
| `/accounts/login/` | `accounts.login` | User login |
| `/accounts/logout/` | `accounts.logout` | User logout (login required) |
| `/accounts/orders/` | `accounts.orders` | Order history (login required) |

### Cart

| URL | Name | Description |
|-----|------|-------------|
| `/cart/` | `cart.index` | View shopping cart |
| `/cart/<id>/add/` | `cart.add` | Add movie to cart (POST) |
| `/cart/clear/` | `cart.clear` | Remove all items from cart |
| `/cart/purchase/` | `cart.purchase` | Complete purchase (login required) |

---

## Static and Media Files

### Static Files

Static assets are served from `moviesstore/static/` and configured via:

```python
STATIC_URL = 'static/'
STATICFILES_DIRS = [BASE_DIR / 'moviesstore/static/']
```

Custom styles are defined in `moviesstore/static/css/style.css` and include:

- Hero banner background styling (`.bg-index`)
- Navigation link colors
- Footer styling (`.ms-footer`, `.ms-footer-bottom`)

### Media Files

User-uploaded content (movie cover images) is stored in the `media/` directory:

```python
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
MEDIA_URL = '/media/'
```

Media file serving is enabled in development via `urls.py`. In production, media files should be served by the web server or a dedicated storage service.

---

## Authentication and Authorization

The application uses Django's built-in authentication system.

### Registration

New users register via `/accounts/signup` using a custom `CustomUserCreationForm` that:

- Extends Django's `UserCreationForm`
- Applies Bootstrap `form-control` styling to all fields
- Displays validation errors as styled alert components

### Login and Logout

- Successful login redirects to the home page (`home.index`).
- Logout requires an authenticated session and redirects to the home page.
- Protected views use the `@login_required` decorator, which redirects unauthenticated users to the login page.

### Access Control Summary

| Action | Authentication Required |
|--------|------------------------|
| Browse movies | No |
| View movie details | No |
| Add to cart | No |
| Purchase cart | Yes |
| Create review | Yes |
| Edit/delete own review | Yes (must be review author) |
| Report another user's review | Yes (must not be review author) |
| View order history | Yes |

---

## Shopping Cart Implementation

The shopping cart uses Django session storage rather than a database model. This keeps cart data lightweight and temporary until checkout.

### How It Works

1. **Adding Items:** When a user submits the "Add to Cart" form on a movie detail page, the cart view stores a dictionary in `request.session['cart']` where keys are movie IDs and values are quantities.

2. **Viewing the Cart:** The cart index page retrieves session data, queries matching `Movie` objects, and calculates the total using `calculate_cart_total()` in `cart/utils.py`.

3. **Custom Template Filter:** The `get_quantity` filter in `cart/templatetags/cart_filters.py` retrieves the quantity for a given movie ID from the session cart dictionary.

4. **Checkout:** On purchase, the application creates an `Order` record and associated `Item` records for each movie in the cart, then clears the session.

5. **Clearing:** The clear action resets `request.session['cart']` to an empty dictionary.

---

## Review and Reporting System

### Reviews

Authenticated users can submit reviews on any movie detail page. Reviews are displayed in chronological order with the author's username and submission date.

- **Create:** POST to `/movies/<id>/review/create/`
- **Edit:** Only the original author can edit their review
- **Delete:** Only the original author can delete their review

### Reporting

Users can report reviews written by other users. When a review is reported:

1. A `Report` record is created with the review's comment, movie, and author.
2. The original review is removed from the movie detail page.
3. A confirmation page is displayed to the reporting user.

Reported reviews can be reviewed by administrators through the Django admin panel.

---

## Testing

Each Django application includes a `tests.py` file for unit and integration tests. Run the full test suite with:

```bash
python manage.py test
```

Run tests for a specific application:

```bash
python manage.py test movies
python manage.py test cart
python manage.py test accounts
python manage.py test home
```

---

## Deployment Considerations

This project is configured for local development. Before deploying to production, address the following:

| Setting | Current Value | Production Recommendation |
|---------|---------------|---------------------------|
| `DEBUG` | `True` | Set to `False` |
| `SECRET_KEY` | Hardcoded insecure key | Use environment variable |
| `ALLOWED_HOSTS` | Empty list | Add your domain(s) |
| Database | SQLite | Use PostgreSQL or MySQL |
| Static files | Development server | Run `collectstatic` and serve via CDN or web server |
| Media files | Local filesystem | Use cloud storage (S3, Azure Blob, etc.) |
| HTTPS | Not configured | Enable SSL/TLS |

Additional production steps:

```bash
python manage.py collectstatic
```

Configure a production WSGI server (Gunicorn, uWSGI) behind a reverse proxy (Nginx, Apache).

---

## Author

**Arnav Sutraway**

Georgia Institute of Technology  
CS 2340: Objects and Design

Portfolio: [arnavsutraway.wixsite.com/portfolio](https://arnavsutraway.wixsite.com/portfolio)

---

## License

This project is developed for academic purposes as part of coursework at the Georgia Institute of Technology. All rights reserved.
