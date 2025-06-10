# Design Principles

## SOLID Principles

The SOLID principles provide a blueprint for writing code that’s easy to adjust, extend, and maintain over time.

It was introduced by Robert C. Martin (Uncle Bob) in the early 2000s.


### S: Single responsibility principle (SRP)
> A class should have one, and only one, reason to change.

This means that a class must have only one responsibility.

When a class performs just one task, it contains a small number of methods and member variables making them more usable and easier to maintain.

If a class has multiple responsibilities, it becomes harder to understand, maintain, and modify and increases the potential for bugs because changes to one responsibility could affect the others.

**Code Example:**
\
Imagine you have a class called UserManager that handles user authentication, user profile management, and email notifications.
```python
class UserManager:
    def authenticate_user(self, username, password):
        #Auth logic
    
    def update_user_profile(self, user_id, new_profile_data):
        #Update logic
    
    def send_email_notification(self, user_email, message):
        #notification logic
```
The above class violates the SRP because it has multiple responsibilities: **authentication**, **profile management**, and **email notifications**.

If you need to change the way user authentication is handled, you might inadvertently affect the email notification logic, or vice versa.

To adhere to the SRP, we can split this class into three separate classes, each with a single responsibility:

```python
class UserAuthenticator:
    def authenticate_user(self, username, password):
        #Auth logic
    
class UserProfileManager:
    def update_user_profile(self, user_id, new_profile_data):
        #Update logic
    
class EmailNotifier:
    def send_email_notification(self, user_email, message):
        # notification logic

```

Now, each class has a single, well-defined responsibility. Changes to user authentication won't affect the email notification logic, and vice versa, improving maintainability and reducing the risk of unintended side effects.

### O: Open closed principle (OCP)
> Software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification.

This means the design of a software entity should be such that you can introduce new functionality or behavior without modifying the existing code since changing the existing code might introduce bugs.

**Code Example:**
Let's say you have a ShapeCalculator class that calculates the area and perimeter of different shapes like rectangles and circles.

```python
class ShapeCalculator:
    def calculate_area(self, shape):
        if shape.type == "rectangle":
            return shape.width * shape.height
        elif shape.type == "circle":
            return 3.14 * (shape.radius**2)
    
    def calculate_perimeter(self, shape):
        if shape.type == "rectangle":
            return 2 * (shape.width + shape.height)
        elif shape.type == "circle":
            return 2 * 3.14 * shape.radius
```

If we want to add support for a new shape, like a triangle, we would have to **modify** the `calculate_area` and `calculate_perimeter` methods, violating the Open/Closed Principle.

To adhere to the OCP, we can create an abstract base class for shapes and separate concrete classes for each shape type:

```python
from abc import  ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def calculate_area(self):
        pass
    
    @abstractmethod
    def calculate_perimeter(self):
        pass
    
class Rectangle(Shape):
    def __init__(self, height, width):
        self.height = height
        self.width = width
        
    def calculate_area(self):
        return self.height * self.width
    
    def calculate_perimeter(self):
        return 2 * (self.width + self.height)
    
    
class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius

    def calculate_area(self):
        return 3.14 * (self.radius**2)

    def calculate_perimeter(self):
        return 2 * 3.14 * self.radius
    
## We can now add new shapes like Triangle without modifying existing code.
class Triangle(Shape):
    #Implementation of Triangle Class
    
```

By introducing an abstraction (`Shape` class) and separating the concrete implementations (`Rectangle` and `Circle` classes), we can add new shapes without modifying the existing code.

The `ShapeCalculator` class can now work with any shape that implements the `Shape` interface, allowing for easy extensibility.
