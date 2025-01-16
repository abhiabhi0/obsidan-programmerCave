### Overview of Django Models

- **Definition**: A Django model is a Python class that defines the structure of your data. Each model corresponds to a single database table, and each attribute of the model represents a field in that table.
- **Purpose**: Models simplify database interactions by abstracting the complexities of SQL. They allow developers to create, retrieve, update, and delete records without writing raw SQL queries.

### Key Characteristics

- **Subclassing**: Every model class must inherit from `django.db.models.Model`.
- **Field Types**: Each attribute in the model class is defined as a field type provided by Django (e.g., `CharField`, `IntegerField`).
- **Automatic Database API**: Django automatically generates a database-access API based on the defined models, which can be used to perform CRUD operations.

### Example of a Simple Model

Here’s an example of how to define a model in Django:

```python
from django.db import models

class Member(models.Model):
    firstname = models.CharField(max_length=255)
    lastname = models.CharField(max_length=255)
    email = models.EmailField(unique=True)
    join_date = models.DateField(auto_now_add=True)

    def __str__(self):
        return f"{self.firstname} {self.lastname}"
```

- **Fields Explained**:
  - `CharField`: A string field with a maximum length specified by `max_length`.
  - `EmailField`: A specialized field for storing email addresses, with an optional uniqueness constraint.
  - `DateField`: A field for storing dates; `auto_now_add=True` automatically sets the field to the current date when the record is created.
  
### Creating and Applying Migrations

After defining your models, you need to create migrations and apply them to generate the corresponding database tables.

1. **Create Migrations**: This command generates migration files based on the changes made to your models:
   ```bash
   python manage.py makemigrations
   ```

2. **Apply Migrations**: This command applies the generated migration files to your database:
   ```bash
   python manage.py migrate
   ```

### Viewing Migrations

To see the SQL commands that will be executed during migration, you can use:
```bash
python manage.py sqlmigrate members 0001
```
This command will show you the SQL statements that correspond to the first migration for the `members` app.

### Common Field Types in Django Models

- **CharField**: For short text fields (e.g., names, titles).
- **TextField**: For long text fields (e.g., descriptions).
- **IntegerField**: For integer values.
- **FloatField**: For floating-point numbers.
- **BooleanField**: For true/false values.
- **DateTimeField**: For date and time values.
- **ForeignKey**: To create a many-to-one relationship with another model.

### Example of Using ForeignKey

If you want to create a relationship between two models, you can use `ForeignKey`. Here’s an example:

```python
class Membership(models.Model):
    member = models.ForeignKey(Member, on_delete=models.CASCADE)
    membership_type = models.CharField(max_length=100)
    start_date = models.DateField()
```

In this example:
- The `Membership` model has a foreign key relationship with the `Member` model. 
- The `on_delete=models.CASCADE` argument specifies that if a member is deleted, all associated memberships should also be deleted.

### Conclusion

Django models are powerful tools for managing data within your application. By defining models as Python classes, you can easily interact with your database without needing extensive knowledge of SQL. Understanding how to create and manipulate models is fundamental for developing applications using Django.

Citations:
[1] https://www.geeksforgeeks.org/django-models/
[2] https://www.scaler.com/topics/django/django-models/
[3] https://www.javatpoint.com/django-model
[4] https://www.tutorialspoint.com/django/django_models.htm
[5] https://www.w3schools.com/django/django_models.php
[6] https://docs.djangoproject.com/en/5.1/topics/db/models/
[7] https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Server-side/Django/Models