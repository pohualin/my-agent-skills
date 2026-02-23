---
description: 'Python coding conventions and guidelines'
applyTo: '**/*.py'
---


# Python Coding Conventions & Best Practices
# Modern Python Scripting Recommendations

## Path Handling
- Prefer using `pathlib.Path` for all file and directory path manipulations instead of plain strings or `os.path` functions.
- Use `.parent`, `.mkdir()`, `.exists()`, and other Path methods for clarity and cross-platform compatibility.
- Convert Path to string with `str(path)` only when required by legacy APIs.

## Type Annotations and Docstrings
- Add type hints to all function signatures using the `typing` module (e.g., `List[str]`, `Dict[str, int]`).
- Write PEP 257-compliant docstrings for all functions and modules.

## Error Handling
- Use try/except blocks around network or file operations to handle errors gracefully and log them.

## Imports
- Place all imports at the top of the file, except for those required only in specific functions (e.g., argparse in main()).

## Logging
- Use the shared logging setup as described above.
- Log key actions, errors, and output file paths for traceability.

## Progress Feedback
- For scripts processing many records, consider using a progress bar (e.g., with `tqdm`) for user feedback.

## Early Returns
- Return early from functions if there is nothing to process (e.g., no records), to avoid unnecessary work.

## 1. Utility Reuse
- Before creating any new utility functions or modules, always check the `python_util` library for existing solutions. Reuse and extend shared utilities whenever possible to ensure consistency and reduce duplication across scripts.


## 2. Script Initialization Sequence

**Always follow this import and setup order in scripts:**

```python
from python_util.config.env_loader import load_env
load_env()
import logging
from python_util.config.logging_config import setup_logging
setup_logging()
logger = logging.getLogger(__name__)
```

- Only call `setup_logging()` in the main script, not in imported modules.
- In modules that can be both imported and run directly, call `setup_logging()` inside `if __name__ == "__main__":`.
- Never configure logging in library modules; let the main script handle it.
- This approach is robust, avoids side effects, and is suitable for real-world, production-quality Python code.
 
# Python Coding Conventions

## Python Instructions

- Write clear and concise comments for each function.
- Ensure functions have descriptive names and include type hints.
- Provide docstrings following PEP 257 conventions.
- Use the `typing` module for type annotations (e.g., `List[str]`, `Dict[str, int]`).
- Break down complex functions into smaller, more manageable functions.

## General Instructions

- Always prioritize readability and clarity.
- For algorithm-related code, include explanations of the approach used.
- Write code with good maintainability practices, including comments on why certain design decisions were made.
- Handle edge cases and write clear exception handling.
- For libraries or external dependencies, mention their usage and purpose in comments.
- Use consistent naming conventions and follow language-specific best practices.
- Write concise, efficient, and idiomatic code that is also easily understandable.

## Code Style and Formatting

- Follow the **PEP 8** style guide for Python.
- Maintain proper indentation (use 4 spaces for each level of indentation).
- Ensure lines do not exceed 79 characters.
- Place function and class docstrings immediately after the `def` or `class` keyword.
- Use blank lines to separate functions, classes, and code blocks where appropriate.

## Edge Cases and Testing

- Always include test cases for critical paths of the application.
- Account for common edge cases like empty inputs, invalid data types, and large datasets.
- Include comments for edge cases and the expected behavior in those cases.
- Write unit tests for functions and document them with docstrings explaining the test cases.

## Example of Proper Documentation

```python
def calculate_area(radius: float) -> float:
    """
    Calculate the area of a circle given the radius.
    
    Parameters:
    radius (float): The radius of the circle.
    
    Returns:
    float: The area of the circle, calculated as π * radius^2.
    """
    import math
    return math.pi * radius ** 2
```
