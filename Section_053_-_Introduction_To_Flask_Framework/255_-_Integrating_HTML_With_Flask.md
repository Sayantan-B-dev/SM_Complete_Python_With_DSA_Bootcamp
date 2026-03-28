# Integrating HTML with Flask: Templates, Jinja2, and Static Files

This document addresses the integration of HTML into Flask applications, focusing on the `render_template` function, the Jinja2 template engine, and the proper organization of templates and static assets. It explains why returning raw HTML strings is discouraged, how `render_template` leverages Jinja2, and how to build a static multi‑page website with a consistent centered layout.

---

## 1. The Problem with Raw HTML Strings

In the provided example:

```python
@app.route("/")
def welcome():
    return "<html><H1>Welcome to the flask course</H1></html>"
```

While functional, this approach suffers from:

- **Maintainability** – HTML is embedded in Python, mixing presentation with logic.
- **Reusability** – Common elements (headers, footers) cannot be shared across pages.
- **Security** – Manual string concatenation for dynamic content risks cross‑site scripting (XSS).
- **Scalability** – As pages grow, managing HTML inside Python becomes unwieldy.

Flask solves these by separating presentation into **template files** processed by the **Jinja2 template engine**.

---

## 2. The `render_template` Function

### 2.1 Purpose

`render_template` is a Flask function that:

1. Locates a template file in the **templates folder**.
2. Uses Jinja2 to process the template, substituting variables passed as keyword arguments.
3. Returns a fully rendered HTML string (wrapped in a `Response` object).

**Signature:**
```python
render_template(template_name_or_list, **context)
```

- `template_name_or_list` – Name(s) of the template file(s).
- `**context` – Variables to be passed into the template.

### 2.2 Why `render_template`? – The Role of Jinja2

`render_template` is the bridge to Jinja2, which provides:

- **Template inheritance** – Define a base layout and extend it.
- **Control structures** – Conditionals, loops, macros.
- **Automatic escaping** – Prevents XSS by escaping variable output.
- **Filters** – Transform data (e.g., `{{ name|upper }}`).

Without Jinja2, the “normal file way” would involve manually reading HTML files and substituting placeholders – a fragile and insecure process. `render_template` abstracts this complexity, enabling clean separation of concerns.

---

## 3. The Templates Folder

By default, Flask expects templates in a folder named `templates` at the application root. This folder can be customized via the `template_folder` parameter:

```python
app = Flask(__name__, template_folder='my_templates')
```

When `render_template('index.html')` is called, Flask searches for `templates/index.html`. If the file is missing, a `TemplateNotFound` error is raised.

---

## 4. Creating Multiple Routes with Different Templates

Each route can render its own template. Example:

```python
from flask import Flask, render_template

app = Flask(__name__)

@app.route("/")
def home():
    return render_template('home.html')

@app.route("/index")
def index():
    return render_template('index.html')

@app.route("/about")
def about():
    return render_template('about.html')

if __name__ == "__main__":
    app.run(debug=True)
```

Corresponding templates would reside in the `templates/` directory.

---

## 5. Building a Static Website with a Centered Layout

To create a consistent, centered appearance across pages, use a **base template** and **CSS static files**.

### 5.1 Folder Structure

```
project/
├── app.py
├── templates/
│   ├── base.html
│   ├── home.html
│   ├── index.html
│   └── about.html
└── static/
    └── css/
        └── style.css
```

### 5.2 Base Template (`templates/base.html`)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>{% block title %}My Site{% endblock %}</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='css/style.css') }}">
</head>
<body>
    <nav>
        <a href="{{ url_for('home') }}">Home</a>
        <a href="{{ url_for('index') }}">Index</a>
        <a href="{{ url_for('about') }}">About</a>
    </nav>
    <main>
        {% block content %}{% endblock %}
    </main>
</body>
</html>
```

### 5.3 CSS (`static/css/style.css`)

```css
body {
    font-family: Arial, sans-serif;
    text-align: center;
    margin-top: 50px;
}
nav a {
    margin: 0 10px;
    text-decoration: none;
    color: #0066cc;
}
nav a:hover {
    text-decoration: underline;
}
main {
    margin-top: 20px;
}
```

### 5.4 Page Templates (extending base)

**`templates/home.html`**
```html
{% extends "base.html" %}

{% block title %}Home{% endblock %}

{% block content %}
    <h1>Welcome to My Flask Website</h1>
    <p>This page uses a centered layout with CSS.</p>
{% endblock %}
```

**`templates/index.html`**
```html
{% extends "base.html" %}

{% block title %}Index{% endblock %}

{% block content %}
    <h1>Index Page</h1>
    <p>Content for the index page.</p>
{% endblock %}
```

**`templates/about.html`**
```html
{% extends "base.html" %}

{% block title %}About{% endblock %}

{% block content %}
    <h1>About This Site</h1>
    <p>Built with Flask and Jinja2.</p>
{% endblock %}
```

### 5.5 Updated `app.py`

Replace the raw‑HTML route with template‑based routes:

```python
from flask import Flask, render_template

app = Flask(__name__)

@app.route("/")
def home():
    return render_template('home.html')

@app.route("/index")
def index():
    return render_template('index.html')

@app.route("/about")
def about():
    return render_template('about.html')

if __name__ == "__main__":
    app.run(debug=True)
```

---

## 6. Summary

- **`render_template`** is the recommended method for returning HTML in Flask. It leverages Jinja2 to separate logic from presentation, provides security through auto‑escaping, and enables template inheritance.
- **Jinja2** powers the templating system, offering control structures, filters, and macros.
- **Templates** must be placed in a `templates` folder (configurable). **Static files** (CSS, JS, images) go in a `static` folder and are served via `/static`.
- Using a **base template** with `url_for('static', ...)` ensures consistent layout and easy maintenance across pages.

By adopting this architecture, developers can build maintainable, secure, and scalable multi‑page websites with Flask.