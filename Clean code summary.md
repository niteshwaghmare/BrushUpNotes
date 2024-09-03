# "Clean Code" by Robert C. Martin

# 1. Meaningful Names

#### 1. **Use Intention-Revealing Names**
- Choose names that clearly describe the purpose of the variable, function, class, or module.
- Example:
  ```python
  # Bad
  d = 86400
  
  # Good
  SECONDS_IN_A_DAY = 86400
  ```

#### 2. **Avoid Disinformation**
- Avoid names that could be misleading or convey incorrect information.
- Example:
  ```python
  # Bad
  list = [1, 2, 3]  # 'list' is a built-in type in Python
  
  # Good
  numbers = [1, 2, 3]
  ```

#### 3. **Make Meaningful Distinctions**
- Ensure that similar names have distinct meanings and are used appropriately.
- Example:
  ```python
  # Bad
  data_list = ['apple', 'banana']
  info_list = ['carrot', 'date']
  
  # Good
  fruits = ['apple', 'banana']
  vegetables = ['carrot', 'date']
  ```

#### 4. **Use Pronounceable and Searchable Names**
- Choose names that are easy to pronounce and can be easily found in the codebase.
- Example:
  ```python
  # Bad
  xpy = 42
  
  # Good
  experiment_year = 42
  ```

#### 5. **Avoid Encodings and Hungarian Notation**
- Do not use prefixes or encodings to indicate types or other meta-information.
- Example:
  ```python
  # Bad
  strName = "Alice"
  intAge = 30
  
  # Good
  name = "Alice"
  age = 30
  ```

#### 6. **Class and Method Names Should Be Nouns and Verbs, Respectively**
- Class names should be nouns or noun phrases, while method names should be verbs or verb phrases.
- Example:
  ```python
  # Class name as noun
  class Account:
      def deposit(self, amount):  # Method name as verb
          pass
  
      def withdraw(self, amount):  # Method name as verb
          pass
  ```

### Examples in Python Code

```python
# Example with meaningful names
class Customer:
    def __init__(self, name, email):
        self.name = name
        self.email = email

    def send_email(self, subject, message):
        print(f"Sending email to {self.email} with subject: {subject}")

# Using the class
customer = Customer(name="John Doe", email="john.doe@example.com")
customer.send_email(subject="Welcome", message="Welcome to our service!")
```

# 2. Functions

### Functions in Python

#### 1. **Keep Functions Small and Focused**
- Functions should be short and focused on a single task. This improves readability and maintainability.
- Example:
  ```python
  # Bad
  def process_data(data):
      clean_data = [item.strip() for item in data if item]
      sorted_data = sorted(clean_data)
      processed_data = [item.lower() for item in sorted_data]
      return processed_data
  
  # Good
  def clean_data(data):
      return [item.strip() for item in data if item]
  
  def sort_data(data):
      return sorted(data)
  
  def process_data(data):
      cleaned = clean_data(data)
      sorted_cleaned = sort_data(cleaned)
      return [item.lower() for item in sorted_cleaned]
  ```

#### 2. **Do One Thing and Do It Well**
- Each function should perform a single, clear task. This makes the code easier to test and understand.
- Example:
  ```python
  # Bad
  def save_user_data(user, db):
      db.save(user)
      send_confirmation_email(user)
  
  # Good
  def save_user_to_db(user, db):
      db.save(user)
  
  def send_confirmation_email(user):
      print(f"Email sent to {user.email}")
  ```

#### 3. **Use Descriptive Names**
- Function names should clearly describe what the function does.
- Example:
  ```python
  # Bad
  def d_u(n):
      return n * 2
  
  # Good
  def double_number(number):
      return number * 2
  ```

#### 4. **Prefer Fewer Arguments**
- Limit the number of arguments a function takes. If you need many arguments, consider using a class or a data structure.
- Example:
  ```python
  # Bad
  def create_user(name, age, email, address, phone_number):
      pass
  
  # Good
  class UserInfo:
      def __init__(self, name, age, email, address, phone_number):
          self.name = name
          self.age = age
          self.email = email
          self.address = address
          self.phone_number = phone_number
  
  def create_user(user_info):
      pass
  ```

#### 5. **Avoid Flag Arguments**
- Flag arguments indicate that a function is doing more than one thing. Instead, split the function into separate functions.
- Example:
  ```python
  # Bad
  def calculate_area(shape, is_circle):
      if is_circle:
          return 3.14 * shape.radius ** 2
      else:
          return shape.width * shape.height
  
  # Good
  def calculate_circle_area(circle):
      return 3.14 * circle.radius ** 2
  
  def calculate_rectangle_area(rectangle):
      return rectangle.width * rectangle.height
  ```

#### 6. **Use Command/Query Separation**
- Functions should either perform an action (command) or return a value (query), but not both.
- Example:
  ```python
  # Bad
  def save_and_get_user(user, db):
      db.save(user)
      return db.get_user(user.id)
  
  # Good
  def save_user(user, db):
      db.save(user)
  
  def get_user(user_id, db):
      return db.get_user(user_id)
  ```

### Examples in Python Code

```python
# Example with small, focused functions
class EmailService:
    def send_email(self, recipient, subject, message):
        print(f"Sending email to {recipient} with subject: {subject}")

class UserService:
    def __init__(self, db, email_service):
        self.db = db
        self.email_service = email_service

    def save_user(self, user):
        self.db.save(user)

    def send_welcome_email(self, user):
        self.email_service.send_email(user.email, "Welcome", "Welcome to our service!")

    def register_user(self, user):
        self.save_user(user)
        self.send_welcome_email(user)

# Using the service
db = {}  # Example database
email_service = EmailService()
user_service = UserService(db, email_service)

user = {'email': 'john.doe@example.com'}
user_service.register_user(user)
```

# 3. Commenting

### Commenting in Python

#### 1. **Use Comments to Explain Why, Not What**
- Comments should provide reasoning and context for the code, not describe what the code is doing (which should be evident from the code itself).
- Example:
  ```python
  # Bad
  total = price * quantity  # Multiply price by quantity to get total
  
  # Good
  # Calculate the total cost for the given quantity of items
  total = price * quantity
  ```

#### 2. **Avoid Redundant Comments**
- Avoid comments that simply restate what the code is doing.
- Example:
  ```python
  # Bad
  x = x + 1  # Increment x by 1
  
  # Good
  x = x + 1  # Adjust x to include the latest increment based on business logic
  ```

#### 3. **Keep Comments Up-to-Date**
- Ensure comments are updated as the code changes. Outdated comments can be misleading.
- Example:
  ```python
  # Bad
  # Calculate the area of a circle
  area = width * height  # This code calculates the area of a rectangle, not a circle
  
  # Good
  # Calculate the area of a rectangle
  area = width * height
  ```

#### 4. **Use Comments to Provide Context and Clarify Complex Code**
- Use comments to explain the purpose of complex logic or any non-obvious decisions.
- Example:
  ```python
  # Bad
  if len(user_list) == 0:
      send_notification()
  
  # Good
  # If there are no users in the list, send a notification to the admin
  if len(user_list) == 0:
      send_notification()
  ```

### Examples in Python Code

```python
class ShoppingCart:
    def __init__(self):
        self.items = []
    
    def add_item(self, item):
        # Adding an item to the shopping cart
        self.items.append(item)
    
    def calculate_total(self):
        # Calculate the total price of all items in the cart
        return sum(item.price for item in self.items)
    
    def apply_discount(self, discount):
        # Apply a discount to each item in the cart
        for item in self.items:
            item.price -= item.price * discount

    def checkout(self):
        # Placeholder for checkout logic
        # Here we might connect to a payment gateway
        pass

# Example usage of ShoppingCart
cart = ShoppingCart()
cart.add_item(Item('Apple', 1.0))
cart.add_item(Item('Banana', 0.5))

# Apply a 10% discount to all items
cart.apply_discount(0.1)

# Calculate the total price after discount
total = cart.calculate_total()
print(f"Total price: {total}")
```

### Checklist for Commenting in Python

- [ ] **Explain Why, Not What**: Ensure comments provide reasoning and context, not just descriptions of the code.
- [ ] **Avoid Redundancy**: Do not write comments that merely restate the code.
- [ ] **Update Regularly**: Keep comments up-to-date with the code.
- [ ] **Clarify Complex Logic**: Use comments to explain complex or non-obvious parts of the code.
- [ ] **Be Clear and Concise**: Comments should be clear, concise, and to the point.
- [ ] **Avoid Over-Commenting**: Do not clutter the code with too many comments. Only comment where necessary.



# 4. Formatting

#### 1. **Use Consistent Formatting**
- Consistency in formatting makes code easier to read and maintain.
- Example:
  ```python
  # Bad - Inconsistent formatting
  def my_function(x, y): return x+y
  z = my_function(3, 4)
  
  # Good - Consistent formatting
  def my_function(x, y):
      return x + y
  
  z = my_function(3, 4)
  ```

#### 2. **Ensure Code is Easy to Read and Navigate**
- Use clear and readable formatting practices to enhance code readability.
- Example:
  ```python
  # Bad - Hard to read and navigate
  def complex_function(var1,var2,var3):
  result= var1*var2-var3
  return result
  
  # Good - Clear and readable
  def complex_function(var1, var2, var3):
      result = var1 * var2 - var3
      return result
  ```

#### 3. **Follow Standard Conventions**
- Adhere to Python's PEP 8 style guide for consistent formatting conventions.
- Example:
  ```python
  # Bad - Non-PEP 8 compliant
  if True :
  print('Hello World' )
  
  # Good - PEP 8 compliant
  if True:
      print('Hello World')
  ```

#### 4. **Use Proper Indentation and Whitespace**
- Use 4 spaces per indentation level for consistent and readable code.
- Example:
  ```python
  # Bad - Inconsistent indentation
  def function():
  print('Hello')
    print('World')
  
  # Good - Proper indentation
  def function():
      print('Hello')
      print('World')
  ```

### Examples in Python Code

```python
# Example of well-formatted Python code
def calculate_square_area(side_length):
    """Calculate the area of a square."""
    return side_length ** 2

def calculate_circle_area(radius):
    """Calculate the area of a circle."""
    return 3.14 * radius ** 2

side_length = 5
radius = 3

square_area = calculate_square_area(side_length)
circle_area = calculate_circle_area(radius)

print(f"Square area: {square_area}")
print(f"Circle area: {circle_area}")
```

### Checklist for Formatting in Python

- [ ] **Consistent Formatting**: Ensure code follows consistent formatting throughout.
- [ ] **Readability**: Make sure code is easy to read and navigate.
- [ ] **PEP 8 Compliance**: Follow Python's style guide (PEP 8) for conventions like indentation and whitespace.
- [ ] **Indentation**: Use 4 spaces per indentation level.
- [ ] **Whitespace**: Use whitespace effectively to enhance readability.



# 5. Error Handling

#### 1. **Use Exceptions Rather Than Return Codes**
- Python encourages the use of exceptions to handle errors rather than relying on return codes.
- Example:
  ```python
  # Bad - Using return codes
  def divide(x, y):
      if y == 0:
          return None  # Return code indicating division by zero
      return x / y
  
  result = divide(10, 0)
  if result is None:
      print("Division by zero error")
  
  # Good - Using exceptions
  def divide(x, y):
      if y == 0:
          raise ZeroDivisionError("Cannot divide by zero")
      return x / y
  
  try:
      result = divide(10, 0)
  except ZeroDivisionError as e:
      print("Error:", e)
  ```

#### 2. **Write try-except-finally Blocks Properly**
- Ensure proper structure and handling in try-except-finally blocks.
- Example:
  ```python
  # Bad - Incorrect try-except-finally usage
  try:
      result = some_function()
  finally:
      cleanup()
  
  # Good - Proper try-except-finally usage
  try:
      result = some_function()
  except SomeException as e:
      handle_exception(e)
  finally:
      cleanup()
  ```

#### 3. **Don't Ignore Exceptions**
- Always handle exceptions appropriately rather than ignoring them.
- Example:
  ```python
  # Bad - Ignoring exceptions
  try:
      process_data()
  except:
      pass  # Ignores all exceptions
  
  # Good - Handle specific exceptions
  try:
      process_data()
  except ValueError as e:
      print("ValueError occurred:", e)
  except Exception as e:
      print("An error occurred:", e)
  ```

#### 4. **Provide Context with Error Messages**
- Error messages should provide enough context to understand the cause of the error.
- Example:
  ```python
  # Bad - Insufficient error message
  def fetch_data(url):
      try:
          response = requests.get(url)
          return response.json()
      except requests.exceptions.RequestException:
          return None
  
  data = fetch_data("https://example.com/api")
  if data is None:
      print("Error fetching data")
  
  # Good - Informative error message
  def fetch_data(url):
      try:
          response = requests.get(url)
          response.raise_for_status()  # Raise an exception for HTTP errors
          return response.json()
      except requests.exceptions.RequestException as e:
          print(f"Error fetching data from {url}: {e}")
          return None
  
  data = fetch_data("https://example.com/api")
  if data is None:
      print("Failed to fetch data from the API")
  ```

### Examples in Python Code

```python
import requests

# Example of proper error handling
def fetch_data(url):
    try:
        response = requests.get(url)
        response.raise_for_status()  # Raise an exception for HTTP errors
        return response.json()
    except requests.exceptions.RequestException as e:
        print(f"Error fetching data from {url}: {e}")
        return None

data = fetch_data("https://example.com/api")
if data is None:
    print("Failed to fetch data from the API")
```

### Checklist for Error Handling in Python

- [ ] **Use Exceptions**: Prefer exceptions over return codes for error handling.
- [ ] **Proper try-except-finally**: Ensure correct structure and usage of try-except-finally blocks.
- [ ] **Handle Exceptions**: Never ignore exceptions; handle them appropriately.
- [ ] **Error Messages**: Provide informative error messages with context.



# 6. Objects and Data Structures

#### 1. **Encapsulate Behavior with Data**
- Encapsulate related behavior and data into classes to promote clear and understandable code.
- Example:
  ```python
  # Example of encapsulating behavior with data
  class Rectangle:
      def __init__(self, width, height):
          self.width = width
          self.height = height
      
      def calculate_area(self):
          return self.width * self.height
  
  # Usage of the Rectangle class
  rectangle = Rectangle(5, 3)
  area = rectangle.calculate_area()
  print(f"Area of rectangle: {area}")
  ```

#### 2. **Use Data Structures for Simple Data Aggregation**
- Use built-in data structures like lists, tuples, and dictionaries to aggregate simple data.
- Example:
  ```python
  # Example of using a dictionary for simple data aggregation
  student = {
      'name': 'Alice',
      'age': 25,
      'courses': ['Math', 'Science']
  }
  
  print(student['name'])  # Accessing data
  ```

#### 3. **Separate Object Interfaces from Implementation**
- Define clear interfaces for objects, hiding implementation details where possible to reduce complexity and dependencies.
- Example:
  ```python
  # Example of separating object interface from implementation
  class Car:
      def __init__(self, make, model):
          self.make = make
          self.model = model
      
      def start(self):
          print(f"Starting {self.make} {self.model}")
  
  # Usage of the Car class
  car = Car('Toyota', 'Camry')
  car.start()
  ```

### Examples in Python Code

```python
# Example of encapsulating behavior with data
class Circle:
    def __init__(self, radius):
        self.radius = radius
    
    def calculate_area(self):
        return 3.14 * self.radius ** 2

# Example of using a list for simple data aggregation
students = ['Alice', 'Bob', 'Charlie']

# Example of separating object interface from implementation
class Animal:
    def make_sound(self):
        raise NotImplementedError("Subclass must implement make_sound method")

# Example usage of Animal class
class Dog(Animal):
    def make_sound(self):
        print("Woof!")

dog = Dog()
dog.make_sound()
```

### Checklist for Objects and Data Structures in Python

- [ ] **Encapsulate Behavior**: Use classes to encapsulate related behavior and data.
- [ ] **Use Built-in Data Structures**: Utilize lists, tuples, and dictionaries for simple data aggregation.
- [ ] **Separate Interface from Implementation**: Define clear interfaces for objects, hiding implementation details where possible.

### Objects and Data Structures in Python

#### 1. **Encapsulate Behavior with Data**
- Encapsulate related behavior and data into classes to promote clear and understandable code.
- Example:
  ```python
  # Example of encapsulating behavior with data
  class Rectangle:
      def __init__(self, width, height):
          self.width = width
          self.height = height
      
      def calculate_area(self):
          return self.width * self.height
  
  # Usage of the Rectangle class
  rectangle = Rectangle(5, 3)
  area = rectangle.calculate_area()
  print(f"Area of rectangle: {area}")
  ```

#### 2. **Use Data Structures for Simple Data Aggregation**
- Use built-in data structures like lists, tuples, and dictionaries to aggregate simple data.
- Example:
  ```python
  # Example of using a dictionary for simple data aggregation
  student = {
      'name': 'Alice',
      'age': 25,
      'courses': ['Math', 'Science']
  }
  
  print(student['name'])  # Accessing data
  ```

#### 3. **Separate Object Interfaces from Implementation**
- Define clear interfaces for objects, hiding implementation details where possible to reduce complexity and dependencies.
- Example:
  ```python
  # Example of separating object interface from implementation
  class Car:
      def __init__(self, make, model):
          self.make = make
          self.model = model
      
      def start(self):
          print(f"Starting {self.make} {self.model}")
  
  # Usage of the Car class
  car = Car('Toyota', 'Camry')
  car.start()
  ```

### Examples in Python Code

```python
# Example of encapsulating behavior with data
class Circle:
    def __init__(self, radius):
        self.radius = radius
    
    def calculate_area(self):
        return 3.14 * self.radius ** 2

# Example of using a list for simple data aggregation
students = ['Alice', 'Bob', 'Charlie']

# Example of separating object interface from implementation
class Animal:
    def make_sound(self):
        raise NotImplementedError("Subclass must implement make_sound method")

# Example usage of Animal class
class Dog(Animal):
    def make_sound(self):
        print("Woof!")

dog = Dog()
dog.make_sound()
```

### Checklist for Objects and Data Structures in Python

- [ ] **Encapsulate Behavior**: Use classes to encapsulate related behavior and data.
- [ ] **Use Built-in Data Structures**: Utilize lists, tuples, and dictionaries for simple data aggregation.
- [ ] **Separate Interface from Implementation**: Define clear interfaces for objects, hiding implementation details where possible.



# 7. Testing

#### 1. **Write Tests for All Code**
- Ensure that all critical parts of your codebase are covered by automated tests.
- Example:
  ```python
  # Example of a test function using pytest
  def test_addition():
      assert 1 + 2 == 3
  
  def test_subtraction():
      assert 5 - 3 == 2
  ```

#### 2. **Use Test-Driven Development (TDD) Principles**
- Write tests before writing the code to implement the functionality.
- Example:
  ```python
  # Example of TDD approach using pytest
  def test_multiply():
      assert multiply(3, 4) == 12
  ```

#### 3. **Ensure Tests Are Fast, Independent, Repeatable, and Self-Validating**
- Tests should execute quickly, not depend on each other, produce the same results every time, and automatically verify their correctness.
- Example:
  ```python
  # Example of a self-validating test using pytest
  def test_division():
      result = divide(10, 2)
      assert result == 5, "Division result incorrect"
  ```

#### 4. **Maintain a Clean and Organized Test Codebase**
- Keep your test codebase well-structured, readable, and easy to maintain.
- Example:
  ```python
  # Example of organized test functions using pytest
  def test_addition():
      assert add(1, 2) == 3
  
  def test_subtraction():
      assert subtract(5, 3) == 2
  ```

### Examples in Python Code (using pytest)

```python
# Example of a module with functions to test
def add(x, y):
    return x + y

def subtract(x, y):
    return x - y

# Example of corresponding test functions
def test_addition():
    assert add(1, 2) == 3

def test_subtraction():
    assert subtract(5, 3) == 2
```

### Checklist for Testing in Python

- [ ] **Write Comprehensive Tests**: Ensure all critical parts of the codebase are covered by tests.
- [ ] **Use TDD Principles**: Write tests before writing code to implement functionality.
- [ ] **Test Characteristics**: Ensure tests are fast, independent, repeatable, and self-validating.
- [ ] **Organize Test Codebase**: Maintain a clean and organized structure for test functions and suites.



# 8. Classes

#### 1. **Keep Classes Small and Focused**
- Classes should have a clear and focused purpose, avoiding excessive complexity.
- Example:
  ```python
  # Example of a class with single responsibility
  class EmailSender:
      def __init__(self, smtp_server):
          self.smtp_server = smtp_server
      
      def send_email(self, recipient, subject, message):
          # Code to send email using smtp_server
          print(f"Sending email to {recipient} with subject: {subject}")
  ```

#### 2. **Follow the Single Responsibility Principle (SRP)**
- Each class should have only one reason to change, adhering to the SRP for maintainability.
- Example:
  ```python
  # Example of adhering to SRP
  class FileParser:
      def __init__(self, file_path):
          self.file_path = file_path
      
      def parse(self):
          # Code to parse the file
          pass
  
  class DatabaseSaver:
      def __init__(self, db_connection):
          self.db_connection = db_connection
      
      def save_to_db(self, data):
          # Code to save data to database
          pass
  ```

#### 3. **Use Descriptive Class Names**
- Choose meaningful and descriptive names for classes that accurately convey their purpose.
- Example:
  ```python
  # Example of using a descriptive class name
  class CustomerDatabase:
      # Class implementation
      pass
  ```

#### 4. **Prefer Composition Over Inheritance**
- Favor composition (using objects as members) over inheritance (creating sub-classes) to achieve code reuse and flexibility.
- Example:
  ```python
  # Example of using composition
  class Engine:
      def start(self):
          print("Engine started")
  
  class Car:
      def __init__(self):
          self.engine = Engine()
      
      def start(self):
          self.engine.start()
  ```

### Examples in Python Code

```python
# Example of a well-structured and focused class
class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def calculate_area(self):
        return self.width * self.height

# Example of adhering to SRP and using descriptive class names
class EmailSender:
    def __init__(self, smtp_server):
        self.smtp_server = smtp_server
    
    def send_email(self, recipient, subject, message):
        # Code to send email using smtp_server
        print(f"Sending email to {recipient} with subject: {subject}")

# Example of using composition over inheritance
class Engine:
    def start(self):
        print("Engine started")

class Car:
    def __init__(self):
        self.engine = Engine()
    
    def start(self):
        self.engine.start()

# Using the classes
rectangle = Rectangle(5, 3)
area = rectangle.calculate_area()
print(f"Area of rectangle: {area}")

email_sender = EmailSender("smtp.example.com")
email_sender.send_email("recipient@example.com", "Hello", "This is a test email")

car = Car()
car.start()
```

### Checklist for Classes in Python

- [ ] **Keep Classes Focused**: Ensure classes have a clear and focused purpose.
- [ ] **Follow SRP**: Adhere to the Single Responsibility Principle to maintain class clarity and manageability.
- [ ] **Descriptive Class Names**: Use meaningful and descriptive names for classes.
- [ ] **Prefer Composition**: Favor composition over inheritance for improved code reuse and flexibility.



# 9. System Design Principles

#### 1. **Keep Systems Decoupled and Modular**
- Design systems to have independent components that communicate through well-defined interfaces, promoting flexibility and maintainability.
- Example:
  ```python
  # Example of decoupled and modular system design
  class DataService:
      def __init__(self, db_connection):
          self.db_connection = db_connection
      
      def fetch_data(self, query):
          # Code to fetch data from database
          pass
  
  class BusinessLogic:
      def __init__(self, data_service):
          self.data_service = data_service
      
      def process_data(self, data):
          # Code to process data
          pass
  
  class Controller:
      def __init__(self, business_logic):
          self.business_logic = business_logic
      
      def handle_request(self, request):
          data = self.business_logic.process_data(request)
          # Code to handle request based on processed data
          return data
  ```

#### 2. **Follow SOLID Principles**

##### a. **Single Responsibility Principle (SRP)**
- Each class or module should have only one reason to change, focusing on a single responsibility.
- Example:
  ```python
  # Example of SRP
  class FileParser:
      def __init__(self, file_path):
          self.file_path = file_path
      
      def parse(self):
          # Code to parse the file
          pass
  
  class DatabaseSaver:
      def __init__(self, db_connection):
          self.db_connection = db_connection
      
      def save_to_db(self, data):
          # Code to save data to database
          pass
  ```

##### b. **Open/Closed Principle (OCP)**
- Classes should be open for extension but closed for modification, allowing behavior to be extended without altering existing code.
- Example:
  ```python
  # Example of OCP using inheritance
  class Shape:
      def area(self):
          raise NotImplementedError("Subclass must implement area method")
  
  class Rectangle(Shape):
      def __init__(self, width, height):
          self.width = width
          self.height = height
      
      def area(self):
          return self.width * self.height
  
  class Circle(Shape):
      def __init__(self, radius):
          self.radius = radius
      
      def area(self):
          return 3.14 * self.radius ** 2
  ```

##### c. **Liskov Substitution Principle (LSP)**
- Subtypes should be substitutable for their base types without affecting the correctness of the program.
- Example:
  ```python
  # Example of LSP
  class Bird:
      def fly(self):
          raise NotImplementedError("Subclass must implement fly method")
  
  class Sparrow(Bird):
      def fly(self):
          print("Sparrow flying")
  
  class Ostrich(Bird):
      def fly(self):
          raise AttributeError("Ostriches cannot fly")
  ```

##### d. **Interface Segregation Principle (ISP)**
- Clients should not be forced to depend on interfaces they do not use. Split interfaces into smaller, specific ones.
- Example:
  ```python
  # Example of ISP
  class Printer:
      def print(self, document):
          raise NotImplementedError("Subclass must implement print method")
  
  class Scanner:
      def scan(self, document):
          raise NotImplementedError("Subclass must implement scan method")
  
  class Photocopier(Printer, Scanner):
      def copy(self, document):
          print("Copying", document)
  ```

##### e. **Dependency Inversion Principle (DIP)**
- High-level modules should not depend on low-level modules. Both should depend on abstractions (e.g., interfaces).
- Example:
  ```python
  # Example of DIP
  class DBConnection:
      def execute_query(self, query):
          raise NotImplementedError("Subclass must implement execute_query method")
  
  class MySQLConnection(DBConnection):
      def execute_query(self, query):
          print("Executing MySQL query:", query)
  
  class PostgreSQLConnection(DBConnection):
      def execute_query(self, query):
          print("Executing PostgreSQL query:", query)
  ```

### Examples in Python Code

```python
# Example of following SOLID principles in Python

# SRP example
class FileParser:
    def __init__(self, file_path):
        self.file_path = file_path
    
    def parse(self):
        # Code to parse the file
        pass

class DatabaseSaver:
    def __init__(self, db_connection):
        self.db_connection = db_connection
    
    def save_to_db(self, data):
        # Code to save data to database
        pass

# OCP example using inheritance
class Shape:
    def area(self):
        raise NotImplementedError("Subclass must implement area method")

class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def area(self):
        return self.width * self.height

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius
    
    def area(self):
        return 3.14 * self.radius ** 2

# LSP example
class Bird:
    def fly(self):
        raise NotImplementedError("Subclass must implement fly method")

class Sparrow(Bird):
    def fly(self):
        print("Sparrow flying")

class Ostrich(Bird):
    def fly(self):
        raise AttributeError("Ostriches cannot fly")

# ISP example
class Printer:
    def print(self, document):
        raise NotImplementedError("Subclass must implement print method")

class Scanner:
    def scan(self, document):
        raise NotImplementedError("Subclass must implement scan method")

class Photocopier(Printer, Scanner):
    def copy(self, document):
        print("Copying", document)

# DIP example
class DBConnection:
    def execute_query(self, query):
        raise NotImplementedError("Subclass must implement execute_query method")

class MySQLConnection(DBConnection):
    def execute_query(self, query):
        print("Executing MySQL query:", query)

class PostgreSQLConnection(DBConnection):
    def execute_query(self, query):
        print("Executing PostgreSQL query:", query)
```

### Checklist for System Design in Python

- [ ] **Decoupled and Modular Systems**: Design systems with independent components communicating through well-defined interfaces.
- [ ] **Follow SOLID Principles**:
  - [ ] **SRP**: Ensure classes have a single responsibility.
  - [ ] **OCP**: Classes should be open for extension but closed for modification.
  - [ ] **LSP**: Subtypes should be substitutable for their base types.
  - [ ] **ISP**: Split interfaces into smaller, specific ones.
  - [ ] **DIP**: High-level modules should not depend on low-level modules; depend on abstractions.



# 10. Concurrency

#### 1. **Keep Concurrent Code Simple**
- Write straightforward concurrent code to minimize complexity and improve readability.
- Example:
  ```python
  # Example of simple concurrent code using threading
  import threading
  
  def count_up():
      for i in range(5):
          print(f"Counting up: {i}")
  
  def count_down():
      for i in range(5, 0, -1):
          print(f"Counting down: {i}")
  
  # Create and start threads
  thread1 = threading.Thread(target=count_up)
  thread2 = threading.Thread(target=count_down)
  thread1.start()
  thread2.start()
  
  # Wait for threads to complete
  thread1.join()
  thread2.join()
  ```

#### 2. **Use Locks and Synchronization Mechanisms Appropriately**
- Employ locks and other synchronization mechanisms to control access to shared resources and prevent race conditions.
- Example:
  ```python
  # Example of using locks for synchronization
  import threading
  
  shared_resource = []
  lock = threading.Lock()
  
  def append_to_shared(value):
      with lock:
          shared_resource.append(value)
  
  # Create threads that modify shared resource
  thread1 = threading.Thread(target=append_to_shared, args=(1,))
  thread2 = threading.Thread(target=append_to_shared, args=(2,))
  
  thread1.start()
  thread2.start()
  
  thread1.join()
  thread2.join()
  
  print("Shared resource:", shared_resource)
  ```

#### 3. **Avoid Shared Mutable Data**
- Minimize or avoid shared mutable data structures to simplify concurrent programming and reduce potential for errors.
- Example:
  ```python
  # Example of avoiding shared mutable data
  import threading
  
  shared_data = threading.local()
  
  def set_thread_local(value):
      shared_data.value = value
  
  def get_thread_local():
      return getattr(shared_data, 'value', None)
  
  # Create threads with local data
  thread1 = threading.Thread(target=set_thread_local, args=(1,))
  thread2 = threading.Thread(target=set_thread_local, args=(2,))
  
  thread1.start()
  thread2.start()
  
  thread1.join()
  thread2.join()
  
  print("Thread 1 local value:", get_thread_local())
  print("Thread 2 local value:", get_thread_local())
  ```

#### 4. **Use Threads Only When Necessary**
- Consider using higher-level abstractions like multiprocessing or asynchronous programming (asyncio) depending on the specific concurrency requirements.
- Example:
  ```python
  # Example of using multiprocessing for CPU-bound tasks
  from multiprocessing import Process
  
  def calculate_square(number):
      result = number ** 2
      print(f"Square of {number} is {result}")
  
  # Create processes for parallel execution
  processes = []
  for i in range(1, 6):
      process = Process(target=calculate_square, args=(i,))
      processes.append(process)
      process.start()
  
  for process in processes:
      process.join()
  ```

### Examples in Python Code

```python
# Example of simple concurrent code using threading
import threading

def count_up():
    for i in range(5):
        print(f"Counting up: {i}")

def count_down():
    for i in range(5, 0, -1):
        print(f"Counting down: {i}")

# Create and start threads
thread1 = threading.Thread(target=count_up)
thread2 = threading.Thread(target=count_down)
thread1.start()
thread2.start()

# Wait for threads to complete
thread1.join()
thread2.join()

# Example of using locks for synchronization
import threading

shared_resource = []
lock = threading.Lock()

def append_to_shared(value):
    with lock:
        shared_resource.append(value)

# Create threads that modify shared resource
thread1 = threading.Thread(target=append_to_shared, args=(1,))
thread2 = threading.Thread(target=append_to_shared, args=(2,))

thread1.start()
thread2.start()

thread1.join()
thread2.join()

print("Shared resource:", shared_resource)

# Example of avoiding shared mutable data
import threading

shared_data = threading.local()

def set_thread_local(value):
    shared_data.value = value

def get_thread_local():
    return getattr(shared_data, 'value', None)

# Create threads with local data
thread1 = threading.Thread(target=set_thread_local, args=(1,))
thread2 = threading.Thread(target=set_thread_local, args=(2,))

thread1.start()
thread2.start()

thread1.join()
thread2.join()

print("Thread 1 local value:", get_thread_local())
print("Thread 2 local value:", get_thread_local())
```

### Checklist for Concurrency in Python

- [ ] **Keep Code Simple**: Write straightforward concurrent code to enhance readability.
- [ ] **Use Synchronization**: Employ locks and synchronization mechanisms to manage shared resources.
- [ ] **Avoid Shared Mutable Data**: Minimize or eliminate shared mutable data to prevent concurrency issues.
- [ ] **Choose Appropriate Concurrency Model**: Use threads, multiprocessing, or asynchronous programming depending on the application's concurrency requirements.



# 11. Code Smells

#### 1. **Duplicated Code**
- Identical or very similar code fragments repeated throughout the codebase.
- **Refactoring Tip**: Extract common functionality into functions or methods to reduce redundancy.
  
#### 2. **Long Methods**
- Methods or functions that are excessively long, making them hard to understand and maintain.
- **Refactoring Tip**: Break down long methods into smaller, focused methods that perform specific tasks.

#### 3. **Large Classes**
- Classes that have grown too large, often handling too many responsibilities.
- **Refactoring Tip**: Split large classes into smaller, cohesive classes that handle distinct responsibilities.

#### 4. **Long Parameter Lists**
- Functions or methods with a large number of parameters, which can be confusing and error-prone to use.
- **Refactoring Tip**: Group related parameters into a single object or use named arguments to improve clarity.

#### 5. **Divergent Change**
- Classes that are frequently modified for different reasons (e.g., different features or requirements).
- **Refactoring Tip**: Apply the Single Responsibility Principle (SRP) to ensure each class has a single, clear responsibility.

#### 6. **Shotgun Surgery**
- Modifications that require changes to multiple classes or methods in different parts of the codebase.
- **Refactoring Tip**: Consolidate related functionality into cohesive units (e.g., classes, modules) to minimize scattered changes.

#### 7. **Feature Envy**
- Methods or functions that excessively use or manipulate data from another class, indicating a potential design issue.
- **Refactoring Tip**: Move methods to the class where the data resides (encapsulation) or use delegation to improve cohesion.

### Examples in Python Code

```python
# Example demonstrating code smells and potential refactorings

# Duplicated code
def calculate_area_rectangle(width, height):
    return width * height

def calculate_area_square(side):
    return side * side

# Refactoring: Extract common functionality
def calculate_area(width_or_side, height=None):
    if height:
        return width_or_side * height
    else:
        return width_or_side * width_or_side

# Long methods
class User:
    def __init__(self, name, age, address):
        self.name = name
        self.age = age
        self.address = address
    
    # Long method
    def print_user_details(self):
        print(f"Name: {self.name}, Age: {self.age}, Address: {self.address}")

# Refactoring: Split into smaller methods
class User:
    def __init__(self, name, age, address):
        self.name = name
        self.age = age
        self.address = address
    
    def print_name(self):
        print(f"Name: {self.name}")
    
    def print_age(self):
        print(f"Age: {self.age}")
    
    def print_address(self):
        print(f"Address: {self.address}")

# Large classes
class Customer:
    def __init__(self, name, address):
        self.name = name
        self.address = address
        self.orders = []
        self.payments = []
        self.preferences = []

    # Methods handling different responsibilities can be split into smaller classes

# Long parameter lists
def process_order(order_id, customer_id, product_id, quantity, price, discount, shipping_address):
    # Function with a long parameter list

# Refactoring: Group related parameters into objects or use named arguments
class Order:
    def __init__(self, order_id, customer_id, product_id, quantity, price, discount, shipping_address):
        self.order_id = order_id
        self.customer_id = customer_id
        self.product_id = product_id
        self.quantity = quantity
        self.price = price
        self.discount = discount
        self.shipping_address = shipping_address

    def process_order(self):
        # Method to process order using encapsulated data

# Divergent change
class PaymentProcessor:
    def process_credit_card_payment(self, amount):
        # Method for credit card payments

    def process_paypal_payment(self, amount):
        # Method for PayPal payments

# Refactoring: Separate payment processing into distinct classes based on payment methods

# Shotgun surgery
class ShoppingCart:
    def add_to_cart(self, product_id):
        # Method to add product to cart

    def update_cart_quantity(self, product_id, quantity):
        # Method to update quantity in cart

# Refactoring: Consolidate cart-related functionality into a ShoppingCart class

# Feature envy
class Customer:
    def __init__(self, name, email):
        self.name = name
        self.email = email
        self.orders = []

class Order:
    def __init__(self, order_id, customer):
        self.order_id = order_id
        self.customer = customer
    
    def print_customer_details(self):
        # Feature envy: Accessing customer details excessively
        print(f"Customer Name: {self.customer.name}, Email: {self.customer.email}")

# Refactoring: Move customer-related methods into Customer class or use delegation
```

### Checklist for Code Smells in Python

- [ ] **Duplicated Code**: Identify and refactor repeated code fragments.
- [ ] **Long Methods**: Break down long methods into smaller, focused methods.
- [ ] **Large Classes**: Split large classes into smaller, cohesive classes.
- [ ] **Long Parameter Lists**: Group related parameters into objects or use named arguments.
- [ ] **Divergent Change**: Apply SRP to ensure classes have a single responsibility.
- [ ] **Shotgun Surgery**: Consolidate related functionality to minimize scattered changes.
- [ ] **Feature Envy**: Move methods to the class where the data resides or use delegation for better encapsulation.



# 📌 Bullets points 

#### 1. **Meaningful Names**

- Use intention-revealing names.
- Avoid disinformation.
- Make meaningful distinctions.
- Use pronounceable and searchable names.
- Avoid encodings and Hungarian notation.
- Class and method names should be nouns and verbs, respectively.

#### 2. **Functions**
- Keep functions small and focused.
- Do one thing and do it well.
- Use descriptive names.
- Prefer fewer arguments.
- Avoid flag arguments.
- Use command/query separation.

#### 3. **Comments**
- Comments are not a substitute for clear code.
- Use comments to explain why, not what.
- Avoid redundant and misleading comments.
- Keep comments up-to-date.

#### 4. **Formatting**
- Use consistent formatting.
- Ensure code is easy to read and navigate.
- Follow standard conventions.
- Use proper indentation and whitespace.

#### 5. **Error Handling**
- Use exceptions rather than return codes.
- Write try-catch-finally blocks properly.
- Don't ignore exceptions.
- Provide context with error messages.

#### 6. **Objects and Data Structures**
- Encapsulate behavior with data.
- Use data structures for simple data aggregation.
- Separate object interfaces from implementation.

#### 7. **Testing**
- Write tests for all code.
- Use TDD (Test-Driven Development) principles.
- Ensure tests are fast, independent, repeatable, and self-validating.
- Maintain a clean and organized test codebase.

#### 8. **Classes**
- Keep classes small and focused.
- Follow the Single Responsibility Principle (SRP).
- Use descriptive class names.
- Prefer composition over inheritance.

#### 9. **System Design**
- Keep systems decoupled and modular.
- Follow SOLID principles:
  - **S**: Single Responsibility Principle
  - **O**: Open/Closed Principle
  - **L**: Liskov Substitution Principle
  - **I**: Interface Segregation Principle
  - **D**: Dependency Inversion Principle

#### 10. **Concurrency**
- Keep concurrent code simple.
- Use locks and synchronization mechanisms appropriately.
- Avoid shared mutable data.
- Use threads only when necessary.

#### 11. **Code Smells**
- Watch out for bad smells in code:
  - Duplicated code
  - Long methods
  - Large classes
  - Long parameter lists
  - Divergent change
  - Shotgun surgery
  - Feature envy

# ✅ Clean Code Checklist/Cheat Sheet

#### Naming Conventions
- [ ] Use intention-revealing names.
- [ ] Avoid disinformation and redundant names.
- [ ] Use pronounceable and searchable names.
- [ ] Avoid encodings (e.g., Hungarian notation).
- [ ] Class names should be nouns; method names should be verbs.

#### Function Design
- [ ] Functions should be small and do one thing.
- [ ] Use descriptive names for functions.
- [ ] Limit the number of arguments in functions.
- [ ] Avoid flag arguments.
- [ ] Separate commands from queries.

#### Commenting
- [ ] Use comments to explain why, not what.
- [ ] Avoid redundant comments.
- [ ] Keep comments up-to-date.
- [ ] Use comments to provide context and clarify complex code.

#### Formatting
- [ ] Use consistent and readable formatting.
- [ ] Follow standard conventions for indentation and whitespace.
- [ ] Ensure code is easy to read and navigate.

#### Error Handling
- [ ] Use exceptions instead of return codes.
- [ ] Properly structure try-catch-finally blocks.
- [ ] Don't ignore exceptions.
- [ ] Provide informative error messages.

#### Objects and Data Structures
- [ ] Encapsulate behavior with data.
- [ ] Use data structures for simple data aggregation.
- [ ] Separate object interfaces from implementation details.

#### Testing
- [ ] Write tests for all code.
- [ ] Follow TDD principles.
- [ ] Ensure tests are fast, independent, repeatable, and self-validating.
- [ ] Keep the test codebase clean and organized.

#### Class Design
- [ ] Keep classes small and focused.
- [ ] Follow the Single Responsibility Principle (SRP).
- [ ] Use descriptive class names.
- [ ] Prefer composition over inheritance.

#### System Design
- [ ] Keep systems decoupled and modular.
- [ ] Follow SOLID principles:
  - [ ] Single Responsibility Principle (SRP)
  - [ ] Open/Closed Principle (OCP)
  - [ ] Liskov Substitution Principle (LSP)
  - [ ] Interface Segregation Principle (ISP)
  - [ ] Dependency Inversion Principle (DIP)

#### Concurrency
- [ ] Keep concurrent code simple.
- [ ] Use locks and synchronization mechanisms appropriately.
- [ ] Avoid shared mutable data.
- [ ] Use threads only when necessary.

#### Code Smells
- [ ] Avoid duplicated code.
- [ ] Keep methods short and focused.
- [ ] Keep classes small and cohesive.
- [ ] Limit the number of parameters in methods.
- [ ] Avoid divergent change and shotgun surgery.
- [ ] Avoid feature envy and ensure proper encapsulation.

This checklist can be used as a quick reference to ensure that your code adheres to the principles of clean code.