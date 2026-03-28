## Python Logging – Complete Guide

Logging is an essential part of any application. It helps you understand what your code is doing, track errors, and monitor performance. Python’s built‑in `logging` module provides a flexible and powerful framework for emitting log messages. This guide covers everything from basic configuration to advanced usage.

---

### 1. Importing and Basic Configuration

```python
import logging

# Configure the root logger
logging.basicConfig(level=logging.DEBUG)

# Log messages with different severity levels
logging.debug("This is a debug message")
logging.info("This is an info message")
logging.warning("This is a warning message")
logging.error("This is an error message")
logging.critical("This is a critical message")
```

**Output** (printed to console, default format):
```
DEBUG:root:This is a debug message
INFO:root:This is an info message
WARNING:root:This is a warning message
ERROR:root:This is an error message
CRITICAL:root:This is a critical message
```

---

### 2. Log Levels – Severity and Meaning

| Level     | Numeric Value | When to Use |
|-----------|---------------|-------------|
| `DEBUG`   | 10            | Detailed information, typically of interest only when diagnosing problems. Use during development or troubleshooting. |
| `INFO`    | 20            | Confirmation that things are working as expected. Use for normal operation events (e.g., “Server started”, “User logged in”). |
| `WARNING` | 30            | An indication that something unexpected happened, or a potential problem in the near future. The software is still working as expected. |
| `ERROR`   | 40            | Due to a more serious problem, the software has not been able to perform some function. Use for recoverable errors. |
| `CRITICAL`| 50            | A very serious error, indicating that the program itself may be unable to continue running. Use for fatal failures. |

**How logging works**: The logger only outputs messages with a severity level **greater than or equal to** the set threshold. For example, if `level=logging.WARNING`, only WARNING, ERROR, and CRITICAL messages are shown.

---

### 3. `basicConfig` Parameters – Everything You Can Configure

`logging.basicConfig(**kwargs)` is a convenience function that configures the root logger. It should be called **only once** (preferably at the start of your program). If called again, it has no effect unless the root logger already has handlers.

#### Important Parameters

| Parameter   | Description |
|-------------|-------------|
| `filename`  | Specifies a file to write log messages to. If omitted, logs go to the console (stderr). |
| `filemode`  | File open mode, e.g., `'a'` for append (default) or `'w'` for overwrite. Only used when `filename` is set. |
| `format`    | A string defining the log message format. Uses `%(attribute)s` placeholders (see below). |
| `datefmt`   | Format for the timestamp in `%(asctime)s`. Uses the same format as `time.strftime`. |
| `level`     | Sets the threshold level for the root logger (e.g., `logging.DEBUG`). |
| `stream`    | Specifies a stream (e.g., `sys.stdout`) to write logs to. Cannot be used with `filename`. |
| `handlers`  | A list of handlers to add to the root logger. Overrides `filename`/`stream`. |
| `encoding`  | Encoding for the file if `filename` is used (Python 3.9+). |
| `force`     | If `True`, any existing handlers are removed before configuration (Python 3.8+). |

#### Format Placeholders (Common Attributes)

| Attribute   | Meaning |
|-------------|---------|
| `%(asctime)s` | Human‑readable timestamp (controlled by `datefmt`). |
| `%(name)s`   | Name of the logger (default is `root`). |
| `%(levelname)s` | Log level name (DEBUG, INFO, etc.). |
| `%(message)s` | The log message. |
| `%(filename)s` | Filename where the log call occurred. |
| `%(lineno)d`  | Line number in the source file. |
| `%(funcName)s` | Function name where the log call occurred. |
| `%(process)d` | Process ID. |
| `%(thread)d`  | Thread ID. |

#### Example of a Complete `basicConfig`

```python
import logging

logging.basicConfig(
    filename='app.log',          # Write logs to this file
    filemode='w',                # Overwrite the file each run (use 'a' to append)
    level=logging.DEBUG,         # Capture all levels DEBUG and above
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
    datefmt='%Y-%m-%d %H:%M:%S'
)

logging.info("Application started")
logging.debug("This is a debug message")
```

**Output in `app.log`**:
```
2025-03-28 10:15:30 - root - INFO - Application started
2025-03-28 10:15:30 - root - DEBUG - This is a debug message
```

---

### 4. Date Format – Customizing Timestamps

The `datefmt` parameter uses the same format codes as `time.strftime`. Common codes:

| Code | Meaning            | Example           |
|------|--------------------|-------------------|
| `%Y` | Year with century  | 2025              |
| `%m` | Month as zero‑padded | 03                |
| `%d` | Day of month       | 28                |
| `%H` | Hour (24‑hour)     | 14                |
| `%M` | Minute             | 05                |
| `%S` | Second             | 30                |

Example: `datefmt='%Y-%m-%d %H:%M:%S'` yields `2025-03-28 14:05:30`.

---

### 5. Logging to a File – Why and How

Writing logs to a file is essential for persistent storage, debugging after the fact, and monitoring long‑running applications. Use `filename` and `filemode`.

```python
logging.basicConfig(
    filename='myapp.log',
    filemode='a',          # Append to existing log file
    level=logging.INFO,
    format='%(asctime)s - %(levelname)s - %(message)s'
)
```

**Note**: If you want to rotate log files (e.g., split by size or time), you need to use a `RotatingFileHandler` or `TimedRotatingFileHandler` instead of `basicConfig`.

---

### 6. Restarting Kernel / Reconfiguring Logging

In interactive environments like Jupyter, calling `basicConfig` again after it has already been set may not work because the root logger already has handlers. To reconfigure, you can:

1. **Use the `force` parameter** (Python 3.8+):
   ```python
   logging.basicConfig(..., force=True)
   ```
2. **Remove existing handlers manually**:
   ```python
   for handler in logging.root.handlers[:]:
       logging.root.removeHandler(handler)
   logging.basicConfig(...)
   ```

---

### 7. Using Logging Across a Project – Best Practices

For larger projects, you should not rely on the root logger. Instead, create named loggers for each module. This gives you finer control.

#### Step 1: Create a separate configuration file (e.g., `logger_config.py`)

```python
import logging

def setup_logger(name, log_file='app.log', level=logging.INFO):
    """Set up a logger with file and console handlers."""
    formatter = logging.Formatter(
        '%(asctime)s - %(name)s - %(levelname)s - %(message)s',
        datefmt='%Y-%m-%d %H:%M:%S'
    )

    # File handler
    file_handler = logging.FileHandler(log_file)
    file_handler.setFormatter(formatter)

    # Console handler
    console_handler = logging.StreamHandler()
    console_handler.setFormatter(formatter)

    # Get logger and set level
    logger = logging.getLogger(name)
    logger.setLevel(level)
    logger.addHandler(file_handler)
    logger.addHandler(console_handler)

    return logger
```

#### Step 2: Use the logger in your modules

```python
from logger_config import setup_logger

logger = setup_logger(__name__, log_file='myapp.log', level=logging.DEBUG)

def some_function():
    logger.debug("Entering some_function")
    # ... code ...
    logger.info("Operation completed")
```

**Benefits**:
- Each module has its own logger identified by `__name__`.
- You can control logging levels independently (e.g., set `DEBUG` for one module, `INFO` for another).
- Handlers can be added or removed without affecting other modules.

---

### 8. Advanced: Handlers, Formatters, and Filters

`basicConfig` is just a shortcut. For more advanced setups, you can directly create handlers and formatters.

```python
import logging

# Create logger
logger = logging.getLogger('myapp')
logger.setLevel(logging.DEBUG)

# Create file handler
file_handler = logging.FileHandler('app.log')
file_handler.setLevel(logging.INFO)

# Create console handler
console_handler = logging.StreamHandler()
console_handler.setLevel(logging.DEBUG)

# Create formatter and attach to handlers
formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
file_handler.setFormatter(formatter)
console_handler.setFormatter(formatter)

# Add handlers to logger
logger.addHandler(file_handler)
logger.addHandler(console_handler)

# Use the logger
logger.debug("This goes only to console (DEBUG)")
logger.info("This goes to both file and console (INFO)")
```

**Filters** allow you to add additional criteria for which log records are emitted. For example, you could create a filter that only logs messages containing a certain keyword.

---

### 9. When to Use Each Log Level – Real-World Examples

- **DEBUG**: “Loaded configuration from /etc/myapp.conf”, “User entered password length = 8” – use only during development.
- **INFO**: “Server started on port 8080”, “User 'john' logged in”, “Request processed in 120 ms” – for normal operation.
- **WARNING**: “Disk usage is 85%”, “Retrying connection (attempt 2)”, “Deprecated API called” – something may need attention.
- **ERROR**: “Failed to connect to database”, “File not found: data.csv”, “Payment processing failed” – a function could not complete.
- **CRITICAL**: “Uncaught exception causing program termination”, “Out of memory”, “Security breach detected” – immediate action required.

---

### 10. Summary

- **`logging`** is the standard Python module for logging.
- **Log levels** (DEBUG, INFO, WARNING, ERROR, CRITICAL) help categorize severity.
- **`basicConfig`** provides a quick way to set up the root logger with file/console output, formatting, and level.
- **Formatting** with `%(asctime)s`, `%(levelname)s`, etc., gives control over log appearance.
- **File logging** is essential for persistence; use `filename` and `filemode`.
- **In notebooks**, use `force=True` or remove handlers to reconfigure.
- **For projects**, create named loggers and configure handlers explicitly.
- **Advanced** handlers (e.g., rotating files, email) are available for production systems.

By mastering these concepts, you can create applications that are easy to debug, monitor, and maintain.