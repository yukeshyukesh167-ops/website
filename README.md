# Ex.07 Restaurant Website
# Date: 05/12/2025
# AIM:
To develop a static Restaurant website to display the food items and services provided by them.

# DESIGN STEPS:
## Step 1:
Requirement collection.

## Step 2:
Creating the layout using HTML and CSS.

## Step 3:
Updating the sample content.

## Step 4:
Choose the appropriate style and color scheme.

## Step 5:
Validate the layout in various browsers.

## Step 6:
Validate the HTML code.

## Step 7:
Publish the website in the given URL.

# PROGRAM:

base.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Madras Bistro | Authentic Flavors</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@700&family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">
    <style>
        body { background-color: #121212; color: #f8f9fa; font-family: 'Poppins', sans-serif; overflow-x: hidden; }
        h1, h2, h3, h4, h5, .navbar-brand { font-family: 'Playfair Display', serif; }
        .navbar { background: rgba(0,0,0,0.9); backdrop-filter: blur(10px); border-bottom: 1px solid #333; padding: 1rem 0; }
        .navbar-brand { font-size: 1.8rem; color: #ff9800 !important; } /* Orange/Gold Brand Color */
        .nav-link { color: rgba(255,255,255,0.8) !important; transition: 0.3s; }
        .nav-link:hover { color: #ff9800 !important; }
        .btn-gold { background: linear-gradient(45deg, #ff9800, #f57c00); color: white; border: none; font-weight: 600; }
        .btn-gold:hover { background: #e65100; color: white; transform: translateY(-2px); }
        .card { background: rgba(255,255,255,0.05); border: 1px solid rgba(255,255,255,0.1); backdrop-filter: blur(5px); color: white; }
        footer { background-color: #000; border-top: 1px solid #333; }
        .footer-link { color: #888; text-decoration: none; }
        .footer-link:hover { color: #ff9800; }
    </style>
</head>
<body>
    <nav class="navbar navbar-expand-lg navbar-dark fixed-top">
        <div class="container">
            <a class="navbar-brand" href="{% url 'home' %}">
                <i class="fas fa-utensils me-2"></i>Madras Bistro
            </a>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav"><span class="navbar-toggler-icon"></span></button>
            <div class="collapse navbar-collapse" id="navbarNav">
                <ul class="navbar-nav ms-auto align-items-center">
                    <li class="nav-item"><a class="nav-link" href="{% url 'home' %}">Home</a></li>
                    <li class="nav-item"><a class="nav-link" href="{% url 'menu' %}">Menu</a></li>
                    <li class="nav-item"><a class="nav-link" href="{% url 'reviews' %}">Reviews</a></li>
                    <li class="nav-item"><a class="nav-link" href="{% url 'book_table' %}">Book Table</a></li>
                    <li class="nav-item"><a class="nav-link btn btn-outline-warning px-3 mx-2 position-relative" href="{% url 'view_cart' %}">
                        <i class="fas fa-shopping-cart"></i> <span class="position-absolute top-0 start-100 translate-middle badge rounded-pill bg-danger">{{ request.session.cart|length }}</span>
                    </a></li>
                    {% if user.is_authenticated %}
                        <li class="nav-item"><a class="nav-link text-warning" href="#">Hello, {{ user.username }}</a></li>
                        <li class="nav-item"><a class="btn btn-sm btn-outline-light ms-2" href="{% url 'logout' %}">Logout</a></li>
                    {% else %}
                        <li class="nav-item"><a class="nav-link" href="{% url 'login' %}">Login</a></li>
                    {% endif %}
                </ul>
            </div>
        </div>
    </nav>
    {% if messages %}
        <div class="container position-fixed start-50 translate-middle-x" style="top: 100px; z-index: 1050;">
            {% for message in messages %}
                <div class="alert alert-warning alert-dismissible fade show shadow-lg" role="alert">{{ message }}<button type="button" class="btn-close" data-bs-dismiss="alert"></button></div>
            {% endfor %}
        </div>
    {% endif %}
    <main>{% block content %}{% endblock %}</main>
    <footer class="py-5 mt-5">
        <div class="container text-center">
            <h4 class="text-white">Madras Bistro</h4>
            <p class="text-secondary small">Authentic Indian Cuisine • Modern Vibe</p>
            <p class="text-muted small">&copy; 2025 Madras Bistro. All rights reserved.</p>
        </div>
    </footer>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>

cart.html
{% extends 'base.html' %}
{% block content %}
<div class="container" style="margin-top: 100px;">
    <h2 class="text-warning mb-4">Your Cart</h2>
    {% if cart_items %}
        <div class="card p-4">
            <table class="table table-dark table-hover align-middle">
                <thead><tr><th>Item</th><th>Price</th><th>Qty</th><th>Subtotal</th></tr></thead>
                <tbody>
                    {% for item in cart_items %}
                    <tr>
                        <td><div class="d-flex align-items-center"><img src="{{ item.item.image.url }}" style="width: 50px; height: 50px; object-fit: cover; margin-right: 15px;">{{ item.item.name }}</div></td>
                        <td>₹{{ item.item.price }}</td>
                        <td>{{ item.qty }}</td>
                        <td class="text-warning">₹{{ item.subtotal }}</td>
                    </tr>
                    {% endfor %}
                </tbody>
                <tfoot><tr><td colspan="3" class="text-end h4">Total:</td><td class="text-warning h4">₹{{ total }}</td></tr></tfoot>
            </table>
            <div class="d-flex justify-content-end mt-4">
                <a href="{% url 'menu' %}" class="btn btn-outline-light me-3">Continue Ordering</a>
                <a href="{% url 'checkout' %}" class="btn btn-gold px-5">Checkout Now</a>
            </div>
        </div>
    {% else %}
        <div class="text-center py-5"><h3 class="text-muted">Your cart is empty 😔</h3><a href="{% url 'menu' %}" class="btn btn-gold mt-3">Browse Menu</a></div>
    {% endif %}
</div>
{% endblock %}

home.html
{% extends 'base.html' %}
{% block content %}
<style>
    .hero-section {
        height: 100vh;
        background: linear-gradient(rgba(0,0,0,0.7), rgba(0,0,0,0.7)), url('https://images.unsplash.com/photo-1514362545857-3bc16549766b?w=1600') center/cover fixed;
        display: flex; align-items: center; justify-content: center; text-align: center;
    }
    .carousel-item img { height: 500px; object-fit: cover; border-radius: 20px; opacity: 0.8; }
</style>

<section class="hero-section">
    <div class="container animate_animated animate_fadeIn">
        <h1 class="display-1 fw-bold text-white mb-3">Madras <span class="text-warning">Bistro</span></h1>
        <p class="lead text-light mb-5 fs-4">A Symphony of Spices in Every Bite.</p>
        <div class="d-flex justify-content-center gap-3">
            <a href="{% url 'menu' %}" class="btn btn-gold btn-lg px-5 rounded-pill shadow-lg">Order Now</a>
            <a href="{% url 'book_table' %}" class="btn btn-outline-light btn-lg px-5 rounded-pill">Book Table</a>
        </div>
    </div>
</section>

<div class="container my-5 py-5">
    <h2 class="text-center text-warning display-5 mb-5">Chef's Specials</h2>
    <div id="specialsCarousel" class="carousel slide" data-bs-ride="carousel">
        <div class="carousel-inner">
            {% for item in specials %}
            <div class="carousel-item {% if forloop.first %}active{% endif %}">
                <div class="row align-items-center">
                    <div class="col-md-6"><img src="{{ item.image.url }}" class="d-block w-100 shadow-lg" alt="{{ item.name }}"></div>
                    <div class="col-md-6 p-5">
                        <h3 class="text-white display-6">{{ item.name }}</h3>
                        <p class="text-secondary fs-5 my-4">{{ item.description }}</p>
                        <h4 class="text-warning mb-4">₹{{ item.price }}</h4>
                        <a href="{% url 'add_to_cart' item.id %}" class="btn btn-gold">Add to Order</a>
                    </div>
                </div>
            </div>
            {% endfor %}
        </div>
        <button class="carousel-control-prev" type="button" data-bs-target="#specialsCarousel" data-bs-slide="prev"><span class="carousel-control-prev-icon"></span></button>
        <button class="carousel-control-next" type="button" data-bs-target="#specialsCarousel" data-bs-slide="next"><span class="carousel-control-next-icon"></span></button>
    </div>
</div>
{% endblock %}

login.html
{% extends 'base.html' %}
{% block content %}
<div class="container d-flex justify-content-center align-items-center" style="min-height: 80vh;">
    <div class="card p-5 shadow-lg border-0 bg-dark" style="width: 400px; border: 1px solid #444;">
        <h2 class="text-warning text-center mb-4">Welcome Back</h2>
        <form method="POST">
            {% csrf_token %}
            {{ form.as_p }}
            <button type="submit" class="btn btn-gold w-100 mt-3">Login</button>
        </form>
        <p class="text-secondary text-center mt-3">
            New here? <a href="{% url 'signup' %}" class="text-warning">Create Account</a>
        </p>
    </div>
</div>
{% endblock %}

menu.html
{% extends 'base.html' %}
{% block content %}
<div class="container" style="margin-top: 100px;">
    <h2 class="text-center text-warning mb-4">Our Menu</h2>
    <div class="d-flex justify-content-center mb-5 gap-2">
        <a href="{% url 'menu' %}" class="btn btn-outline-light rounded-pill">All</a>
        {% for cat in categories %}
        <a href="?category={{ cat.id }}" class="btn btn-outline-light rounded-pill">{{ cat.name }}</a>
        {% endfor %}
    </div>
    <div class="row g-4">
        {% for item in items %}
        <div class="col-md-4 col-lg-3">
            <div class="card h-100 shadow-lg">
                <div style="height: 200px; overflow: hidden;"><img src="{{ item.image.url }}" class="card-img-top h-100 w-100" style="object-fit: cover;"></div>
                <div class="card-body d-flex flex-column">
                    <h5 class="card-title text-warning">{{ item.name }}</h5>
                    <p class="card-text text-secondary small">{{ item.description|truncatechars:50 }}</p>
                    <div class="mt-auto d-flex justify-content-between align-items-center">
                        <span class="h5 mb-0 text-white">₹{{ item.price }}</span>
                        <a href="{% url 'add_to_cart' item.id %}" class="btn btn-gold btn-sm rounded-circle"><i class="fas fa-plus"></i></a>
                    </div>
                </div>
            </div>
        </div>
        {% endfor %}
    </div>
</div>
{% endblock %}

reviews.html
{% extends 'base.html' %}
{% block content %}
<div class="container" style="margin-top: 100px;">
    <div class="text-center mb-5">
        <h1 class="text-warning display-4">Customer Stories</h1>
        <p class="text-light lead">What people are saying about the Velvet Fork experience.</p>
    </div>

    <div class="card bg-dark border-secondary mb-5 p-4 shadow-sm mx-auto" style="max-width: 600px;">
        <h4 class="text-white mb-3">Leave a Review</h4>
        {% if user.is_authenticated %}
            <form method="POST">
                {% csrf_token %}
                <div class="mb-3">
                    <label class="text-secondary">Rating (1-5)</label>
                    {{ form.rating }}
                </div>
                <div class="mb-3">
                    <label class="text-secondary">Your Comment</label>
                    {{ form.comment }}
                </div>
                <button type="submit" class="btn btn-gold">Post Review</button>
            </form>
        {% else %}
            <p class="text-secondary">Please <a href="{% url 'login' %}" class="text-warning">login</a> to leave a review.</p>
        {% endif %}
    </div>

    <div class="row g-4">
        {% for review in reviews %}
        <div class="col-md-6 col-lg-4">
            <div class="card bg-black border-secondary h-100 p-4">
                <div class="d-flex justify-content-between align-items-center mb-3">
                    <h5 class="text-white mb-0">{{ review.user.username }}</h5>
                    <div class="text-warning">
                        {% for i in "12345"|make_list %}
                            {% if forloop.counter <= review.rating %}★{% else %}☆{% endif %}
                        {% endfor %}
                    </div>
                </div>
                <p class="text-secondary">"{{ review.comment }}"</p>
                <small class="text-muted">{{ review.created_at|date:"M d, Y" }}</small>
            </div>
        </div>
        {% endfor %}
    </div>
</div>
{% endblock %}

signup.html
{% extends 'base.html' %}
{% block content %}
<div class="container d-flex justify-content-center align-items-center" style="min-height: 80vh;">
    <div class="card p-5 shadow-lg border-0 bg-dark" style="width: 400px; border: 1px solid #444;">
        <h2 class="text-warning text-center mb-4">Welcome Back</h2>
        <form method="POST">
            {% csrf_token %}
            {{ form.as_p }}
            <button type="submit" class="btn btn-gold w-100 mt-3">Login</button>
        </form>
        <p class="text-secondary text-center mt-3">
            New here? <a href="{% url 'signup' %}" class="text-warning">Create Account</a>
        </p>
    </div>
</div>
{% endblock %}

book_table.html
{% extends 'base.html' %}

{% block content %}
<div class="container" style="margin-top: 100px;">
    <div class="row justify-content-center">
        <div class="col-md-8 col-lg-6">
            <div class="card shadow-lg border-0">
                <div class="card-body p-5">
                    <h2 class="text-center text-warning mb-4">Reserve Your Table</h2>
                    <p class="text-center text-light mb-4">Join us for an unforgettable dining experience.</p>
                    
                    <form method="POST">
                        {% csrf_token %}
                        <div class="mb-3">
                            <label class="text-secondary">Full Name</label>
                            <input type="text" name="name" class="form-control bg-dark text-white border-secondary" required>
                        </div>
                        <div class="row">
                            <div class="col-md-6 mb-3">
                                <label class="text-secondary">Email</label>
                                <input type="email" name="email" class="form-control bg-dark text-white border-secondary" required>
                            </div>
                            <div class="col-md-6 mb-3">
                                <label class="text-secondary">Phone</label>
                                <input type="tel" name="phone" class="form-control bg-dark text-white border-secondary" required>
                            </div>
                        </div>
                        <div class="row">
                            <div class="col-md-6 mb-3">
                                <label class="text-secondary">Date & Time</label>
                                <input type="datetime-local" name="date_time" class="form-control bg-dark text-white border-secondary" required>
                            </div>
                            <div class="col-md-6 mb-3">
                                <label class="text-secondary">Guests</label>
                                <input type="number" name="guests" min="1" max="20" class="form-control bg-dark text-white border-secondary" required>
                            </div>
                        </div>
                        <div class="mb-4">
                            <label class="text-secondary">Special Request</label>
                            <textarea name="message" class="form-control bg-dark text-white border-secondary" rows="3"></textarea>
                        </div>
                        <button type="submit" class="btn btn-gold w-100 py-3 rounded-pill">Confirm Reservation</button>
                    </form>
                </div>
            </div>
        </div>
    </div>
</div>
{% endblock %}

# OUTPUT:
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/adab377f-21a7-490d-b067-0d7e08c2b956" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2995a952-9eb2-498e-9d99-030d7d3150e5" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4a28c2c5-a575-44d0-8196-b729c819de9f" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/52606a14-7da1-49d6-8dd9-2e66bd1dc022" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c579c7dc-c9ea-4e91-b9c2-b108f97534fd" />





# RESULT:
The program for designing software company website using HTML and CSS is completed successfully.
