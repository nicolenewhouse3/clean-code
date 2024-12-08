## Introduction 
...

## 1. Naming 
- Good naming requires descriptive skills and cultural understanding.
- Regularly refactor code to improve naming and readability.
- Clear, intentional names make code easier to understand and maintain over the long term.
  
### 1.1.  Use Intention-Revealing Names
- Names should convey purpose, usage, and meaning. Avoid ambiguous or generic names.
- Replace cryptic names with descriptive ones that do not require additional comments.
```python
# Good
elapsed_time_in_days = 7
days_since_creation = 10

# Bad
d = 7  # elapsed time
```

### 1.2. Make Meaningful Distinctions
- Avoid arbitrary changes, like adding numbers or noise words.
- Use names that highlight the purpose and differences between entities.
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

### 1.3. Avoid Disinformation
- Avoid misleading names or words with established meanings that don’t align with your intent.
- Ensure names clearly differentiate between similar concepts.
```python
# Good
account_group = [account1, account2]

# Bad
account_list = {key: value}  # Misleading, not a list
```

### 1.4. Use Pronounceable Names
- Choose names that are easy to pronounce.
- This helps facilitate clear communication during discussions and to reduce onboarding time for new developers.
```python
# Good
pswdExpDt = "2024-12-31"

# Bad
password_expiration_date = "2024-12-31"
```

### 1.4. Use Searchable Names
- Avoid single-letter names or numeric constants that are difficult to locate.
- Use descriptive, searchable names.
```python
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

### 1.5. Avoid Encodings
- Do not add unnecessary type or scope encodings (e.g., m_ or Hungarian Notation).
- Modern tools and languages provide better ways to distinguish variable types and scope.
```python
# Good
class Part:
    def __init__(self):
        self.description = ""

# Bad
class Part:
    def __init__(self):
        self.m_description = ""
```

### 1.6. Avoid Mental Mapping
- Use clear, explicit names instead of abstract placeholders or single letters.
```python
# Good
url_without_host = "example.com/path"

# Bad
r = "example.com/path"  # What does r mean?
```

### 1.7. Add Meaningful Context
- Provide context through class, function, or variable names. Avoid gratuitous prefixes or unnecessary context.
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

### 1.8. Don’t Be Cute or Pun
- Avoid clever or humorous names that may not be clear to others.
```python
# Good
def delete_items():
    pass

# Bad
def holy_hand_grenade():
    pass
```

## 2. Functions
- Functions are the building blocks of clean code and should be small, focused, and descriptive.
- Strive for simplicity, eliminate duplication, and write code that reads like a well-structured narrative. Functions are not just code—they are the verbs of your domain-specific language.
  
### 2.1 Keep Functions Small
- Functions should be as short as possible. Ideally, they should fit within a few lines.
- Each function should perform one task only, focusing on clarity and brevity.
```python
# Good
def render_page(page_data):
    if is_test_page(page_data):
        include_setup_and_teardown_pages(page_data)
    return page_data.get_html()

# Bad
def render_page(page_data):
    if page_data.has_attribute("Test"):
        # Long, complex operations for setup and teardown
        setup_pages = fetch_setup_pages()
        teardown_pages = fetch_teardown_pages()
        page_data.set_content(setup_pages + page_data.get_content() + teardown_pages)
    return page_data.get_html()
```

### 2.2. Do One Thing
- Functions should do one thing and do it well. Avoid combining unrelated operations.
- If a function can be described with a single verb or action, it’s likely doing one thing.
```python
# Good
def include_setup_and_teardown_pages(page_data):
    include_setup_pages(page_data)
    include_teardown_pages(page_data)

# Bad
def include_pages(page_data):
    include_setup_pages(page_data)
    include_teardown_pages(page_data)
    render_html(page_data)
```

### 2.3. Use One Level of Abstraction
- Functions should stay at a single level of abstraction.
- Avoid mixing high-level logic with low-level details in the same function.
```python
# Good
def include_setup_pages(page_data):
    suite_setup = find_suite_setup(page_data)
    if suite_setup:
        append_setup(page_data, suite_setup)

# Bad
def include_setup_pages(page_data):
    suite_setup = PageCrawler.get_inherited_page("SuiteSetup", page_data.wiki_page)
    if suite_setup:
        setup_path = suite_setup.get_page_crawler().get_full_path(suite_setup)
        page_data.buffer.append(f"!include {PathParser.render(setup_path)}")
```

### 2.4. Use Descriptive Names
- Function names should clearly describe their purpose. Long, descriptive names are preferable to short, cryptic ones.
- Avoid ambiguous terms and ensure consistency across similar functions.
```python
# Good
def include_setup_pages():
    pass

# Bad
def process_setups():
    pass
```

### 2.5. Minimize Function Arguments
- Ideal number of arguments: 0 (niladic), 1 (monadic), or 2 (dyadic). Avoid triadic or polyadic arguments when possible.
- Use argument objects if more than two arguments are required.
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
def render_for_suite(page_data):
    pass

def render_for_test(page_data):
    pass

# Bad
def render_page(page_data, is_suite):
    pass
```

### 2.7. Separate Command and Query Functions
- Functions should either perform an action (command) or return information (query), but not both.
```python
# Good
if attribute_exists("username"):
    set_attribute("username", "john_doe")

# Bad
if set_attribute("username", "john_doe"):
    pass
```

### 2.8. Use Exceptions Instead of Error Codes
- Avoid returning error codes. Use exceptions to separate normal flow from error handling.
```python
# Good
try:
    delete_page(page)
except Exception as e:
    log_error(e)

# Bad
if delete_page(page) == "ERROR":
    log_error("Failed to delete page")
```

### 2.9. Avoid Duplication
- Eliminate repeated code by abstracting common logic into reusable functions.
```python
# Good
def include(page, type):
    page_path = fetch_page_path(type)
    if page_path:
        buffer.append(f"!include -{type} {page_path}")

# Bad
def include_setup_page():
    setup_path = fetch_setup_path()
    if setup_path:
        buffer.append(f"!include -setup {setup_path}")

def include_teardown_page():
    teardown_path = fetch_teardown_path()
    if teardown_path:
        buffer.append(f"!include -teardown {teardown_path}")
```

### 2.10. Error Handling is One Thing
- Functions that handle errors should do only that. Extract error-handling logic into separate functions.
```python
# Good
def delete_page_with_error_handling(page):
    try:
        delete_page(page)
    except Exception as e:
        log_error(e)

# Bad
def delete_page(page):
    try:
        # Delete logic
    except Exception as e:
        log_error(e)
```

## 3. Comments
- Comments are a necessary evil, not a hallmark of good code. They often:
  - Become outdated and misleading.
  - Signal failures to express intent through code.
  - Always strive to rewrite code for clarity instead of relying on comments.

### 3.1. When Comments are Necessary
#### 3.1.1. Informative Comments
- Provide essential information that cannot be expressed directly in code.
```python
# Matches "Tue, 02 Apr 2003 22:18:49 GMT"
http_date_pattern = re.compile(
    r"[SMTWF][a-z]{2},\s[0-9]{2}\s[JFMASOND][a-z]{2}\s[0-9]{4}\s[0-9]{2}:[0-9]{2}:[0-9]{2}\sGMT"
)
```

#### 3.1.2. Explanation of Intent
- Use comments to clarify why decisions were made.
```python
# Use a thread-local date formatter to avoid concurrency issues
from threading import local

thread_local_date_format = local()
def get_date_format():
    if not hasattr(thread_local_date_format, "formatter"):
        thread_local_date_format.formatter = "%Y-%m-%d"
    return thread_local_date_format.formatter
```

#### 3.1.3. Warnings About Consequences
- Warn developers of potential pitfalls or consequences.
```python
# SimpleDateFormat is not thread-safe; create a new instance for each thread
date_format = "%Y-%m-%d"
```

#### 3.1.4. TODO Comments
- Mark areas for future work clearly.
```python
# TODO: Optimize this function for large datasets
def calculate_primes(limit):
    pass
```

#### 3.1.5. Amplification
- Emphasize critical details that might otherwise seem minor.
```python
# The trim is crucial to avoid issues with leading/trailing spaces
list_item = text.strip()
```

#### 3.1.6. Public API Documentation
- Use docstrings for public-facing APIs.
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
#### 3.2.1. Redundant Comments
- Avoid comments that restate what the code already expresses.
```python
# Adds two numbers
result = a + b  # Redundant and unnecessary
```

#### 3.2.2. Misleading Comments
- Ensure comments accurately describe the code.
```python
# Returns True if the list is empty
return len(my_list) > 0  # Misleading; this checks if the list is non-empty
```

#### 3.2.3. Mandated Comments
- Avoid comments added solely for compliance purposes.
```python
# The name of the user
user_name = "John"  # Pointless redundancy
```

#### 3.2.4. Journal Comments
- Use version control instead of journal entries.
```python
# Added by Eric on 2024-01-15
```

#### 3.2.5. Commented-Out Code
- Remove commented-out code; rely on version control.
```python
# Old implementation
# result = calculate_old_way(x, y)
# return result
```

#### 3.2.6. Noisy Comments
- Avoid comments that add no value.
```python
# Returns the day of the month
def get_day_of_month():
    return day_of_month 
```

### 3.3. Rewrite Instead of Commenting
- Use **descriptive function names** and **variables** to make comments unnecessary.
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
- Clean code is both readable and robust. Error handling should be treated as a separate concern, allowing the main logic to remain clear and focused.
- By organizing error handling independently, developers can reason about it more effectively, leading to greater maintainability and fewer bugs.
### 6.1. Use Exceptions Rather Than Return Codes




