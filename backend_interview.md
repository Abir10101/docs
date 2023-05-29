# Backend Developer Interview Questions

### In MVC framework, what is the purpose of migrations?
- Migrations are used to define and modify the structure of database tables.
- Migrations enable version control for the database schema by storing the migration files in the project and version control using git.
- Migrations work hand-in-hand with Laravel's Object-Relational Mapping (ORM) system, such as Eloquent. When you define a migration for a table, it automatically generate the corresponding model.
<br>
<br>

## What is the purpose of Object-Relational Mapping (ORM)?
- The ORM allows you to interact with the database using object-oriented principles instead of writing raw SQL queries.
- It enables abstraction between different database engines, such as MySQL, PostgreSQL, SQLite, or Oracle. This abstraction makes it easier to switch between different databases without needing to change the application code.
- The ORM maps database tables to classes and database records to objects of those classes. This allows to work with database data as if they were regular objects, using attributes and methods to access and manipulate the data.
- It provides query optimization techniques to improve the performance of database queries.
<br>
<br>

## What is CSRF protection?
CSRF (Cross-Site Request Forgery) is a type of attack where an attacker tricks a user's authentication into making an unintended request to server on behalf of the user.
<br>
<br>

## How CSRF attack executed?
- ***Crafting the Malicious Request:*** The attacker crafts a malicious request that performs an action on a target website. This could be a request to change the victim's password, make a purchase, post a message, or perform any other action that the target website allows.
- ***Generating the Attack Payload:*** The attacker generates a webpage or an HTML email containing the malicious request. This payload is designed to be executed by the victim's browser.
- ***Delivery of the Attack Payload:*** The attacker delivers the payload to the victim. This can be done through various means, such as sending a deceptive email or enticing the victim to visit a specially crafted webpage.
- ***Victim's Interaction:*** The victim, unaware of the malicious nature of the request, interacts with the payload by clicking on the deceptive link.
- ***Execution of the Malicious Request:*** The victim's browser sends the malicious request to the target website. Since the victim is already authenticated, the target website treats the request as legitimate and performs the requested action.
<br>
<br>

## How frameworks (Laravel/Django) implements csrf protection?
- ***CSRF Token Generation:*** When a user visits a app, a unique non-persistent (session deleted when application closed) CSRF token is generated for their session. This token is typically stored in a cookie and as a separate header.
- ***Token Inclusion:*** Developer should include the CSRF token in all HTML forms.
- ***Token Verification:*** When a request is received, the CSRF protection middleware automatically verifies the token. If the token in the request does not match the one associated with the user's session, the framework rejects the request.
<br>
<br>

## What is a Decorator in Python?
Decorator is a special construct that allows you to modify the behavior of a function or a class without directly changing its source code. It provides a way extend the functionality of the original object with additional code.  
Example:
```
def log_function_call(func):
    def wrapper(*args, **kwargs):
        # Perform logging before the function call
        logging.info(f"Calling function: {func.__name__}")
        
        # Call the original function
        result = func(*args, **kwargs)
        
        # Perform logging after the function call
        logging.info(f"Function {func.__name__} completed")
        
        # Return the result of the original function
        return result
    
    return wrapper

@log_function_call
def multiply(a, b):
    return a * b
```
Whenever the multiply function is called, the logging decorator will be invoked. It will log a message before the function call and another message after the function is executed.
<br>
<br>

## What is Middleware in a frameworks (Django/Laravel)?
- Middleware functions are used for tasks such as authentication, authorization, request/response manipulation, logging, error handling, etc. They allow to perform certain checks or modifications on the request before they reach the controller function.
- For example, in case of user authentication, a middleware function can be used to check if a user is logged in or has the necessary permissions to access a particular route. If the conditions are not met, the middleware can redirect the user to a login page or return an error response without reaching the controller function.





