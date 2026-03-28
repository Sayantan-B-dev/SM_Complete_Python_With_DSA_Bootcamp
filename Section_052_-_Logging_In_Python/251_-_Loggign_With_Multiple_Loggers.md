## Understanding Multiple Loggers in Python – Mental Model and Step-by-Step Explanation

When you create multiple loggers in Python, you are tapping into a **hierarchical logging system** where loggers are named and arranged in a tree. The root logger sits at the top, and all other loggers are children. This structure allows fine‑grained control over what gets logged, where it goes, and at what severity.

---

### 1. The Logger Hierarchy

Every logger has a **name** (e.g., `"module1"`, `"module2"`). Names are hierarchical using dot notation (e.g., `"parent.child"`). The root logger is named `""` (empty string). All other loggers are descendants of the root.

- **Root logger**: the ultimate ancestor; it has no parent.
- **Child loggers**: inherit settings (like level) from their parent **unless overridden**. They also propagate log messages to their parent’s handlers by default.

**Visual representation**:

```
        root (level = NOTSET)
       /     \
module1      module2
(level=DEBUG) (level=WARNING)
```

---

### 2. What Does `basicConfig` Do?

`logging.basicConfig()` is a convenience function that **configures the root logger** in a simple way. It:

- Sets the root logger’s level (if `level` is provided).
- Optionally creates a `StreamHandler` (console) or a `FileHandler` (if `filename` is given) and attaches it to the root logger.
- Sets a formatter for that handler.

**Important**: If the root logger already has any handlers, `basicConfig()` does nothing (unless `force=True` is used). This is why you typically call it **once at the beginning** of your program.

In the example code, `basicConfig` is called **after** creating the custom loggers. That’s fine because the root logger initially has no handlers, so the call adds a console handler to the root.

---

### 3. Creating Custom Loggers with `getLogger`

```python
logger1 = logging.getLogger("module1")
logger2 = logging.getLogger("module2")
```

`getLogger(name)` returns a logger with the given name. If it doesn’t exist, it creates a new one and automatically links it to its parent. The name determines its place in the hierarchy:

- `"module1"` is a child of the root.
- `"module2"` is also a child of the root.

These custom loggers are **independent objects** but share the same underlying system. They have their own level settings and can have their own handlers.

---

### 4. Setting Levels on Custom Loggers

```python
logger1.setLevel(logging.DEBUG)
logger2.setLevel(logging.WARNING)
```

`setLevel()` sets the **logger’s internal threshold**. Any message with a severity lower than this level is **ignored** by that logger **before** any further processing (handlers, propagation). This acts as a filter.

- For `logger1`: it will accept DEBUG, INFO, WARNING, ERROR, CRITICAL.
- For `logger2`: it will accept only WARNING, ERROR, CRITICAL.

**If you don’t set a level on a custom logger**, it inherits the level from its parent (or from the root if no parent has been set). The root logger’s level is initially `NOTSET`, which means “process all messages”. After `basicConfig`, the root’s level is set to `DEBUG` (or whatever you passed). So by default, custom loggers would also allow all messages unless you explicitly override them.

---

### 5. Propagation – How Messages Travel

When you call `logger1.debug("...")`:

1. **Logger level check**: Does `logger1` have a level set? If yes, and the message level is below that, it is discarded. Here `DEBUG` is allowed.
2. **Processing**: The logger processes the message. It may have its own handlers. In our example, `logger1` has **no custom handlers**, so the message is not yet output.
3. **Propagation**: By default, after processing its own handlers, the logger passes the message to its parent (the root). This is called propagation.
4. **Root logger**: The root logger has a handler (the console handler from `basicConfig`). It performs its own level check (which is `DEBUG`) and then the handler outputs the formatted message.

**Because propagation is on**, the message from `logger1` ends up being printed by the root’s handler. Similarly, `logger2.warning()` and `logger2.error()` are propagated.

**Why propagate?** It allows you to have a global output (like a console) for all logs while still being able to add module‑specific handlers (like writing DEBUG logs of `module1` to a separate file). Propagation can be disabled by setting `logger1.propagate = False`.

---

### 6. Mental Model – The Why Behind Each Step

Let’s break down the sequence of actions and the reasoning:

| Step | Action | Why |
|------|--------|-----|
| 1 | Create `logger1` and `logger2` | You want to separate log messages from different parts of your application. This makes it easier to filter, route, and manage logs. |
| 2 | Set `logger1.setLevel(logging.DEBUG)` | You want to see very detailed messages from module1 during development or troubleshooting. |
| 3 | Set `logger2.setLevel(logging.WARNING)` | Module2 is more stable; you only care about warnings and above to reduce noise. |
| 4 | Call `logging.basicConfig(...)` | You want a **global configuration** for the root logger. This ensures that even if you don’t add custom handlers to your loggers, they still output to the console (or file) with a consistent format and timestamp. The level you pass to `basicConfig` affects the root’s threshold; since propagation is on, it will also affect which messages from children are printed (but the children’s own levels already filter before propagation). |
| 5 | Log messages via `logger1.debug`, `logger2.warning`, etc. | You emit log messages at appropriate levels. The logger’s own level filters them first, then they propagate to the root where the handler outputs them. |

**Key insight**: The root logger acts as the default sink. Custom loggers allow you to:

- Control verbosity per module (via `setLevel`).
- Add module‑specific handlers (e.g., a file handler that only writes `module1` logs to a separate file).
- Separate log streams while still having a global output.

---

### 7. What If You Add Handlers to Custom Loggers?

If you later add a handler directly to `logger1`:

```python
fh = logging.FileHandler('module1.log')
logger1.addHandler(fh)
```

Now when you call `logger1.debug(...)`:

- The logger’s level is checked (DEBUG passes).
- It processes its own handlers: the file handler writes to `module1.log`.
- It propagates to the root, which also writes to the console (if the root has a handler).

Thus you get **both** a module‑specific file and the global console output. If you want to avoid duplication, you can set `logger1.propagate = False` so messages do not go to the root.

---

### 8. Summary of Concepts

| Concept | Explanation |
|---------|-------------|
| **Logger** | An object that records log messages. Has a name, level, and list of handlers. |
| **Root logger** | The top‑level logger; the ancestor of all loggers. |
| **Logger hierarchy** | Loggers are arranged by dotted names (e.g., `"parent.child"`). Child loggers inherit from parents. |
| **Level** | A threshold that filters messages. If the message level is lower than the logger’s level, it is discarded. |
| **Handler** | An object that decides where to output a log record (console, file, network, etc.). |
| **Formatter** | An object that formats the log record into a string (timestamp, level, message, etc.). |
| **Propagation** | By default, loggers pass messages to their parent after processing their own handlers. This allows a single root handler to capture everything. |
| **`basicConfig`** | A shortcut to configure the root logger with a handler and formatter. It is called once at startup. |

---

### 9. Complete Example with Commentary

```python
import logging

# Step 1: Create custom loggers
logger1 = logging.getLogger("module1")
logger2 = logging.getLogger("module2")

# Step 2: Set their levels (filters)
logger1.setLevel(logging.DEBUG)   # module1: everything
logger2.setLevel(logging.WARNING) # module2: warnings and above

# Step 3: Configure the root logger
# This adds a console handler with the specified format and sets root level to DEBUG
logging.basicConfig(
    level=logging.DEBUG,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
    datefmt='%Y-%m-%d %H:%M:%S'
)

# Step 4: Emit messages
logger1.debug("Debug message from module1")    # Will appear (DEBUG >= logger1.DEBUG, root level allows)
logger2.debug("Debug message from module2")    # Ignored by logger2 (DEBUG < WARNING)
logger2.warning("Warning from module2")        # Appears (WARNING >= logger2.WARNING)
logger2.error("Error from module2")            # Appears
```

**Output**:
```
2025-03-28 12:34:56 - module1 - DEBUG - Debug message from module1
2025-03-28 12:34:56 - module2 - WARNING - Warning from module2
2025-03-28 12:34:56 - module2 - ERROR - Error from module2
```

Notice that the `logger2.debug` is completely silent because it was filtered out at the logger level.

---

### 10. When to Use Multiple Loggers – Real‑World Use Cases

- **Per‑module loggers**: In a large application, each module gets its own logger (e.g., `logger = logging.getLogger(__name__)`). This allows you to see which module produced which message.
- **Different verbosity levels**: You can set `DEBUG` for a module under development, while keeping `INFO` for stable modules.
- **Different destinations**: You can route logs from critical modules to a separate file or even email them.
- **Organized logs**: With proper naming (e.g., `"app.database"`, `"app.web"`), you can control logging for entire subsystems by setting a level on a parent logger (e.g., `logging.getLogger("app.database").setLevel(logging.WARNING)`).

---

### 11. Key Takeaways

- **`getLogger(name)`** creates or retrieves a logger. The name establishes hierarchy.
- **`setLevel`** on a logger controls what that logger will accept.
- **`basicConfig`** sets up the root logger’s handlers and level. It should be called once at program start.
- **Propagation** ensures that messages flow from child loggers to the root (and beyond) by default. This is why you often see output even without adding handlers to custom loggers.
- **The mental model**: Think of the root logger as the central hub, and custom loggers as branches. You can attach extra handlers and filters to branches, but the hub will still receive everything unless you cut the connection (`propagate=False`).

