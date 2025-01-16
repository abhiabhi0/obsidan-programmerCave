- **What is Django?**
  - Django is a high-level Python web framework designed to facilitate rapid development of secure and maintainable websites.
  - It emphasizes the "Don't Repeat Yourself" (DRY) principle, promoting reusability of components.
  - The framework includes built-in features such as user authentication, database connections, and CRUD operations (Create, Read, Update, Delete) [1][5].

- **Key Features of Django:**
  - **Rapid Development:** Django allows developers to build applications quickly by providing pre-built components.
  - **Security:** It includes protections against common security threats like SQL injection and cross-site scripting [3].
  - **Scalability:** Django can efficiently scale from small to large applications [3].
  - **Versatility:** It can be used for various types of applications, including content management systems and social networks [5].
  - **Open Source:** Django is free to use and has a large supportive community [3][5].

## How Does Django Work?

Django operates on the MVT (Model-View-Template) design pattern:

- **Model:**
  - Represents the data structure and interacts with the database using Object-Relational Mapping (ORM).
  - Models are defined in a file named `models.py`.
  
  Example:
  ```python
  from django.db import models

  class Book(models.Model):
      title = models.CharField(max_length=100)
      author = models.CharField(max_length=100)
      published_date = models.DateField()
  ```

- **View:**
  - Handles HTTP requests and returns responses. Views are defined in `views.py`.
  
  Example:
  ```python
  from django.shortcuts import render
  from .models import Book

  def book_list(request):
      books = Book.objects.all()
      return render(request, 'book_list.html', {'books': books})
  ```

- **Template:**
  - Defines how data is presented. Templates are typically HTML files stored in a `templates` folder.
  
  Example:
  ```html
  <!-- book_list.html -->
  <h1>Book List</h1>
  <ul>
      {% for book in books %}
          <li>{{ book.title }} by {{ book.author }}</li>
      {% endfor %}
  </ul>
  ```

## URL Routing

- Django manages URL routing through a file named `urls.py`, which maps URLs to views.

Example:
```python
from django.urls import path
from .views import book_list

urlpatterns = [
    path('books/', book_list, name='book_list'),
]
```

## Request Handling Flow

1. A user requests a URL.
2. Django checks `urls.py` to find the corresponding view.
3. The view retrieves data from the model.
4. The view sends data to the template.
5. The template generates HTML and returns it to the browser.

## Boilerplate Code Structure

When you create a new Django project, it typically includes the following files:

- **`manage.py`:** A command-line utility for managing your project.
- **`settings.py`:** Contains configuration settings for your project, including database settings and installed apps.
- **`urls.py`:** Defines URL patterns for routing requests.
- **`wsgi.py`:** An entry point for WSGI-compatible web servers to serve your project.

### Example of `settings.py`
```python
# settings.py
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / "db.sqlite3",
    }
}

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'myapp', # Your application name
]
```

This foundational knowledge will help you understand the structure and functionality of a Django application as you prepare for your interview.

Citations:
[1] https://www.w3schools.com/django/django_intro.php
[2] https://www.simplilearn.com/django-interview-questions-article
[3] https://www.javatpoint.com/django-tutorial
[4] https://www.geeksforgeeks.org/django-interview-questions/
[5] https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Server-side/Django/Introduction
[6] https://www.youtube.com/watch?v=9ai0b1LRMQM
[7] https://www.geeksforgeeks.org/django-basics/
[8] https://in.indeed.com/career-advice/interviewing/django-interview-questions