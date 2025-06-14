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


### L: Liskov's Substitution Principle (LSP)
> Objects of a superclass should be replaceable with objects of its subclasses without affecting the correctness of the program.

This means if you have a base class and a derived class, you should be able to use the instances of the derived class wherever instances if the base class are expected, without breaking the application.

**Code Example:**

Consider  we have a base class Vehicle and two derived classes Car and Bicycle.

Without following the LSP, the code might look like this:

```python
    class Vehicle:
        def start_engine(self):
            pass
    
    class Car:
        def start_engine(self):
            print("starting car engine")
    
    class Bicycle:
        def start_engine(self):
            # doesn't make sense for bicylce
            pass
            
```

In this example, the `Bicycle` class violates the LSP because it provides an implementation for the `start_engine` method, which doesn't make sense for a bicycle.

If we try to substitute a `Bicycle` instance where a Vehicle instance is expected, it might lead to unexpected behavior or errors.

To adhere to the LSP, we can restructure the code as follows:

```python
    class Vehicle:
        def start(self):
            raise notImplementedError
    
    class Car:
        def start(self):
            print("starting car engine")
    
    class Bicycle:
        def start(self):
            print("pedalling bicycle..")
            
            
```
Here, we've replaced the `start_engine` method with a more general `start` method in the base class Vehicle. The `Car` class implements the start method to start the engine, while the `Bicycle` class implements the start method to indicate that the rider is pedaling.

Now, instances of `Car` and `Bicycle` can be safely substituted for instances of Vehicle without any unexpected behavior or errors.

Summary
- The Liskov Substitution Principle states that objects in a program should be
  replaceable with instances of their subtypes without altering the correctness of that
  program.
- To identify violations, we can check if we can replace a class with its subclasses
  having to handle special cases and expect the same behaviour.

- Do not tie behaviour to the class hierarchy. Instead, create new base classes so that
we can add new types of behaviour without having to add new intermediate levels in
the class hierarchy.

### I : Interface segregation principle (ISP)

> No client should be forced to depend on interfaces they don't use.

The main idea behind ISP is to prevent the creation of "fat" or "bloated" interfaces that include methods that are not required by all clients.

By segregating interfaces into smaller, more specific ones, clients only depend on the methods they actually need, promoting loose coupling and better code organization.

**Code Example:**

Let's consider a scenario where we have a media player application that supports different types of media files, such as audio files (MP3, WAV) and video files (MP4, AVI).

Without applying the ISP, we might have a single interface like this:

```python
    class MediaPlayer:
        def play_audio(self, audio_file):
            raise NotImplementedError
        
        def play_video(self, video_file):
            raise NotImplementedError
        
        def stop_audio(self):
            raise NotImplementedError
        
        def stop_video(self):
            raise NotImplementedError

        def adjust_audio_volume(self, volume):
            raise NotImplementedError 
        
        def adjust_video_brightness(self, brightness):
            raise NotImplementedError   

```

In this case, any class that implements the `MediaPlayer` interface would be forced to implement all the methods, even if it doesn't need them.

For example, an audio player would have to implement the `play_video`, `stop_video`, and `adjust_video_brightness` methods, even though they are not relevant for audio playback.

To adhere to the ISP, we can segregate the interface into smaller, more focused interfaces:

```python
    class AudioPlayer:
        def play_audio(self, audio_file):
            raise NotImplementedError 
            
    def stop_audio(self):
            raise NotImplementedError      
    
    def adjust_audio_volume(self, volume):
            raise NotImplementedError 
        
 
    class VideoPlayer:
        def play_video(self, video_file):
            raise NotImplementedError
        
        def stop_video(self):
            raise NotImplementedError
        
        def adjust_video_brightness(self, brightness):
            raise NotImplementedError
               
```


Now, we can have separate implementations for audio and video players:

```python
    
     class MP3Player(AudioPlayer):
        def play_audio(self, audio_file):
            # play mp3 file
            
        def stop_audio(self):
                # stop audio      
        
        def adjust_audio_volume(self, volume):
                # adjust volume 
    
    
     class AviVideoPlayer(VideoPlayer):
        def play_video(self, video_file):
            # play video
        
        def stop_video(self):
            # stop video
        
        def adjust_video_brightness(self, brightness):
            # adjust brightness

```

By segregating the interfaces, each class only needs to implement the methods it actually requires. This not only makes the code more maintainable but also prevents clients from being forced to depend on methods they don't use.

If we need a class that supports both audio and video playback, we can create a new class that implements both interfaces:
```python
    class MultiMediaPlayer(AudioPlayer, VideoPlayer):
        # Implements methods from both audio and video player
```
