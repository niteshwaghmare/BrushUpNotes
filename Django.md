# ⛵️ Django

Django is a high-level Python web framework that encourages rapid development and clean, pragmatic design. Built by experienced developers, it takes care of much of the hassle of web development, so you can focus on writing your app without needing to reinvent the wheel. It’s free and open source.

- **Ridiculously fast.**

  Django was designed to help developers take applications from concept to completion as quickly as possible.

- **Reassuringly secure.**

  Django takes security seriously and helps developers avoid many common security mistakes.

- **Exceedingly scalable.**

  Some of the busiest sites on the web leverage Django’s ability to quickly and flexibly scale.

## Installation & Setup

```shell
# Install package
$pip install django

# Create project
$ django-admin startproject project_name
$ python3 -m django startproject proejct name 

# Without creating sub dir 
$ django-admin startproject project_name .

# To create a app
$ python manage.py startapp app_name

# To run a project
$ python manage.py runserver

# Create a file under app_name/migrations with the database structure to create
$ python manage.py makemigrations

# Will read the migrations files and create the actual database and tables
$ python manage.py migrate

# Create superuser for authenficiation/admin panel
$ python manage.py createsuperuser

# Collect static files in single location (Deployment)
$ python manage.py collectstatic 

# Will clear all previou static
$ python manage.py collectstatic --clear
```

## Project structure

### Project

```shell
.
├── core
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── manage.py
└── users
    ├── __init__.py
    ├── admin.py
    ├── apps.py
    ├── migrations
    │   └── __init__.py
    ├── models.py
    ├── tests.py
    └── views.py
```

A Django project contains the following packages and files. The outer directory is just a container for the application. We can rename it further.

- **manage.py:** It is a command-line utility which allows us to interact with the project in various ways and also used to manage an application that we will see later on in this tutorial.
- A directory (core) located inside, is the actual application package name. Its name is the Python package name which we'll need to use to import module inside the application.
- **__init__.py:** It is an empty file that tells to the Python that this directory should be considered as a Python package.
- **settings.py:** This file is used to configure application settings such as database connection, static files linking etc.
- **urls.py:** This file contains the listed URLs of the application. In this file, we can mention the URLs and corresponding actions to perform the task and display the view.
- **wsgi.py:** It is an entry-point for WSGI-compatible web servers to serve Django project.

### App

Apps are bulding blocks for the overall django project. we can say as each feature can be treated as one app like user, books, and blogs etc.

> **Projects vs. apps**
>
> What’s the difference between a project and an app? An app is a web application that does something – e.g., a blog system, a database of public records or a small poll app. A project is a collection of configuration and apps for a particular website. A project can contain multiple apps. An app can be in multiple projects.

## MVT

The MVT (Model View Template) is a software design pattern. It is a collection of three important components Model View and Template. The Model helps to handle database. It is a data access layer which handles the data.

The Template is a presentation layer which handles User Interface part completely. The View is used to execute the business logic and interact with a model to carry data and renders a template.

![MVT](https://www.javatpoint.com/django/images/django-mvt-based-control-flow.png)

## Model

A model is the single, definitive source of information about your data. It contains the essential fields and behaviors of the data you’re storing. Generally, each model maps to a single database table.

### Relationships

Django models can have relationships with other models. These relationships allow you to establish connections between data in different tables, and make it easy to query related data.

1. One-to-many relationships
2. Many-to-many relationships
3. One-to-one relationships

#### One-to-many relationships 

A one-to-many relationship is where one record in a table can be associated with many records in another table, but each record in the second table can only be associated with one record in the first table. This is the most common type of relationship in Django. In a one-to-many relationship, the model with the foreign key is the "many" side, and the model that the foreign key refers to is the "one" side.

Example: A `Person` model has many `Address` records associated with it.

```python
class Person(models.Model):
    name = models.CharField(max_length=50)

class Address(models.Model):
    person = models.ForeignKey(Person, on_delete=models.CASCADE)
    street_address = models.CharField(max_length=100)
    city = models.CharField(max_length=50)
```

#### Many-to-many relationships

A many-to-many relationship is where records in one table can be associated with many records in another table, and vice versa. In a many-to-many relationship, Django creates an intermediate table that holds the foreign keys to both tables.

Example: A `Person` model can have many `Interests`, and each `Interest` can be associated with many `Person` records.

```python
class Person(models.Model):
    name = models.CharField(max_length=50)
    interests = models.ManyToManyField('Interest')

class Interest(models.Model):
    name = models.CharField(max_length=50)
```

#### One-to-one relationships

A one-to-one relationship is where each record in one table is associated with exactly one record in another table. In a one-to-one relationship, one model has a `OneToOneField` that references another model.

Example: A `Person` model has a one-to-one relationship with a `Profile` model.

```python
class Person(models.Model):
    name = models.CharField(max_length=50)
    profile = models.OneToOneField('Profile', on_delete=models.CASCADE)

class Profile(models.Model):
    bio = models.TextField()
    age = models.IntegerField()
```

Django also supports reverse relationships, which allow you to access related objects from the "many" side of a one-to-many relationship. For example, you can use the `related_name` argument to define a reverse relationship for a `ForeignKey` field:

```python
class Person(models.Model):
    name = models.CharField(max_length=50)

class Address(models.Model):
    person = models.ForeignKey(Person, on_delete=models.CASCADE, related_name='addresses')
    street_address = models.CharField(max_length=100)
    city = models.CharField(max_length=50)
```

#### Foreign Key 🔑 on_Delete

In Django, when you define a ForeignKey or OneToOneField in a model, you can specify the behavior that should occur when the referenced object is deleted. This behavior is defined using the `on_delete` argument, which accepts various options. Here are explanations of some commonly used options:

- `models.CASCADE`:
  - Automatically deletes related objects (children) when the referenced object (parent) is deleted.
  - Provides a "cascade delete" behavior.

- `models.DO_NOTHING`:
  - Does nothing automatically when the referenced object is deleted.
  - Requires manual handling of data integrity in your code.

- `models.SET_NULL`:
  - Sets the foreign key to `NULL` in the child objects when the referenced object is deleted.
  - Useful when you want to maintain child records but remove the reference to the parent.

- `models.SET_DEFAULT`:
  - Sets the foreign key to its default value when the referenced object is deleted.
  - Requires specifying a default value for the foreign key field.

- `models.SET(value)`:
  - Sets the foreign key to the specified value when the referenced object is deleted.
  - You provide the value to set.

- `models.PROTECT`:
  - Prevents the deletion of the referenced object if it is referenced by any child objects.
  - Acts as a safeguard to maintain data integrity.

These options allow you to define how the database should handle referential integrity when related objects are deleted, providing flexibility in managing relationships between models in your Django application.

### Fields types

| Field Name           | Class                                                        | Particular                                                   |
| :------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| AutoField            | class AutoField(**options)                                   | It An IntegerField that automatically increments.            |
| BigAutoField         | class BigAutoField(**options)                                | It is a 64-bit integer, much like an AutoField except that it is guaranteed to fit numbers from 1 to 9223372036854775807. |
| BigIntegerField      | class BigIntegerField(**options)                             | It is a 64-bit integer, much like an IntegerField except that it is guaranteed to fit numbers from -9223372036854775808 to 9223372036854775807. |
| BinaryField          | class BinaryField(**options)                                 | A field to store raw binary data.                            |
| BooleanField         | class BooleanField(**options)                                | A true/false field. The default form widget for this field is a CheckboxInput. |
| CharField            | class DateField(auto_now=False, auto_now_add=False, **options) | It is a date, represented in Python by a datetime.date instance. |
| DateTimeField        | class DateTimeField(auto_now=False, auto_now_add=False, **options) | It is a date, represented in Python by a datetime.date instance. |
| DateTimeField        | class DateTimeField(auto_now=False, auto_now_add=False, **options) | It is used for date and time, represented in Python by a datetime.datetime instance. |
| DecimalField         | class DecimalField(max_digits=None, decimal_places=None, **options) | It is a fixed-precision decimal number, represented in Python by a Decimal instance. |
| DurationField        | class DurationField(**options)                               | A field for storing periods of time.                         |
| EmailField           | class EmailField(max_length=254, **options)                  | It is a CharField that checks that the value is a valid email address. |
| FileField            | class FileField(upload_to=None, max_length=100, **options)   | It is a file-upload field.                                   |
| FloatField           | class FloatField(**options)                                  | It is a floating-point number represented in Python by a float instance. |
| ImageField           | class ImageField(upload_to=None, height_field=None, width_field=None, max_length=100, **options) | It inherits all attributes and methods from FileField, but also validates that the uploaded object is a valid image. |
| IntegerField         | class IntegerField(**options)                                | It is an integer field. Values from -2147483648 to 2147483647 are safe in all databases supported by Django. |
| NullBooleanField     | class NullBooleanField(**options)                            | Like a BooleanField, but allows NULL as one of the options.  |
| PositiveIntegerField | class PositiveIntegerField(**options)                        | Like an IntegerField, but must be either positive or zero (0). Values from 0 to 2147483647 are safe in all databases supported by Django. |
| SmallIntegerField    | class SmallIntegerField(**options)                           | It is like an IntegerField, but only allows values under a certain (database-dependent) point. |
| TextField            | class TextField(**options)                                   | A large text field. The default form widget for this field is a Textarea. |
| TimeField            | class TimeField(auto_now=False, auto_now_add=False, **options) | A time, represented in Python by a datetime.time instance.   |

### Field options

| Field Options | Particulars                                                  |
| :------------ | :----------------------------------------------------------- |
| Null          | Django will store empty values as NULL in the database.      |
| Blank         | It is used to allowed field to be blank.                     |
| Choices       | An iterable (e.g., a list or tuple) of 2-tuples to use as choices for this field. |
| Default       | The default value for the field. This can be a value or a callable object. |
| help_text     | Extra "help" text to be displayed with the form widget. It's useful for documentation even if your field isn't used on a form. |
| primary_key   | This field is the primary key for the model.                 |
| Unique        | This field must be unique throughout the table.              |

### Example 📌

```python
from django.db import models

# Define a custom model manager
class PersonManager(models.Manager):
    def get_active(self):
        return self.filter(is_active=True)

# Define a model with custom fields and methods
class Person(models.Model):
    # Define fields
    id = models.BigAutoField(primary_key=True)
    first_name = models.CharField(max_length=50)
    last_name = models.CharField(max_length=50)
    email = models.EmailField(max_length=254, unique=True)
    bio = models.TextField(blank=True)
    birth_date = models.DateField(null=True, blank=True)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    # Define a custom model manager
    objects = PersonManager()

    # Define a custom method on model instances
    def full_name(self):
        return f"{self.first_name} {self.last_name}"

    # Define a custom string representation for the model
    def __str__(self):
        return f"{self.full_name()} ({self.email})"

    # Define a custom Meta class for the model
    class Meta:
      	# Table name
        db_table = "person"
        ordering = ['-created_at']
        verbose_name_plural = 'People'

# Define a related model with a foreign key to Person
class Address(models.Model):
    # Define fields
    street_address = models.CharField(max_length=100)
    city = models.CharField(max_length=50)
    state = models.CharField(max_length=50)
    postal_code = models.CharField(max_length=10)

    # Define a foreign key to Person
    person = models.ForeignKey(Person, on_delete=models.CASCADE, related_name='addresses')

    # Define a custom string representation for the model
    def __str__(self):
        return f"{self.street_address}, {self.city}, {self.state} {self.postal_code}"
```

### Extras:

1. **Abstract class: **Abstract base classes are useful when you want to put some common information into a number of other models.

2. **Meta Options: **Give your model metadata by using an inner `class Meta`,

3. **Validators: **A validator is a callable that takes a value and raises a [`ValidationError`](https://docs.djangoproject.com/en/4.2/ref/exceptions/#django.core.exceptions.ValidationError) if it doesn’t meet some criteria. Validators can be useful for reusing validation logic between different types of fields.

4. **Overwrite saving**:

   



## QuerySet

A QuerySet in Django is a collection of objects that are retrieved from a database. It is a powerful tool that allows you to filter, order, and paginate your data.

Django QuerySets are designed to be efficient and optimized for performance. Here are some ways that QuerySets can help improve the performance of your Django application:

1. Lazy Evaluation: QuerySets are lazily evaluated, meaning that they are only executed when needed. This helps reduce unnecessary database queries and improves overall performance.
2. Caching: Once a QuerySet has been evaluated, the results are cached. This means that if the same QuerySet is evaluated again, the results will be retrieved from the cache rather than executing the query again.
3. Query Optimization: Django QuerySets are optimized to minimize the number of queries needed to retrieve data. For example, related objects are retrieved using a single JOIN statement rather than multiple queries.
4. QuerySet Methods: QuerySet methods like `select_related()` and `prefetch_related()` can help reduce the number of database queries needed to retrieve related objects. These methods also help avoid the N+1 query problem, where multiple database queries are executed to retrieve related objects.
5. QuerySet Caching: QuerySet caching can be used to store the results of a QuerySet in memory for a specified period of time. This can improve performance by reducing the number of database queries needed to retrieve the same data multiple times.
6. Database Indexing: Proper indexing of database tables can significantly improve the performance of Django QuerySets. You should ensure that all fields used in filters and ordering are properly indexed.

#### Examples 📌

```python
# Retrieve all people
people = Person.objects.all()

# Retrieve a specific person by primary key
person = Person.objects.get(pk=1)

# Retrieve people whose first name is "John"
people = Person.objects.filter(first_name="John")

# Retrieve people whose last name contains "Smith"
people = Person.objects.filter(last_name__contains="Smith")

# Retrieve people whose email ends with "@example.com"
people = Person.objects.filter(email__endswith="@example.com")

# Retrieve people ordered by their first name in ascending order
# Just add - if for decending order
people = Person.objects.order_by('first_name')

# Retrieve the latest 5 people created
people = Person.objects.order_by('-created_at')[:5]

# Retrieve the number of people
count = Person.objects.count()

# Retrieve people using a custom manager method
people = Person.objects.get_active()

# Retrieve a person and their related addresses
person = Person.objects.prefetch_related('addresses').get(pk=1)
addresses = person.addresses.all()

# Retrieve all people who have addresses in a specific city
people = Person.objects.filter(addresses__city="New York")


# Retrieve all people who have addresses in multiple cities
people = Person.objects.annotate(num_addresses=Count('addresses')).filter(num_addresses__gt=1)

# Retrieve all people who have addresses in a specific city and state
people = Person.objects.filter(addresses__city="New York", addresses__state="NY")

# Retrieve all people who have at least one address with a postal code starting with "90"
people = Person.objects.filter(addresses__postal_code__startswith="90")

# Retrieve all people who have the same last name as another person
from django.db.models import F
people = Person.objects.filter(last_name__in=Person.objects.values('last_name').annotate(count=Count('id')).filter(count__gt=1).values('last_name'))

# Retrieve all people who have addresses and were created after a specific date
from datetime import date
people = Person.objects.filter(addresses__isnull=False, created_at__gt=date(2022, 1, 1))


# Retrieve all people who have addresses and were created within a specific date range
start_date = date(2022, 1, 1)
end_date = date(2022, 12, 31)
people = Person.objects.filter(addresses__isnull=False, created_at__range=(start_date, end_date))
```

```python
# NOTE: Join query example with adding extra column
all_customers = Customers.objects.annotate(
	current_status = Subquery(
	Customer_Stages_Tracker.objects.filter(customer=OuterRef('customer_id')).order_by('-					stage_created_at').values('current_stage__stage_name')[:1]))

# Same thing
SELECT
    u.id AS user_id,
    u.username,
    u.email,
    oa.name AS last_action_name
FROM 
    users u
LEFT JOIN
    user_action_track uat ON u.id = uat.user_id
LEFT JOIN
    org_actions oa ON uat.action_id = oa.id
WHERE
    uat.timestamp = (
        SELECT MAX(timestamp) FROM user_action_track WHERE user_id = u.id
    );
```



## View

A view is a place where we put our business logic of the application. The view is a python function which is used to perform some business logic and return a response to the user. This response can be the HTML contents of a Web page, or a redirect, or a 404 error.

### Returning Errors

Django provides various built-in error classes that are the subclass of **HttpResponse** and use to show error message as HTTP response. Some classes are listed below.

| Class                         | Description                                                  |
| :---------------------------- | :----------------------------------------------------------- |
| class HttpResponseNotModified | It is used to designate that a page hasn't been modified since the user's last request (status code 304). |
| class HttpResponseBadRequest  | It acts just like HttpResponse but uses a 400 status code.   |
| class HttpResponseNotFound    | It acts just like HttpResponse but uses a 404 status code.   |
| class HttpResponseNotAllowed  | It acts just like HttpResponse but uses a 410 status code.   |
| HttpResponseServerError       | It acts just like HttpResponse but uses a 500 status code.   |

### Funcation based view Examples 📌

```python
from django.shortcuts import render, get_object_or_404, redirect
from .models import Person

def person_list(request):
    """
    Display a list of all people.
    """
    people = Person.objects.all()
    return render(request, 'person/list.html', {'people': people})

def person_detail(request, pk):
    """
    Display details of a specific person.
    """
    person = get_object_or_404(Person, pk=pk)
    return render(request, 'person/detail.html', {'person': person})

def person_create(request):
    """
    Create a new person.
    """
    if request.method == 'POST':
        # Handle form submission
        first_name = request.POST.get('first_name')
        last_name = request.POST.get('last_name')
        email = request.POST.get('email')
        bio = request.POST.get('bio')
        birth_date = request.POST.get('birth_date')
        
        try:
            person = Person.objects.create(
                first_name=first_name,
                last_name=last_name,
                email=email,
                bio=bio,
                birth_date=birth_date
            )
            return redirect('person_detail', pk=person.pk)
        except Exception as e:
            error_message = f"Error creating person: {str(e)}"
            return render(request, 'person/create.html', {'error_message': error_message})
    else:
        # Render the empty form for creating a person
        return render(request, 'person/create.html')

def person_update(request, pk):
    """
    Update an existing person.
    """
    person = get_object_or_404(Person, pk=pk)

    if request.method == 'POST':
        # Handle form submission
        person.first_name = request.POST.get('first_name')
        person.last_name = request.POST.get('last_name')
        person.email = request.POST.get('email')
        person.bio = request.POST.get('bio')
        person.birth_date = request.POST.get('birth_date')
        
        try:
            person.save()
            return redirect('person_detail', pk=person.pk)
        except Exception as e:
            error_message = f"Error updating person: {str(e)}"
            return render(request, 'person/update.html', {'person': person, 'error_message': error_message})
    else:
        # Render the form pre-filled with person's data
        return render(request, 'person/update.html', {'person': person})

def person_delete(request, pk):
    """
    Delete an existing person.
    """
    person = get_object_or_404(Person, pk=pk)

    if request.method == 'POST':
        try:
            person.delete()
            return redirect('person_list')
        except Exception as e:
            error_message = f"Error deleting person: {str(e)}"
            return render(request, 'person/detail.html', {'person': person, 'error_message': error_message})
    else:
        # Render the confirmation page for deleting the person
        return render(request, 'person/delete.html', {'person': person})
```

### Class-based views Examples 📌

```python
from django.shortcuts import render, get_object_or_404, redirect
from django.views import View
from .models import Person

class PersonListView(View):
    """
    Display a list of all people.
    """
    def get(self, request):
        people = Person.objects.all()
        return render(request, 'person/list.html', {'people': people})


class PersonDetailView(View):
    """
    Display details of a specific person.
    """
    def get(self, request, pk):
        person = get_object_or_404(Person, pk=pk)
        return render(request, 'person/detail.html', {'person': person})


class PersonCreateView(View):
    """
    Create a new person.
    """
    def get(self, request):
        return render(request, 'person/create.html')

    def post(self, request):
        first_name = request.POST.get('first_name')
        last_name = request.POST.get('last_name')
        email = request.POST.get('email')
        bio = request.POST.get('bio')
        birth_date = request.POST.get('birth_date')

        try:
            person = Person.objects.create(
                first_name=first_name,
                last_name=last_name,
                email=email,
                bio=bio,
                birth_date=birth_date
            )
            return redirect('person_detail', pk=person.pk)
        except Exception as e:
            error_message = f"Error creating person: {str(e)}"
            return render(request, 'person/create.html', {'error_message': error_message})


class PersonUpdateView(View):
    """
    Update an existing person.
    """
    def get(self, request, pk):
        person = get_object_or_404(Person, pk=pk)
        return render(request, 'person/update.html', {'person': person})

    def post(self, request, pk):
        person = get_object_or_404(Person, pk=pk)
        person.first_name = request.POST.get('first_name')
        person.last_name = request.POST.get('last_name')
        person.email = request.POST.get('email')
        person.bio = request.POST.get('bio')
        person.birth_date = request.POST.get('birth_date')

        try:
            person.save()
            return redirect('person_detail', pk=person.pk)
        except Exception as e:
            error_message = f"Error updating person: {str(e)}"
            return render(request, 'person/update.html', {'person': person, 'error_message': error_message})


class PersonDeleteView(View):
    """
    Delete an existing person.
    """
    def get(self, request, pk):
        person = get_object_or_404(Person, pk=pk)
        return render(request, 'person/delete.html', {'person': person})

    def post(self, request, pk):
        person = get_object_or_404(Person, pk=pk)

        try:
            person.delete()
            return redirect('person_list')
        except Exception as e:
            error_message = f"Error deleting person: {str(e)}"
            return render(request, 'person/detail.html', {'person': person, 'error_message': error_message})
```

### Generic class-based views Examples 📌

```python
from django.views.generic import ListView, DetailView, CreateView, UpdateView, DeleteView
from django.urls import reverse_lazy
from .models import Person

class PersonListView(ListView):
    """
    Display a list of all people.
    """
    model = Person
    template_name = 'person/list.html'
    context_object_name = 'people'
    ordering = ['-created_at']

class PersonDetailView(DetailView):
    """
    Display details of a specific person.
    """
    model = Person
    template_name = 'person/detail.html'
    context_object_name = 'person'

class PersonCreateView(CreateView):
    """
    Create a new person.
    """
    model = Person
    template_name = 'person/create.html'
    fields = ['first_name', 'last_name', 'email', 'bio', 'birth_date']
    success_url = reverse_lazy('person_list')

class PersonUpdateView(UpdateView):
    """
    Update an existing person.
    """
    model = Person
    template_name = 'person/update.html'
    fields = ['first_name', 'last_name', 'email', 'bio', 'birth_date']
    success_url = reverse_lazy('person_list')

class PersonDeleteView(DeleteView):
    """
    Delete an existing person.
    """
    model = Person
    template_name = 'person/delete.html'
    success_url = reverse_lazy('person_list')

```

### Class Based View vs Funcation Based View

When deciding between a class-based view (CBV) and a function-based view (FBV) in Django, there are a few factors to consider:

1. Complexity and Reusability:
   - CBVs provide a structured and object-oriented approach, allowing you to define common behavior in base classes and inherit from them. This can be useful when you have complex views with shared functionality or when you need to handle multiple HTTP methods.
   - FBVs are simpler and more straightforward, suitable for smaller views with less shared functionality.
2. Code Organization and Readability:
   - CBVs can provide better code organization by separating concerns into separate methods and class-based mixins. This can make it easier to understand and maintain the codebase, especially for larger projects.
   - FBVs can be concise and more readable, especially for simple views with minimal logic.
3. Familiarity and Developer Experience:
   - If you are more comfortable with object-oriented programming and class-based approaches, CBVs may be a natural fit for you.
   - If you prefer a simpler and more functional programming style, FBVs may be more suitable.
4. Django Ecosystem and Third-Party Packages:
   - Django has built-in support for both CBVs and FBVs, so you can choose based on your preference and project requirements.
   - Some third-party packages and libraries may provide additional functionality or integrations specifically for CBVs or FBVs. Checking if any of these packages align with your needs can be a deciding factor.

## Template

## URL mapping

A set of patterns that Django will try to match the requested URL to find the correct view.

### Django URL Functions

Here, we are giving some commonly used functions for URL handling and mapping.



| Name                                         | Description                                                  | Example                                          |
| :------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------- |
| path(route, view, kwargs=None, name=None)    | It returns an element for inclusion in urlpatterns.          | path('index/', views.index, name='main-view')    |
| re_path(route, view, kwargs=None, name=None) | It returns an element for inclusion in urlpatterns.          | re_path(r'^index/$', views.index, name='index'), |
| include(module, namespace=None)              | It is a function that takes a full Python import path to another URLconf module that should be "included" in this place. |                                                  |
| register_converter(converter, type_name)     | It is used for registering a converter for use in path() routes. |                                                  |

### Register Converter

In Django, you can use regular expressions to capture parts of a URL and pass them as arguments to a view function. This is called URL parameter capturing or URL parameter conversion. Here's an example of how to use the `register_converter()` function to define a custom URL parameter converter:

1. Create a new file called `converters.py` in your Django app.
2. Define a custom converter class in `converters.py`. For example, to capture a date in `YYYY-MM-DD` format:

```python
#converters.py
from datetime import date
from django.urls.converters import StringConverter

class DateConverter(StringConverter):
    regex = r'\d{4}-\d{2}-\d{2}'

    def to_python(self, value):
        return date.fromisoformat(value)

    def to_url(self, value):
        return value.isoformat()
```

This defines a new `DateConverter` class that extends Django's built-in `StringConverter` class. The `regex` attribute defines the regular expression pattern that matches a date in `YYYY-MM-DD` format. The `to_python()` method converts the captured string to a `date` object. The `to_url()` method converts a `date` object back to a string in the same format.

1. In your app's `urls.py` file, import the `register_converter()` function and your custom converter class:

```python
#urls.py
from django.urls import path, register_converter
from . import views, converters

register_converter(converters.DateConverter, 'date')
...
```

This registers your `DateConverter` class with Django and associates it with the `date` type.

1. Define a URL pattern that uses your custom converter. For example:

```python
...
urlpatterns = [
    path('events/<date:date>/', views.events_by_date, name='events_by_date'),
]
...
```

This matches URLs like `http://example.com/events/2022-05-07/`, where `2022-05-07` is a date in `YYYY-MM-DD` format. The `date` parameter is passed to the `events_by_date` view function as a `date` object.

That's the basic idea! You can define any number of custom URL parameter converters to capture different types of data in your URLs.

### Examples 📌

```python
#urls.py

from django.urls import path
from . import views

urlpatterns = [
  	# Simple URL pattern with a view function
    path('', views.home, name='home'),
  
		#URL pattern with a parameter
    path('post/<int:pk>/', views.post_detail, name='post_detail'),
  
  	#URL pattern with a regular expression READ: point 1.
  	re_path(r'^archive/(?P<year>\d{4})/(?P<month>\d{2})/$', views.archive, name='archive'),
  
  	#URL pattern with optional parameters READ: Point 2.
  	path('blog/', views.blog, name='blog'),
    path('blog/<int:year>/', views.blog_year, name='blog_year'),
    path('blog/<int:year>/<str:month>/', views.blog_month, name='blog_month'),
  
  	#URL with include READ: Point 3
	  path('myapp/', include('myapp.urls')),
]
```

**Point 1.** matches a URL with the format `http://example.com/archive/2022/05/`, `http://example.com/archive/2023/06/`, etc. The `year` and `month` parameters are passed to the `archive` function in `views.py`.

**Point 2.** This matches the URL `http://example.com/blog/`, `http://example.com/blog/2022/`, or `http://example.com/blog/2022/may/`. If the year and/or month parameters are not included in the URL, they default to `None`. These parameters are passed to the appropriate functions in `views.py`.

**Note: ** The **reverse** function **allows retrieving url details from the url's.py file through the name value provided**. 

## Useful funcations

#### `reverse()`

The `reverse()` function is used to generate URLs dynamically. It takes a view name or URL pattern name and returns a URL that can be used in templates, views, or anywhere else that URLs are needed in your application.

```python
from django.urls import reverse

id = 123
url = reverse('myapp:view_item', args=[id])
```

## Forms

Django Forms provides a way to define forms in Python code rather than writing HTML markup for forms manually. This abstraction helps in keeping the codebase clean, maintainable, and secure by handling validation, data conversion, and rendering of HTML forms automatically.

Key features of Django Forms include:

1. **Field Types:** Django Forms provides various field types, such as CharField for text input, EmailField for email addresses, BooleanField for checkboxes, and more.
2. **Validation:** Django Forms includes built-in validation for form fields, ensuring that the data entered by users meets certain criteria before it is processed by the server.
3. **Data Conversion:** When users submit a form, Django Forms automatically converts the submitted data into appropriate Python data types, handling things like type conversion and data normalization.
4. **Rendering:** Django Forms can automatically generate the HTML markup required for rendering the form, including handling labels, input fields, checkboxes, and other form elements.
5. **Security:** Django Forms helps prevent common security issues like cross-site scripting (XSS) attacks and cross-site request forgery (CSRF) attacks by including anti-CSRF tokens and other security measures.
6. **Formsets:** Django Forms allows you to work with multiple instances of a form in a single view using formsets, making it easy to manage lists of related data.

Examples 📌

```python
# form.py
from django import forms

class BookForm(forms.Form):
    book_id = forms.CharField(max_length=10, widget=forms.TextInput(attrs={'class': 'form-control', 'placeholder': 'Enter book ID'}))
    book_name = forms.CharField(max_length=100, widget=forms.TextInput(attrs={'class': 'form-control'}), help_text="Enter the title of the book")
    # book_name = forms.CharField(label="Book name", max_length=100, error_message={
    # "required": "Book name is required",
    # "max_length": "please enter shorter book length"})
    book_genre = forms.ChoiceField(choices=[('fiction', 'Fiction'), ('non-fiction', 'Non-Fiction')], widget=forms.Select(attrs={'class': 'form-control'}))
    book_published_date = forms.DateField(widget=forms.DateInput(attrs={'class': 'form-control', 'placeholder': 'YYYY-MM-DD'}), help_text="Format: YYYY-MM-DD")
    book_isbn = forms.CharField(max_length=20, widget=forms.TextInput(attrs={'class': 'form-control'}), help_text="Enter the ISBN number")
    book_author = forms.CharField(max_length=100, widget=forms.TextInput(attrs={'class': 'form-control', 'placeholder': 'Enter author name'}))
    
    def clean_book_published_date(self):
        data = self.cleaned_data['book_published_date']
        if data > timezone.now().date():
            raise forms.ValidationError("Published date cannot be in the future.")
        return data     
```

```python
# view.py
from django.shortcuts import render, redirect
from .forms import BookForm

def add_book(request):
    if request.method == 'POST':
        form = BookForm(request.POST)
        if form.is_valid():
            # Process the form data here (e.g., store it in a dictionary or list)
            book_data = {
                'book_id': form.cleaned_data['book_id'],
                'book_name': form.cleaned_data['book_name'],
                'book_genre': form.cleaned_data['book_genre'],
                'book_published_date': form.cleaned_data['book_published_date'],
                'book_isbn': form.cleaned_data['book_isbn'],
                'book_author': form.cleaned_data['book_author'],
            }
            # You can process or save the book_data as needed
            return redirect('book_list')  # Redirect to a page displaying the list of books
    else:
        form = BookForm()
    return render(request, 'add_book.html', {'form': form})

```

```html
    <form method="post">
        {% csrf_token %}
        
        {{ form.book_id.label_tag }}
        {{ form.book_id }}
        <br>
        
        {{ form.book_name.label_tag }}
        {{ form.book_name }}
        {{ form.book_name.error_message }}
        <br>
        .
        .
        <button type="submit">Add Book</button>
    </form>


```

```python
from django import forms
from .models import Book

class BookForm(forms.ModelForm):
    class Meta:
        model = Book
        fields = [
            'book_id',
            'book_name',
            'book_genre',
            'book_published_date',
            'book_isbn',
            'book_author',
        ]
        widgets = {
            'book_id': forms.TextInput(attrs={'class': 'form-control', 'placeholder': 'Enter book ID'}),
            'book_name': forms.TextInput(attrs={'class': 'form-control'}),
            'book_genre': forms.Select(attrs={'class': 'form-control'}),
            'book_published_date': forms.DateInput(attrs={'class': 'form-control', 'placeholder': 'YYYY-MM-DD'}),
            'book_isbn': forms.TextInput(attrs={'class': 'form-control'}),
            'book_author': forms.TextInput(attrs={'class': 'form-control', 'placeholder': 'Enter author name'}),
        }
        
        exclude = []
        
        labels = {
          'book_name': "your book name"
        }
        
        error_message = {
          "book_name": {
            "required": "book name is required"
            "max_lenght": "book name should be shorter "
          }
        }
```

```python
from django.views.generic.edit import CreateView
from .models import Book
from .forms import BookForm

class BookCreateView(CreateView):
    model = Book
    form_class = BookForm
    template_name = 'add_book.html'  # The name of your template
    success_url = reverse_lazy('book_list')

    def form_valid(self, form):
        return super().form_valid(form)

```

**Note:** you can customize whole error and displaying in html by adding your style in css. Get the class name by Inspect element.

### Best practices & Project structure example

#### Best practices

Certainly! Here are some best practices to follow while developing a Django project:

1. **Follow the DRY (Don't Repeat Yourself) Principle:**
   Avoid duplicating code as much as possible. If you find yourself writing similar code in multiple places, refactor it into reusable functions, methods, or mixins.

2. **Use Django's Built-in Features:**
   Django comes with many built-in features for common web development tasks such as authentication, URL routing, form handling, etc. Utilize these features instead of reinventing the wheel.

3. **Organize Your Code:**
   Follow Django's recommended project structure, which typically includes separate directories for apps, static files, templates, and configuration. This helps keep your codebase organized and maintainable.

4. **Keep Business Logic out of Views:**
   Views should be kept as thin as possible. Business logic should reside in model methods, managers, or separate modules. This keeps your code clean and makes it easier to test.

5. **Use Django Forms for Data Validation:**
   Django forms provide a convenient way to validate user input. Use them to validate both user-submitted data and data passed between different parts of your application.

6. **Optimize Database Queries:**
   Use Django's ORM efficiently to avoid unnecessary database queries. Utilize select_related() and prefetch_related() to minimize the number of queries required to fetch related objects.

7. **Handle Static and Media Files Properly:**
   Serve static files (CSS, JavaScript, etc.) using Django's built-in static file handling during development, but use a dedicated server or CDN for production. Similarly, configure proper media file handling for user uploads.

8. **Implement Security Best Practices:**
   Protect your application against common security threats such as SQL injection, cross-site scripting (XSS), cross-site request forgery (CSRF), and clickjacking. Use Django's built-in security features and libraries like django-crispy-forms for CSRF protection.

9. **Write Comprehensive Tests:**
   Use Django's testing framework to write unit tests, integration tests, and functional tests for your application. Aim for good test coverage to catch bugs early and ensure your application behaves as expected.

10. **Document Your Code:**
    Write clear and concise documentation for your project, including docstrings for functions and classes, README files, and any other relevant documentation. This makes it easier for other developers (and your future self) to understand and contribute to the project.

11. **Version Control with Git:**
    Use version control, preferably Git, to manage your codebase. Commit regularly and write meaningful commit messages. Utilize branching and merging effectively to collaborate with other developers and manage feature development.

12. **Optimize for Performance:**
    Profile your application to identify performance bottlenecks and optimize critical paths. Use caching, pagination, lazy loading, and other techniques to improve the performance of your application.

Following these best practices will help you develop a robust, maintainable, and scalable Django project.

#### Project structure example 1

```shell
project_name/
├── project_name/           (Django project directory)
│   ├── settings.py         (Main settings file)
│   ├── urls.py             (Main URL configuration)
│   └── ...
├── templates/              (HTML templates directory)
│   └── base.html           (Base template)
├── static/                 (Static files directory)
│   └── css/
│   └── js/
│   └── img/
├── media/                  (Media files directory)
├── apps/
│   ├── app1/               (Example app 1 directory)
│   │   ├── migrations/     (Database migrations)
│   │   ├── templates/      (Templates specific to app 1)
│   │   ├── static/         (Static files specific to app 1)
│   │   ├── tests.py        (Tests for app 1)
│   │   ├── views.py        (Views for app 1)
│   │   ├── models.py       (Models for app 1)
│   │   ├── serializers.py  (Serializers for app 1, if using DRF)
│   │   └── ...
│   ├── app2/               (Example app 2 directory)
│   │   ├── migrations/     (Database migrations)
│   │   ├── templates/      (Templates specific to app 2)
│   │   ├── static/         (Static files specific to app 2)
│   │   ├── tests.py        (Tests for app 2)
│   │   ├── views.py        (Views for app 2)
│   │   ├── models.py       (Models for app 2)
│   │   ├── serializers.py  (Serializers for app 2, if using DRF)
│   │   └── ...
├── api/                    (API endpoints directory)
│   ├── urls.py             (URL configuration for API)
│   ├── views.py            (Views for API endpoints)
│   ├── serializers.py      (Serializers for API, if using DRF)
│   └── ...
└── manage.py               (Django's command-line utility)

```

#### Project structure example 2

```shell
.
└── backend/
    ├── app/
    │   ├── api/
    │   │   └── v1/
    │   │       ├── serializers.py
    │   │       ├── urls.py
    │   │       └── views.py
    │   ├── migrations/
    │   │   └── __init__.py
    │   ├── static/
    │   │   └── app/
    │   │       ├── css
    │   │       ├── img
    │   │       └── js
    │   ├── templates/
    │   │   └── app
    │   ├── test/
    │   │   ├── test_models.py
    │   │   ├── test_setup.py
    │   │   └── test_views.py
    │   ├── __init__.py
    │   ├── actions.py
    │   ├── admin.py
    │   ├── apps.py
    │   ├── constants.py
    │   ├── exceptions.py
    │   ├── filters.py
    │   ├── forms.py
    │   ├── managers.py
    │   ├── messages.py
    │   ├── models.py
    │   └── views.py
    ├── backend/
    │   ├── __init__.py
    │   ├── asgi.py
    │   ├── settings.py
    │   ├── urls.py
    │   └── wsgi.py
    ├── common/
    │   ├── __init__.py
    │   ├── constants.py
    │   ├── custom_logger.py
    │   ├── exceptions.py
    │   ├── filters.py
    │   ├── generics.py
    │   ├── messages.py
    │   ├── mixins.py
    │   ├── models.py
    │   ├── pagination.py
    │   ├── permissions.py
    │   ├── swagger.py
    │   ├── utils.py
    │   └── viewsets.py
    ├── static/
    │   ├── css
    │   ├── img
    │   └── js
    ├── templates
    ├── .dockerignore
    ├── .gitignore
    ├── .sample.env
    ├── Dockerfile
    ├── docker-compose.yaml
    ├── manage.py
    └── requirements.txt
```

1. **backend/**: This is the root directory of your Django project, containing settings, routing, and other high-level configurations.

-  **settings.py**: Houses project-wide settings, including database configurations, middleware, and installed apps.
-  **urls.py**: Manages URL routing for your application.
-  **asgi.py**: Handles asynchronous requests and is used with ASGI-compatible servers, ideal for real-time applications.
-  **wsgi.py**: Handles synchronous requests and is used with WSGI-compatible servers, suitable for traditional web applications.

2. **app/**: Within this directory, you create Django apps, each with a specific purpose. Apps are reusable components that can be integrated into other projects.
   - **api/**: Subdirectory for organizing API-related code.
   - **v1/**: You can version your API by creating subdirectories for different versions.
     - **serializers.py**: Defines how data is converted to and from Python objects.
     - **views.py**: Defines your API views.
     - **urls.py**: Handles URL routing for this API version.
     - 

- **static/**: Houses static files (CSS, JavaScript, images) specific to this app.
- \- **templates/**: Contains HTML templates for rendering views.
- \- **tests/**: Location for writing unit tests for this app.

\3. **common/**: This directory is often used to store shared code, constants, utilities, and mixins.

\- **constants.py**: For constants and configuration values.

\- c**ustom_logger.py**: Custom logging configurations.

\- **exceptions.py**: Implement custom exceptions if needed.

\- **filters.py**: Custom filters for querying data.

\- **messages.py**: Store messages and error messages.

\- **models.py**: Shared models that can be reused across different apps.

\- **permissions.py**: Define custom permissions for your project.

\- **utils.py**: General-purpose utility functions.

\- **viewsets.py**: Define custom viewsets for your project.

\4. **static/**: This directory stores static files shared across the entire project.

\5. **templates/**: Templates shared across different apps or the entire project.

\6. Other Files: Various configuration and utility files like `**.dockerignore**`, `**.gitignore**`, `**Dockerfile**`, `**docker-compose.yaml**`, `**manage.py**`, and `**requirements.txt**` are crucial for managing development and deployment processes.
