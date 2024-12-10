## Introduction 
...

## 1. Naming 
- Good naming improves readability and maintainability by clearly conveying intent, purpose, and context.
- Regularly refactor names to enhance clarity and eliminate ambiguity.

### 1.1.  Use Intention-Revealing Names
- Names should describe the purpose, usage, and meaning without needing additional comments.
```python
# Good
elapsed_time_in_days = 7
days_since_creation = 10

# Bad
d = 7  # elapsed time
```

### 1.2. Make Meaningful Distinctions
- Names should highlight the purpose and differences between entities.
- Avoid generic or arbitrary changes, like adding numbers or noise words.
```python
# Good  
def copy_chars(source, destination):
    for i in range(len(source)):
        destination[i] = source[i]

# Bad
def copy_chars(a1, a2):
    for i in range(len(a1)):
        a2[i] = a1[i]
```

### 1.3. Avoid Misleading or Ambiguous Names
- Avoid names that mislead or suggest a meaning different from their intent.
```python
# Good
account_group = [account1, account2]

# Bad
account_list = {key: value}  # Misleading, not a list
```

### 1.4. Use Pronounceable and Searchable Names
- Choose names that are easy to pronounce and search for in code.
- Avoid single-letter variables or constants that lack clarity.
```python
# Good
pswdExpDt = "2024-12-31"

# Bad
password_expiration_date = "2024-12-31"

# Good
WORK_DAYS_PER_WEEK = 5
HOURS_PER_DAY = 4
TASK_COUNT = 7

for task_index in range(TASK_COUNT):
    total += values[task_index] * HOURS_PER_DAY / WORK_DAYS_PER_WEEK

# Bad
for i in range(7):
    total += values[i] * 4 / 5
```

### 1.5. Avoid Encodings and Mental Mapping
- Do not add unnecessary type or scope encodings (e.g., m_ or Hungarian Notation).
- Use descriptive names instead of abstract placeholders.
```python
# Good
class Part:
    def __init__(self):
        self.description = ""

# Bad
class Part:
    def __init__(self):
        self.m_description = ""

# Good
url_without_host = "example.com/path"

# Bad
r = "example.com/path"  # What does r mean?
```

### 1.6. Add Context Through Names
- Provide context through clear class, function, or variable names.
- Avoid unnecessary prefixes or redundant context.
```python
# Good
class Address:
    def __init__(self, street, city, state, zip_code):
        pass

# Bad
class GSDAddress:
    def __init__(self, gsd_street, gsd_city, gsd_state, gsd_zip_code):
        pass
```

### 1.7. Avoid Cute or Clever Names
- Avoid humor or puns in names that may confuse others or lack clarity.
```python
# Good
def delete_items():
    pass

# Bad
def hand_grenade():
    pass
```

## 2. Functions
- Functions are the building blocks of clean code.
- They should be small, focused, and descriptive, resembling well-structured narrative verbs in your domain language.
- Strive for simplicity, avoid duplication, and write functions that clearly express intent.

### 2.1 Keep Functions Small and Focused
- Functions should be short and perform only one task.
- Each function should focus on clarity and brevity.
```python
# Good
def calculate_total_price(cart):
    total_price = sum(item.price for item in cart)
    return apply_discount(total_price)

def apply_discount(price):
    discount_rate = 0.1  # 10% discount
    return price * (1 - discount_rate)

# Bad
def calculate_total_price(cart):
    total_price = 0
    for item in cart:
        total_price += item.price
    discount_rate = 0.1  # 10% discount
    total_price = total_price * (1 - discount_rate)
    return total_price
```

### 2.2. Do One Thing
- Each function should do one thing and do it well.
- Avoid combining unrelated operations.
```python
# Good
def process_order(order):
    validate_order(order)
    calculate_shipping(order)
    finalize_order(order)

def validate_order(order):
    if not order.is_valid():
        raise ValueError("Order is invalid")

def calculate_shipping(order):
    order.shipping_cost = order.weight * 0.5

def finalize_order(order):
    order.status = "completed"
    send_order_confirmation(order)

# Bad
def process_order(order):
    if not order.is_valid():
        raise ValueError("Order is invalid")
    order.shipping_cost = order.weight * 0.5
    order.status = "completed"
    send_order_confirmation(order)
```

### 2.3. Use a Single Level of Abstraction
- Functions should operate at a single level of abstraction.
- Avoid mixing high-level logic with low-level details.
```python
# Good
def process_user_profile(user_data):
    user_profile = fetch_user_profile(user_data)
    if user_profile:
        update_profile_details(user_profile, user_data)

def fetch_user_profile(user_data):
    return database.get("user_profiles", user_data["id"])

def update_profile_details(profile, data):
    profile["name"] = data.get("name")
    profile["email"] = data.get("email")
    database.save("user_profiles", profile)

# Bad
def process_user_profile(user_data):
    user_profile = database.get("user_profiles", user_data["id"])
    if user_profile:
        user_profile["name"] = user_data.get("name")
        user_profile["email"] = user_data.get("email")
        database.save("user_profiles", user_profile)
```

### 2.4. Use Descriptive Names
- Function names should clearly describe their purpose.
- Long, descriptive names are better than short, cryptic ones.
```python
# Good
def calculate_monthly_expenses(expenses):
    total = sum(expenses)
    return total

# Bad
def process_data(data):
    total = sum(data)
    return total
```

### 2.5. Minimize Function Arguments
- Keep function arguments minimal. Prefer niladic (0), monadic (1), or dyadic (2) arguments.
- Use argument objects for more than two arguments.
```python
# Good
def create_circle(center, radius):
    pass

# Bad
def create_circle(x, y, radius):
    pass
```

### 2.6. Avoid Flag Arguments
- Flag arguments (e.g., `is_suite`) indicate a function is doing more than one thing. Split such functions into separate ones.
```python
# Good
def process_admin_request(request_data):
    log_admin_access(request_data)
    return handle_admin_logic(request_data)

def process_user_request(request_data):
    return handle_user_logic(request_data)

# Bad
def process_request(request_data, is_admin):
    if is_admin:
        log_admin_access(request_data)
        return handle_admin_logic(request_data)
    else:
        return handle_user_logic(request_data)
```

### 2.7. Separate Command and Query Functions
- Functions should either perform an action (command) or return information (query), but not both.
```python
# Good
def is_username_valid(username):
    return username in valid_usernames

def add_username(username):
    valid_usernames.append(username)

if is_username_valid("john_doe"):
    add_username("john_doe")

# Bad
def add_username_if_valid(username):
    if username in valid_usernames:
        valid_usernames.append(username)
        return True
    return False

if add_username_if_valid("john_doe"):
    pass
```

### 2.8. Use Exceptions Instead of Error Codes
- Avoid returning error codes.
- Use exceptions to separate normal flow from error handling.
```python
# Good
def fetch_data(url):
    if not url:
        raise ValueError("URL cannot be empty")
    # Logic to fetch data from the URL
    return "data"

try:
    data = fetch_data("")
except ValueError as e:
    print(f"Error: {e}")

# Bad
def fetch_data(url):
    if not url:
        return "ERROR: URL cannot be empty"
    # Logic to fetch data from the URL
    return "data"

result = fetch_data("")
if "ERROR" in result:
    print(result)
```

### 2.9. Eliminate Duplication
- Abstract common logic into reusable functions to avoid repeated code.
```python
# Good
def handle_user_action(action, user_id):
    user = fetch_user(user_id)
    if action == "activate":
        update_status(user, "active")
    elif action == "deactivate":
        update_status(user, "inactive")

# Bad
def activate_user(user_id):
    user = fetch_user(user_id)
    update_status(user, "active")

def deactivate_user(user_id):
    user = fetch_user(user_id)
    update_status(user, "inactive")
```

### 2.10. Error Handling is One Thing
- Functions that handle errors should focus solely on error handling.
- Extract error-handling logic into separate functions.
```python
# Good
def process_order_with_error_handling(order):
    try:
        process_order(order)
    except ValueError as e:
        handle_validation_error(e)
    except ConnectionError as e:
        handle_connection_error(e)

# Bad
def process_order(order):
    try:
        # Core processing logic
        if not validate_order(order):
            raise ValueError("Invalid order")
        connect_to_service()
        send_order(order)
    except ValueError as e:
        log_error(f"Validation error: {e}")
    except ConnectionError as e:
        log_error(f"Connection error: {e}")
```

## 3. Comments
- Comments are a necessary evil, not a hallmark of good code. They often:
  - Become outdated and misleading.
  - Signal failures to express intent through code.
- Always strive to rewrite code for clarity instead of relying on comments.
### 3.1. When Comments are Necessary
#### 3.1.1. Informative Comments and Explanation of Intent
- Provide essential information that cannot be expressed directly in code.
```python
# Matches "Tue, 02 Apr 2003 22:18:49 GMT"
http_date_pattern = re.compile(
    r"[SMTWF][a-z]{2},\s[0-9]{2}\s[JFMASOND][a-z]{2}\s[0-9]{4}\s[0-9]{2}:[0-9]{2}:[0-9]{2}\sGMT"
)
```

#### 3.1.2. Explanation of Intent and Amplification
- Clarify the reasoning behind specific decisions.
- Emphasize critical details that might not be immediately obvious.
```python
# Use a thread-local date formatter to avoid concurrency issues
from threading import local

thread_local_date_format = local()
def get_date_format():
    if not hasattr(thread_local_date_format, "formatter"):
        thread_local_date_format.formatter = "%Y-%m-%d"
    return thread_local_date_format.formatter

# The trim is crucial to avoid issues with leading/trailing spaces
list_item = text.strip()
```

#### 3.1.3. Warnings About Consequences
- Warn developers about potential pitfalls or important considerations.
```python
# SimpleDateFormat is not thread-safe; create a new instance for each thread
date_format = "%Y-%m-%d"
```

#### 3.1.4. TODO Comments
- Mark areas that require further work or optimization.
```python
# TODO: Optimize this function for large datasets
def calculate_primes(limit):
    pass
```

#### 3.1.5. Public API Documentation
- Use docstrings for public-facing APIs to improve usability.
```python
def get_user_by_id(user_id: str):
    """
    Retrieves a user by their ID.

    Args:
        user_id (str): The unique identifier for the user.

    Returns:
        dict: A dictionary containing user information, or None if not found.
    """
    pass
```

### 3.2. What to Avoid
#### 3.2.1. Redundant and Noisy Comments
- Avoid comments that restate what the code already expresses.
- Avoid comments that add no value to the code.
   Avoid comments added solely for compliance or formalities.
```python
# Adds two numbers
result = a + b  # Redundant and unnecessary

# Returns the day of the month
def get_day_of_month():
    return day_of_month

# The name of the user
user_name = "John"  # Pointless redundancy
```

#### 3.2.2. Misleading Comments
- Ensure comments accurately describe the code they accompany.
```python
# Returns True if the list is empty
return len(my_list) > 0  # Misleading; this checks if the list is non-empty
```

#### 3.2.3. Journal Comments and Commented-Out Code
- Use version control systems instead of maintaining manual comment logs.
- Remove commented-out code entirely; version control retains the history.
```python
# Added by Eric on 2024-01-15

# Old implementation
# result = calculate_old_way(x, y)
# return result
```

### 3.3. Refactor Instead of Commenting
- Use **descriptive function names** and **variables** instead of relying on comments.
```python
# Good
if user.is_eligible_for_benefits():
    grant_benefits(user)

# Bad
# Check if the user is eligible for benefits
if user.age > 65 and user.is_retired:
    grant_benefits(user)
```

> ### Principles for Clean Comments
> 1. **Keep Comments Local**: A comment should describe only the code near it.
> 3. **Write Meaningful Comments**: Ensure comments are informative, accurate, and add value.
> 4. **Strive for Self-Documenting Code**: Refactor code to minimize the need for comments.
> 5. **Avoid Using Comments as a Crutch**: Poorly written code with lots of comments is not better than clean, comment-free code.


## 4. Formatting
### 4.1. Vertical Openness Between Concepts
- Use **blank lines** to separate distinct concepts and make the code more readable.
```python
# Good
def add(a, b):
    return a + b

def subtract(a, b):
    return a - b

# Bad
def add(a,b):
    return a+b
def subtract(a,b):
    return a-b
```

### 4.2. Vertical Density
- Keep **related lines of code tightly grouped** without unnecessary blank lines.
```python
# Good
class ReporterConfig:
    def __init__(self):
        self.reporter_name = ""
        self.properties = []

    def add_property(self, property):
        self.properties.append(property)

# Bad
class ReporterConfig:

    def __init__(self):

        self.reporter_name = ""


        self.properties = []


    def add_property(self, property):

        self.properties.append(property)
```

### 4.3. Newspaper Metaphor
- Keep **high-level functions and concepts at the top**, and **low-level details at the bottom**.
```python
# Good
def main():
    result = calculate_total(10, 20)
    print(f"Result: {result}")

def calculate_total(a, b):
    return a + b

# Bad
def calculate_total(a, b):
    return a + b

def main():
    result = calculate_total(10, 20)
    print(f"Result: {result}")
```

### 4.4. Vertical Ordering
- **Caller functions should appear above callee functions**, allowing a natural top-down flow.
```python
# Good
def main():
    result = multiply(2, 3)
    print(f"Result: {result}")

def multiply(a, b):
    return a * b

# Bad
def multiply(a, b):
    return a * b

def main():
    result = multiply(2, 3)
    print(f"Result: {result}")
```

### 4.5. Limit Line Length
- Avoid **long lines** of code that are hard to read.
```python
# Good
result = (-b + math.sqrt((b ** 2) - (4 * a * c))) / (2 * a)

# Bad
result = (-b + math.sqrt((b ** 2) - (4 * a * c))) / (2 * a) + (-b - math.sqrt((b ** 2) - (4 * a * c))) / (2 * a)
```

### 4.6. Horizontal Openness
- Use **spaces around operators** for clarity.
```python
# Good
result = (a + b) * (c - d)

# Bad
result=(a+b)*(c-d)
```

### 4.7. Avoid Horizontal Alignment
- Avoid **manually aligning variables or assignments**, as it adds clutter and isn't flexible.
```python
# Good
x = 10
y = 20
zz = 30

# Bad
x   = 10
y   = 20
zz  = 30
```

### 4.8. Indentation
- Maintain **consistent indentation** of 4 spaces, and don't collapse scopes unnecessarily.
```python
# Good
if condition:
    do_something()

# Bad
if condition: do_something()
```

### 4.9. Dummy Scopes
- Add **clear intent** for empty loops or dummy scopes.
```python
# Good
while some_condition:
    pass  # Waiting for the condition to change

# Bad
while some_condition:
    ;
```

> ### Principles for Readable and Consistent Formatting
>  1. Code should be easy to read, consistent, and orderly.
>  2. The team must agree on a uniform style to ensure consistency across the project.
>  3. Tools like Black, isort, and flake8 can help automate formatting and enforce rules.


## 5. Objects and Data Structures in Python
- This section highlights the balance between objects and data structures, focusing on their proper use in a Python codebase.
- Objects encapsulate data and expose behavior, while data structures expose data without embedding business logic.
### 5.1 Data Abstraction
- Objects should expose behavior while hiding implementation details.
- Avoid exposing raw variables; instead, use abstract interfaces.
```python
# Good
class Point:
    def set_cartesian(self, x, y):
        self._x = x
        self._y = y

    def get_coordinates(self):
        return self._x, self._y

# Bad
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y
```

### 5.2. Abstract vs. Concrete Interfaces
- Abstract interfaces describe "what" the data represents, not "how" it is stored.
- Avoid forcing consumers to understand implementation details.
```python
# Good
class Vehicle:
    def get_fuel_percentage(self):
        return (self._fuel_remaining / self._fuel_capacity) * 100

# Bad
class Vehicle:
    def __init__(self, fuel_capacity, fuel_remaining):
        self.fuel_capacity = fuel_capacity
        self.fuel_remaining = fuel_remaining
```

### 5.3. Data/Object Anti-Symmetry
- Procedural code simplifies adding new functions; OO simplifies adding new types.
- Use the approach that fits your system's needs.
```python
# Good
class Shape:
    def area(self):
        raise NotImplementedError

class Circle(Shape):
    def area(self):
        return math.pi * self.radius ** 2

# Bad
class Circle:
    def __init__(self, radius):
        self.radius = radius

def area(shape):
    if isinstance(shape, Circle):
        return math.pi * shape.radius ** 2
```

### 5.4. The Law of Demeter
- Minimize dependencies by interacting only with immediate collaborators.
- Avoid "train wrecks" (long chains of method calls).
```python
# Good
output_dir = ctxt.get_absolute_scratch_path()

# Bad
output_dir = ctxt.get_options().get_scratch_dir().get_absolute_path()
```

### 5.5. Hybrids
- Avoid hybrids that mix object behaviors and data structure accessors.
- Separate data structures from business logic.
```python
# Good
class UserDTO:
    def __init__(self, username, email):
        self.username = username
        self.email = email

# Bad
class User:
    def __init__(self, username, email):
        self.username = username
        self.email = email

    def send_welcome_email(self):
        print(f"Sending welcome email to {self.email}")
```

> ### Principles of Objects and Data Structures
> - **Objects**:
>   - Encapsulate behavior and hide data.
>   - Easy to add new types without changing existing behaviors.
>   - Difficult to add new behaviors to existing types.
> - **Data Structures**:
>   - Expose data and have minimal behavior.
>   - Easy to add new behaviors without altering existing data structures.
>   - Difficult to add new types without modifying existing functions.
> - **Choosing the Right Approach**:
>   - Prefer objects when you need flexibility in adding new types.
>   - Prefer data structures when you need flexibility in adding new behaviors.

## 6. Error Handling
- Separate error handling from the main logic for clarity and maintainability.
- Treat error handling as an independent concern to ensure clean, robust, and testable code.
### 6.1. Use Exceptions Rather Than Return Codes
- Exceptions make code cleaner by separating logic from error handling.
- Avoid cluttering the caller with checks for return values.
```python
# Good
try:
    device.shutdown()
except DeviceShutdownError as e:
    logger.log(e)

# Bad
if device.handle != INVALID_HANDLE:
    if device.status != DEVICE_SUSPENDED:
        device.pause()
        device.clear_queue()
        device.close()
    else:
        logger.log("Device is suspended")
else:
    logger.log("Invalid handle")
```

### 6.2. Write Your Try-Catch-Finally Statement First
- Define the scope of error handling upfront.
- Helps maintain consistent program state even when errors occur.
```python
# Good
try:
    with open("data.txt", "r") as file:
        process(file.read())
except FileNotFoundError as e:
    logger.log(f"File not found: {e}")

# Bad
with open("data.txt", "r") as file:
    try:
        process(file.read())
    except:
        pass
```

### 6.3. Use Unchecked Exceptions
- Reduce dependencies and method signature changes by using unchecked exceptions.
- Avoid forcing upstream changes for low-level exceptions.
```python
# Good
def process_data(data):
    if not validate(data):
        raise DataValidationError("Invalid data")
    # Process data...

# Bad
def process_data(data):
    if not validate(data):
        return False  # Caller must check return value
    # Process data...
```

### 6.4. Provide Context with Exceptions
- Include meaningful error messages to help with debugging.
- Add relevant context like operation type or failing input.
```python
# Good
raise FileProcessingError(f"Failed to process file {filename}")

# Bad
raise Exception("Error occurred")
```

### 6.5. Define Exception Classes in Terms of Caller’s Needs
- Use a single custom exception class for similar errors.
- Simplify handling by grouping exceptions into logical categories.
```python
# Good
class PortDeviceFailure(Exception):
    pass

try:
    port.open()
except PortDeviceFailure as e:
    logger.log(e)

# Bad
try:
    port.open()
except DeviceResponseException:
    logger.log("Device response error")
except ATM1212UnlockedException:
    logger.log("ATM unlock error")
```

### 6.6. Define the Normal Flow
- Use the **Special Case Pattern** to encapsulate special cases.
- Avoid exceptions in regular logic flows.
```python
# Good
class MealExpenses:
    def get_total(self):
        return 0

# Always return an object, even for missing data
expenses = expense_report.get_meals(employee.id) or MealExpenses()
total += expenses.get_total()

# Bad
try:
    expenses = expense_report.get_meals(employee.id)
    total += expenses.get_total()
except MealExpensesNotFound:
    total += per_diem
```

### 6.7. Don’t Return Null
- Return special case objects or throw exceptions instead of returning None.
- Eliminate the need for repetitive `None` checks.
```python
# Good
def get_employees():
    return []

employees = get_employees()
for e in employees:
    total_pay += e.get_pay()

# Bad
employees = get_employees()
if employees is not None:
    for e in employees:
        total_pay += e.get_pay()
```

### 6.8. Don’t Pass Null
- Avoid accepting `None` as an argument unless explicitly required.
- Validate inputs to catch issues early.
```python
# Good
def calculate_metric(p1, p2):
    if p1 is None or p2 is None:
        raise ValueError("Points must not be null")
    return (p2.x - p1.x) * 1.5

# Bad
def calculate_metric(p1, p2):
    return (p2.x - p1.x) * 1.5  # Will throw runtime error if p1 or p2 is None
```

## 7. Boundaries
- Encapsulating third-party code minimizes the impact of future changes.
- Learning tests ensure reliable usage and maintain compatibility with updates.
- Clean boundaries reduce dependencies and promote maintainable, robust code.
### 7.1. Encapsulate Third-Party Code
- Avoid passing third-party interfaces, like Map, around your system directly.
- Encapsulate third-party code in classes or adapters to enforce constraints and control changes.
```python
# Good
class Sensors:
    def __init__(self):
        self._sensors = {}

    def add_sensor(self, id, sensor):
        self._sensors[id] = sensor

    def get_sensor(self, id):
        return self._sensors.get(id)

# Bad
sensors = {}
sensors["sensor1"] = Sensor()
sensor = sensors["sensor1"]
```

### 7.2. Use Learning Tests
- Write tests to understand and validate the behavior of third-party APIs before using them.
- Learning tests help verify compatibility with future updates of the third-party library.
```python
# Example: Learning test for a logging library
def test_logger():
    logger = logging.getLogger("test")
    logger.setLevel(logging.INFO)
    handler = logging.StreamHandler()
    handler.setFormatter(logging.Formatter("%(levelname)s: %(message)s"))
    logger.addHandler(handler)
    logger.info("This is a test log")
```

### 7.3. Define Interfaces for Unknown Boundaries
- When dependent code or APIs are unavailable, define your own interfaces to decouple development.
- Use adapters to bridge gaps when the actual API becomes available.
```python
# Good
class Transmitter:
    def transmit(self, frequency, data_stream):
        raise NotImplementedError("This method should be overridden")

# Adapter for future API
class TransmitterAdapter(Transmitter):
    def __init__(self, external_transmitter_api):
        self._api = external_transmitter_api

    def transmit(self, frequency, data_stream):
        self._api.send(frequency, data_stream)
```

### 7.4. Control Change at Boundaries
- Create minimal dependencies on third-party code to reduce maintenance overhead.
- Use adapters or wrappers to isolate changes when third-party libraries are updated.
```python
# Good
class MyLogger:
    def __init__(self):
        self.logger = logging.getLogger("MyApp")
        self.logger.addHandler(logging.StreamHandler())

    def log(self, message):
        self.logger.info(message)

# Bad
logger = logging.getLogger("MyApp")
logger.addHandler(logging.StreamHandler())
logger.info("This is a log message")
```

### 7.5. Keep Boundaries Clean
- Avoid spreading references to external APIs across your codebase.
- Use abstraction to limit the exposure of external interfaces.
```python
# Good
class PaymentProcessor:
    def process_payment(self, amount):
        raise NotImplementedError

class StripeAdapter(PaymentProcessor):
    def __init__(self, stripe_client):
        self.stripe = stripe_client

    def process_payment(self, amount):
        return self.stripe.charge(amount)

# Bad
stripe = StripeClient()
stripe.charge(100)
```

## 8. Unit Tests
- Clean tests are essential for maintainable, flexible, and robust production code.
- Follow principles like F.I.R.S.T. and focus on clarity and expressiveness.
- Invest in writing high-quality tests as they safeguard your code and enable long-term project health.
### 8.1. Write Tests First (TDD Principles)
- Write a failing unit test before any production code.
- Write just enough production code to pass the test, and repeat.
```python
# Good: Test-Driven Development
def test_addition():
    assert add(2, 3) == 5  # Failing test first

def add(a, b):
    return a + b  # Write production code after test
```

### 8.2. Keep Tests Clean
- Treat test code with the same care as production code.
- Clean and readable tests reduce maintenance costs and increase reliability.
```python
# Good: Clean and readable
def test_temperature_alert():
    hw.set_temp(WAY_TOO_COLD)
    controller.tic()
    assert hw.heater_on()
    assert hw.lo_temp_alarm()

# Bad: Dirty and repetitive
def test_temperature_alert():
    hw.set_temp(-50)
    if not hw.heater:
        raise AssertionError
    if not hw.lo_temp_alarm:
        raise AssertionError
```

### 8.2. Keep Tests Clean
- Focus each test on one concept or behavior to improve clarity and debugging.
```python
# Good: Separate concepts
def test_add_month_single_month():
    assert add_months(date(2023, 1, 31), 1) == date(2023, 2, 28)

def test_add_month_multiple_months():
    assert add_months(date(2023, 1, 31), 2) == date(2023, 3, 31)

# Bad: Multiple concepts in one test
def test_add_month():
    assert add_months(date(2023, 1, 31), 1) == date(2023, 2, 28)
    assert add_months(date(2023, 1, 31), 2) == date(2023, 3, 31)
```

### 8.5. Build a Domain-Specific Testing Language
- Create reusable utilities or methods that simplify and express the intent of your tests.
```python
# Good: Domain-Specific Language
def make_page(name, content=""):
    crawler.add_page(root, PathParser.parse(name), content)

def test_page_content():
    make_page("TestPage", "Hello World")
    assert response_contains("Hello World")

# Bad: Repeated details in every test
def test_page_content():
    crawler.add_page(root, PathParser.parse("TestPage"), "Hello World")
    response = responder.make_response(context, request)
    assert "Hello World" in response.get_content()
```

### 8.6. Minimize Asserts (If Practical)
- While not always mandatory, minimizing the number of assertions can improve test clarity.
```python
# Good: Focused test
def test_low_temp_alarm():
    hw.set_temp(WAY_TOO_COLD)
    controller.tic()
    assert hw.get_state() == "HBchL"  # Encoded state: Heater, Blower, Low Alarm On

# Bad: Multiple assertions, harder to interpret
def test_low_temp_alarm():
    hw.set_temp(WAY_TOO_COLD)
    controller.tic()
    assert hw.heater_on()
    assert hw.blower_on()
    assert not hw.cooler_on()
    assert hw.lo_temp_alarm()
```

### 8.7. F.I.R.S.T. Principles
- **Fast**: Tests should run quickly to encourage frequent execution.
- **Independent**: Tests should not depend on the state set by other tests.
- **Repeatable**: Tests should produce consistent results in any environment.
- **Self-Validating**: Tests should pass or fail without manual inspection.
- **Timely**: Write tests just before the production code they validate.

## 9. Classes
- Clean classes adhere to **SRP** and **OCP**, are cohesive, and minimize coupling.
- Proper abstraction and organization enhance scalability, maintainability, and testability.
### 9.1. Class Organization
- Organize constants, instance variables, and methods logically.
```python
# Good: Constants, followed by instance variables, followed by methods
class User:
    DEFAULT_ROLE = "guest"  # Constant

    def __init__(self, username):
        self.username = username  # Instance variable
        self.role = self.DEFAULT_ROLE

    def promote(self):
        self.role = "admin"

    def demote(self):
        self.role = self.DEFAULT_ROLE
```

### 9.2. Keep Classes Small
- **Single Responsibility Principle (SRP)**: A class should do one thing well.
```python
# Good: Single Responsibility
class Version:
    def __init__(self, major, minor):
        self.major = major
        self.minor = minor

    def get_version(self):
        return f"{self.major}.{self.minor}"


# Bad: Multiple Responsibilities
class SuperDashboard:
    def __init__(self):
        self.major_version = 1
        self.minor_version = 0
        self.last_focused_component = None

    def get_version(self):
        return f"{self.major_version}.{self.minor_version}"

    def set_last_focused(self, component):
        self.last_focused_component = component
```

### 9.3. High Cohesion
- A class should tightly relate its methods to its variables.
```python
# Good: Cohesive Class
class Stack:
    def __init__(self):
        self.elements = []

    def push(self, element):
        self.elements.append(element)

    def pop(self):
        if not self.elements:
            raise ValueError("Stack is empty")
        return self.elements.pop()

    def size(self):
        return len(self.elements)


# Bad: Low Cohesion
class MixedBag:
    def __init__(self):
        self.numbers = []
        self.text = ""

    def add_number(self, number):
        self.numbers.append(number)

    def print_text(self):
        print(self.text)
```

### 9.4. Organize for Change
- Design for minimal changes when adding new functionality.
- **Open-Closed Principle (OCP)**: Extend functionality without modifying existing code.
```python
# Good: Open-Closed with Subclasses
class Sql:
    def generate(self):
        raise NotImplementedError()


class SelectSql(Sql):
    def __init__(self, table, columns):
        self.table = table
        self.columns = columns

    def generate(self):
        return f"SELECT {', '.join(self.columns)} FROM {self.table}"


class InsertSql(Sql):
    def __init__(self, table, columns, values):
        self.table = table
        self.columns = columns
        self.values = values

    def generate(self):
        return f"INSERT INTO {self.table} ({', '.join(self.columns)}) VALUES ({', '.join(self.values)})"
```

### 9.5. Minimize Coupling
- Depend on **abstractions** rather than **concrete implementations**.
- Use dependency injection to decouple classes.
```python
# Good: Interface Dependency
class StockExchange:
    def get_current_price(self, symbol):
        raise NotImplementedError()


class FixedStockExchange(StockExchange):
    def __init__(self):
        self.prices = {}

    def set_price(self, symbol, price):
        self.prices[symbol] = price

    def get_current_price(self, symbol):
        return self.prices.get(symbol, 0)


class Portfolio:
    def __init__(self, stock_exchange):
        self.stock_exchange = stock_exchange
        self.holdings = {}

    def add_stock(self, symbol, quantity):
        self.holdings[symbol] = self.holdings.get(symbol, 0) + quantity

    def value(self):
        return sum(
            self.stock_exchange.get_current_price(symbol) * quantity
            for symbol, quantity in self.holdings.items()
        )


# Bad: Concrete Dependency
class Portfolio:
    def __init__(self):
        self.stock_exchange = FixedStockExchange()
        self.holdings = {}

    # Other methods tightly coupled to FixedStockExchange
```


### 9.6. Testability and Flexibility
- Use mock objects or stub implementations to isolate functionality.
```python
# Test with Stub
class FixedStockExchangeStub(StockExchange):
    def __init__(self):
        self.prices = {}

    def set_price(self, symbol, price):
        self.prices[symbol] = price

    def get_current_price(self, symbol):
        return self.prices[symbol]


def test_portfolio():
    exchange_stub = FixedStockExchangeStub()
    exchange_stub.set_price("AAPL", 150)

    portfolio = Portfolio(exchange_stub)
    portfolio.add_stock("AAPL", 10)

    assert portfolio.value() == 1500
```

## 10. Systems
- This section will cover:
  - **Separation of Concerns** using dependency injection and factories.
  - **Cross-Cutting Concerns** handled with decorators.
  - **Scalable and Testable Architectures** using plain objects and modular design.
  - **DSLs** for high-level configuration.
### 10.1. Separate Constructing a System from Using It
- **Example: Dependency Injection** separates object construction and usage.
```python
# Dependency Injection example in Python
class Service:
    def operation(self):
        return "Service operation result"

class Consumer:
    def __init__(self, service: Service):
        self.service = service

    def perform_task(self):
        return self.service.operation()

# Main function handles construction
if __name__ == "__main__":
    service = Service()
    consumer = Consumer(service)
    print(consumer.perform_task())  # Outputs: "Service operation result"
```

### 10.2. Factories
- Abstract Factory pattern allows decoupling object creation from usage.
```python
from abc import ABC, abstractmethod

# Abstract Factory
class LineItemFactory(ABC):
    @abstractmethod
    def create_line_item(self, name, price):
        pass

# Concrete Factory
class ConcreteLineItemFactory(LineItemFactory):
    def create_line_item(self, name, price):
        return LineItem(name, price)

# Product
class LineItem:
    def __init__(self, name, price):
        self.name = name
        self.price = price

    def __str__(self):
        return f"LineItem(name={self.name}, price={self.price})"

# Usage
factory = ConcreteLineItemFactory()
item = factory.create_line_item("Widget", 19.99)
print(item)  # Outputs: LineItem(name=Widget, price=19.99)
```

### 10.3. Scaling Up: Using Plain Old Python Objects (POPOs)
- Design domain logic as simple, reusable, and testable POPOs.
```python
# Domain logic as a POPO
class BankAccount:
    def __init__(self, account_number, balance=0.0):
        self.account_number = account_number
        self.balance = balance

    def deposit(self, amount):
        self.balance += amount

    def withdraw(self, amount):
        if amount > self.balance:
            raise ValueError("Insufficient funds")
        self.balance -= amount

    def __str__(self):
        return f"BankAccount(account_number={self.account_number}, balance={self.balance})"
```

### 10.4. Using Decorators for Cross-Cutting Concerns
- Apply a logging decorator to add behavior without modifying the original function.
```python
def log_decorator(func):
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__} with args {args} and kwargs {kwargs}")
        result = func(*args, **kwargs)
        print(f"{func.__name__} returned {result}")
        return result
    return wrapper

@log_decorator
def transfer_funds(account_from, account_to, amount):
    account_from.withdraw(amount)
    account_to.deposit(amount)
    return f"Transferred {amount} from {account_from.account_number} to {account_to.account_number}"

# Example usage
account1 = BankAccount("12345", 500)
account2 = BankAccount("67890", 300)
print(transfer_funds(account1, account2, 100))
```

### 10.5. Domain-Specific Languages (DSLs)
- Use Python functions and classes to create a DSL-like API for configuration.
```python
# Simple DSL for configuring a banking system
class Bank:
    def __init__(self, name):
        self.name = name
        self.accounts = []

    def add_account(self, account):
        self.accounts.append(account)

    def __str__(self):
        return f"Bank(name={self.name}, accounts={[str(a) for a in self.accounts]})"

# DSL-like API
def configure_bank():
    bank = Bank("MyBank")
    account1 = BankAccount("12345", 1000)
    account2 = BankAccount("67890", 500)
    bank.add_account(account1)
    bank.add_account(account2)
    return bank

# Configuration
bank = configure_bank()
print(bank)
```

### 10.6. Test-Driven System Architecture
- Writing tests for a modular system with injected dependencies.
```python
import unittest

class TestBankAccount(unittest.TestCase):
    def test_deposit(self):
        account = BankAccount("12345", 100)
        account.deposit(50)
        self.assertEqual(account.balance, 150)

    def test_withdraw_insufficient_funds(self):
        account = BankAccount("12345", 100)
        with self.assertRaises(ValueError):
            account.withdraw(200)

    def test_withdraw_sufficient_funds(self):
        account = BankAccount("12345", 100)
        account.withdraw(50)
        self.assertEqual(account.balance, 50)

if __name__ == "__main__":
    unittest.main()
```

## 11. Emergence
- This section on **Emergent Design Principles** will cover the following:
  - **Runs All the Tests** ensures correctness and supports clean design.
  - **No Duplication** reduces redundancy and improves maintainability.
  - **Expressiveness** makes code readable and easier to maintain.
  - **Minimize Classes and Methods** encourages simplicity without overcomplicating.

### 11.1. Runs All the Tests
- Testing ensures the system behaves as intended and supports clean code principles.
```python
# Example: Writing tests with `unittest`
import unittest

class MathOperations:
    def add(self, a, b):
        return a + b

    def subtract(self, a, b):
        return a - b

class TestMathOperations(unittest.TestCase):
    def setUp(self):
        self.math_ops = MathOperations()

    def test_add(self):
        self.assertEqual(self.math_ops.add(2, 3), 5)

    def test_subtract(self):
        self.assertEqual(self.math_ops.subtract(5, 3), 2)

if __name__ == "__main__":
    unittest.main()
```

### 11.2. No Duplication
- Eliminating duplication improves maintainability. Here’s an example of refactoring to remove duplication.
```python
# Before refactoring
class ImageProcessor:
    def scale_to_dimension(self, desired, current):
        if abs(desired - current) < 0.01:
            return
        scale = desired / current
        self.image = self._apply_scale(scale)

    def rotate_image(self, angle):
        self.image = self._apply_rotation(angle)

    def _apply_scale(self, scale):
        # Scaling logic
        return f"Scaled by {scale}"

    def _apply_rotation(self, angle):
        # Rotation logic
        return f"Rotated by {angle}"

# After refactoring
class ImageProcessorRefactored:
    def process_image(self, transformation, **kwargs):
        self.image = transformation(**kwargs)

    def scale_to_dimension(self, desired, current):
        if abs(desired - current) < 0.01:
            return
        scale = desired / current
        self.process_image(self._apply_scale, scale=scale)

    def rotate_image(self, angle):
        self.process_image(self._apply_rotation, angle=angle)

    def _apply_scale(self, scale):
        return f"Scaled by {scale}"

    def _apply_rotation(self, angle):
        return f"Rotated by {angle}"
```

### 11.3. Expressiveness
- Expressive code is readable, maintainable, and intuitive. Use meaningful names and small, clear methods.
```python
# Expressive naming and small methods
class BankAccount:
    def __init__(self, account_number, balance=0.0):
        self.account_number = account_number
        self.balance = balance

    def deposit(self, amount):
        """Add amount to balance."""
        self.balance += amount

    def withdraw(self, amount):
        """Subtract amount from balance if sufficient funds exist."""
        if amount > self.balance:
            raise ValueError("Insufficient funds")
        self.balance -= amount

    def __str__(self):
        """String representation of the account."""
        return f"BankAccount(account_number={self.account_number}, balance={self.balance})"
```

### 11.4. Template Method
- Avoid duplication by using the Template Method pattern.
```python
from abc import ABC, abstractmethod

# Abstract class
class VacationPolicy(ABC):
    def accrue_vacation(self):
        self.calculate_base_hours()
        self.adjust_for_legal_minimums()
        self.apply_to_payroll()

    def calculate_base_hours(self):
        print("Calculating base vacation hours...")

    @abstractmethod
    def adjust_for_legal_minimums(self):
        pass

    def apply_to_payroll(self):
        print("Applying vacation to payroll...")

# Concrete implementations
class USVacationPolicy(VacationPolicy):
    def adjust_for_legal_minimums(self):
        print("Adjusting for US legal minimums...")

class EUVacationPolicy(VacationPolicy):
    def adjust_for_legal_minimums(self):
        print("Adjusting for EU legal minimums...")

# Usage
us_policy = USVacationPolicy()
us_policy.accrue_vacation()

eu_policy = EUVacationPolicy()
eu_policy.accrue_vacation()
```

### 11.5. Minimal Classes and Methods
- Strive for balance: avoid excessive small methods and classes without compromising clarity or simplicity.
```python
# Example of pragmatic method design
class Calculator:
    def __init__(self):
        self.result = 0

    def calculate(self, operation, *args):
        """Perform the given operation."""
        operations = {
            "add": lambda x, y: x + y,
            "subtract": lambda x, y: x - y,
        }
        if operation not in operations:
            raise ValueError(f"Unsupported operation: {operation}")
        self.result = operations[operation](*args)
        return self.result

# Usage
calc = Calculator()
print(calc.calculate("add", 5, 3))       # 8
print(calc.calculate("subtract", 10, 4))  # 6
```

## 12. Concurrency 
- Concurrency increases complexity and risk.
- By adhering to principles like SRP, limiting shared data, and using established patterns, we can write cleaner, more reliable concurrent code.
- Use appropriate tools, libraries, and testing techniques to manage and mitigate concurrency issues effectively.
### 12.1. Why Concurrency?
- Concurrency decouples _what_ gets done from _when_ it gets done.
- It helps improve throughput and system structure.
- **Example: Concurrent Web Requests:**
```python
import concurrent.futures
import requests

def fetch_url(url):
    response = requests.get(url)
    return f"{url}: {len(response.content)} bytes"

urls = [
    "https://example.com",
    "https://openai.com",
    "https://python.org",
]

with concurrent.futures.ThreadPoolExecutor() as executor:
    results = executor.map(fetch_url, urls)

for result in results:
    print(result)
```

### 12.2. Common Myths
- Concurrency can improve performance but also increases complexity.
- Multithreaded systems must be carefully designed.
- **Example: Mismanaged Shared Resource:**
```python
import threading

class Counter:
    def __init__(self):
        self.value = 0

    def increment(self):
        self.value += 1

counter = Counter()

def task():
    for _ in range(10000):
        counter.increment()

threads = [threading.Thread(target=task) for _ in range(2)]

for thread in threads:
    thread.start()
for thread in threads:
    thread.join()

print("Counter Value:", counter.value)  # Likely less than 20000 due to race condition
```

### 12.3. Single Responsibility Principle
- Concurrency should be separated from other concerns to simplify debugging and testing.
- **Example: Decoupling Concurrency**
```python
import threading

class DataProcessor:
    def process(self, data):
        print(f"Processing {data}...")

class WorkerThread(threading.Thread):
    def __init__(self, processor, data):
        super().__init__()
        self.processor = processor
        self.data = data

    def run(self):
        self.processor.process(self.data)

processor = DataProcessor()
threads = [WorkerThread(processor, f"Data-{i}") for i in range(5)]

for thread in threads:
    thread.start()
for thread in threads:
    thread.join()
```

### 12.4. Avoid Shared Data
- Minimize shared data to reduce synchronization overhead.
- **Example: Using Thread-Local Data**
```python
import threading

thread_local_data = threading.local()

def process_data():
    thread_local_data.value = threading.current_thread().name
    print(f"Thread {threading.current_thread().name} has value: {thread_local_data.value}")

threads = [threading.Thread(target=process_data) for _ in range(5)]

for thread in threads:
    thread.start()
for thread in threads:
    thread.join()
```

### 12.5. Thread Safety
- Use thread-safe data structures and libraries to avoid common pitfalls.
- **Example: Using** `queue.Queue`
```python
import queue
import threading

task_queue = queue.Queue()

def worker():
    while not task_queue.empty():
        task = task_queue.get()
        print(f"{threading.current_thread().name} processing {task}")
        task_queue.task_done()

for i in range(10):
    task_queue.put(f"Task-{i}")

threads = [threading.Thread(target=worker) for _ in range(3)]

for thread in threads:
    thread.start()
for thread in threads:
    thread.join()

task_queue.join()
```

### 12.6. Producer-Consumer
- The producer-consumer pattern is common for balancing workloads.
- **Example: Producer-Consumer with** `queue.Queue`
```python
import threading
import queue
import time

task_queue = queue.Queue()

def producer():
    for i in range(5):
        task_queue.put(f"Task-{i}")
        print(f"Produced Task-{i}")
        time.sleep(0.5)

def consumer():
    while True:
        task = task_queue.get()
        if task is None:
            break
        print(f"Consumed {task}")
        task_queue.task_done()

producer_thread = threading.Thread(target=producer)
consumer_thread = threading.Thread(target=consumer)

producer_thread.start()
consumer_thread.start()

producer_thread.join()
task_queue.put(None)  # Sentinel to stop the consumer
consumer_thread.join()
```

### 12.7. Testing Concurrent Code
- Testing concurrent code requires rigorous validation and instrumentation.
- **Example: Testing with Thread Jiggling**
```python
import threading
import random
import time

class ThreadJigglePoint:
    @staticmethod
    def jiggle():
        if random.choice([True, False]):
            time.sleep(0.01)

def task():
    for _ in range(5):
        print(f"Task running in {threading.current_thread().name}")
        ThreadJigglePoint.jiggle()

threads = [threading.Thread(target=task) for _ in range(3)]

for thread in threads:
    thread.start()
for thread in threads:
    thread.join()
```

## 13. Refactoring
- Refactoring and writing clean code often involve identifying code "smells" and applying heuristics to fix them.
- Below is a summary of key smells and heuristics, aimed at improving code readability, maintainability, and efficiency.
---
## 13.1. Comments
- **1: Inappropriate Information**
  - Comments should not include metadata better suited for tools like version control (e.g., author names, modification dates). Use comments only for technical notes about the code.
- **2: Obsolete Comment**
  - Outdated comments can mislead developers. Remove or update them to maintain relevance.
- **3: Redundant Comment**
  - Avoid comments that state the obvious or merely restate the code (e.g., `i++; // increment i`).
- **4: Poorly Written Comment**
  - Take the time to write concise, grammatically correct comments with meaningful information.
- **5: Commented-Out Code**
  - Delete commented-out code. Use version control to recover it if needed.
---
## 13.2. Environment
- **1: Build Requires More Than One Step**
  - Ensure the build process is a single, simple command.
- **2: Tests Require More Than One Step**
  - Running all tests should also require just one command or click.
---
## 13.3. Functions
- **1: Too Many Arguments**
  - Functions should have a small number of arguments. Prefer 0–3 arguments.
- **2: Output Arguments**
  - Avoid using output arguments; they can confuse readers. Change the state of the object instead.
- **3: Flag Arguments**
  - Boolean flags often indicate a function is doing too much. Split the functionality into separate functions.
- **4: Dead Function**
  - Remove unused functions. Use version control to retrieve them if necessary.
---
## 13.4. General Principles
- **1: Multiple Languages in One Source File**
  - Minimize mixing languages (e.g., Java with XML, HTML, or JavaScript) in a single file.
- **2: Obvious Behavior Is Unimplemented**
  - Implement behaviors that users would reasonably expect (e.g., case-insensitive string comparisons).
- **3: Incorrect Behavior at the Boundaries**
  - Test and validate all edge cases to ensure correct behavior.
- **4: Overridden Safeties**
  - Avoid overriding safety mechanisms (e.g., turning off compiler warnings or ignoring failing tests).
- **5: Duplication**
  - Follow the DRY principle: avoid code duplication by using abstractions or design patterns.
- **6: Code at Wrong Level of Abstraction**
  - Separate high-level concepts from low-level details.
- **7: Base Classes Depending on Their Derivatives**
  - Base classes should not depend on their derived classes.
- **8: Too Much Information**
  - Keep interfaces small. Limit what is exposed to reduce coupling.
- **9: Dead Code**
  - Remove code that is no longer executed or relevant.
- **10: Vertical Separation**
  - Declare variables close to where they are used. Define private functions near their first usage.
- **11: Inconsistency**
  - Be consistent in naming and formatting conventions.
- **12: Clutter**
  - Remove unused variables, functions, and comments.
- **13: Artificial Coupling**
  - Avoid unnecessary dependencies between unrelated modules.
- **14: Feature Envy**
  - Methods should focus on their own class's data and functionality, not another class's.
---
## 13.5. Names
- **1: Choose Descriptive Names**
  - Use meaningful names that reflect the purpose of variables and functions.
- **2: Avoid Encodings**
  - Avoid adding type or scope information in variable names (e.g., `m_variableName`).
- **3: Use Long Names for Long Scopes**
  - Use concise names for short-lived variables but longer, descriptive names for those with broader scopes.
- **4: Names Should Describe Side-Effects**
  - Function names should clearly state all their effects.
---
## 13.6. Tests
- **1: Insufficient Tests**
  - Ensure all potential edge cases and failures are covered.
- **2: Use a Coverage Tool**
  - Use tools to measure and improve test coverage.
- **3: Don’t Skip Trivial Tests**
  - Even simple tests provide value as documentation and validation.
- **4: Test Boundary Conditions**
  - Pay special attention to edge cases, as they often cause errors.
---
  






