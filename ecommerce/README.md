# Django E-commerce Starter

A minimal but functional e-commerce app: product catalog, categories, a
session-based cart, and checkout that creates real orders — no third-party
payment integration wired in yet, so it's a clean base to build on.

## Structure

```
ecommerce/
├── config/              # Project settings, urls, wsgi
├── apps/
│   ├── store/           # Categories & products, product listing/detail
│   ├── cart/            # Session-based shopping cart
│   └── orders/          # Checkout form, Order & OrderItem models
├── templates/           # Global base template
├── static/css/          # Stylesheet
├── media/products/      # Uploaded product images
└── manage.py
```

## Setup

```bash
python -m venv venv
source venv/bin/activate   # Windows: venv\Scripts\activate

pip install -r requirements.txt

cp .env.example .env

python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
```

Visit http://127.0.0.1:8000/ for the shop, and /admin/ to add categories
and products (with images and stock counts).

## How it works

- **Catalog**: `Category` and `Product` models live in `apps/store`. Add
  products through Django admin — image uploads work out of the box.
- **Cart**: `apps/cart/cart.py` implements a `Cart` class backed by the
  session (no login required to shop). A context processor makes the cart
  available in every template for the header badge.
- **Checkout**: `apps/orders` has an `OrderCreateForm` for shipping details.
  Submitting it converts the session cart into an `Order` + `OrderItem`
  records, then clears the cart.

## Extending this

- **Payments**: wire in Stripe Checkout or a similar provider inside
  `apps/orders/views.py` — currently orders are marked unpaid by default.
- **Accounts**: add Django's built-in auth views/templates if you want
  order history tied to logged-in users.
- **Search/filtering**: add query params to `product_list` in
  `apps/store/views.py` for price/search filters.
