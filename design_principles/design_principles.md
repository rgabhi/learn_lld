# Design Principles

## SOLID Principles

The SOLID principles provide a blueprint for writing code that’s easy to adjust, extend, and maintain over time.

It was introduced by Robert C. Martin (Uncle Bob) in the early 2000s.


### S: Single responsibility principle (SRP)
> A class should have one, and only one, reason to change.

This means that a class must have only one responsibility.

When a class performs just one task, it contains a small number of methods and member variables making them more usable and easier to maintain.

If a class has multiple responsibilities, it becomes harder to understand, maintain, and modify and increases the potential for bugs because changes to one responsibility could affect the others.

**Code Example**:
\
Imagine you have a class called UserManager that handles user authentication, user profile management, and email notifications.
```python
class UserManager:
    def authenticate_user(self, username, password):
        #Auth logic
    
    def update_user_profile(self, user_id, new_profile_data):
        #Update logic
    
    def send_email_notification(self, user_email, message):
        # notification logic
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

