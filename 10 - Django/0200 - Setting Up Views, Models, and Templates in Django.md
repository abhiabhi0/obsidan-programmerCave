### 1. Setting Up the Project Structure

To begin, create a new Django project and an application within it. The following steps outline how to set up the structure:

- Create a new Django project:
  ```bash
  django-admin startproject my_tennis_club
  cd my_tennis_club
  ```

- Create a new application:
  ```bash
  python manage.py startapp members
  ```

### 2. Configuring the Application

After creating the application, you need to add it to your project's settings:

- Open `settings.py` located in the `my_tennis_club/my_tennis_club/` directory.
- Add `'members'` to the `INSTALLED_APPS` list:
  ```python
  INSTALLED_APPS = [
      'django.contrib.admin',
      'django.contrib.auth',
      'django.contrib.contenttypes',
      'django.contrib.sessions',
      'django.contrib.messages',
      'django.contrib.staticfiles',
      'members',  # Add your app here
  ]
  ```

- Run migrations to set up your database:
  ```bash
  python manage.py migrate
  ```

### 3. Creating Models

Models define the data structure for your application. In `members/models.py`, create a simple model:

```python
from django.db import models

class Member(models.Model):
    name = models.CharField(max_length=100)
    email = models.EmailField()
    join_date = models.DateField(auto_now_add=True)

    def __str__(self):
        return self.name
```

- After defining your model, run the following commands to create and apply migrations:
  ```bash
  python manage.py makemigrations
  python manage.py migrate
  ```

### 4. Setting Up Views

Views handle the logic for your application. In `members/views.py`, create a view that retrieves and displays members:

```python
from django.shortcuts import render
from .models import Member

def member_list(request):
    members = Member.objects.all()
    return render(request, 'members/member_list.html', {'members': members})
```

### 5. Creating Templates

Templates define how data is presented in HTML format. Follow these steps to set up templates:

- Create a directory structure for templates:
  ```
  my_tennis_club/
      members/
          templates/
              members/
                  member_list.html
  ```

- In `member_list.html`, add the following HTML code:
```html
<!DOCTYPE html>
<html>
<head>
    <title>Member List</title>
</head>
<body>
    <h1>Members</h1>
    <ul>
        {% for member in members %}
            <li>{{ member.name }} - {{ member.email }}</li>
        {% endfor %}
    </ul>
</body>
</html>
```

### 6. Configuring URL Routing

To connect URLs to views, edit `members/urls.py` (create this file if it doesn't exist):

```python
from django.urls import path
from .views import member_list

urlpatterns = [
    path('', member_list, name='member_list'),
]
```

Next, include this URL configuration in the main project’s `urls.py`:

```python
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('admin/', admin.site.urls),
    path('members/', include('members.urls')),  # Include app URLs here
]
```

### 7. Running the Server

Finally, start the Django development server:

```bash
python manage.py runserver
```

Open your web browser and navigate to `http://127.0.0.1:8000/members/` to see the list of members displayed.

### Summary of Steps

1. **Create Project and App**: Use Django commands to set up your project and app.
2. **Configure Settings**: Add your app to `INSTALLED_APPS`.
3. **Define Models**: Create models in `models.py`.
4. **Create Views**: Implement logic in `views.py`.
5. **Set Up Templates**: Design HTML templates for displaying data.
6. **Configure URLs**: Map URLs to views using `urls.py`.
7. **Run Server**: Start the server and access your application.

This structured approach will help you effectively set up views, models, and templates in Django for your project or during interviews.

Citations:
[1] https://www.w3schools.com/django/django_templates.php
[2] https://docs.djangoproject.com/en/5.1/intro/tutorial03/
[3] https://www.tangowithdjango.com/book/chapters/models_templates.html
[4] https://www.geeksforgeeks.org/django-templates/
[5] https://docs.djangoproject.com/en/5.1/topics/templates/