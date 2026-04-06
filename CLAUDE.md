# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Spendly** is a Flask-based expense tracking web application. It's structured as an educational project with progressive implementation steps, where the core application framework and UI are in place, but database and user-facing features are scaffolded for implementation.

### Architecture

- **Backend**: Flask web framework (Python 3)
- **Frontend**: Jinja2 templates with custom CSS, vanilla JavaScript
- **Styling**: Custom CSS organized in `static/css/`
- **Database**: SQLite (to be implemented in `database/db.py`)
- **Testing**: pytest with pytest-flask

### Directory Structure

```
app.py                 # Flask application root, defines all routes
database/
  db.py               # Database initialization (to be implemented)
  __init__.py
templates/
  base.html           # Base template with navbar and footer
  landing.html        # Landing/home page
  login.html          # Login form
  register.html       # Registration form
  terms.html          # Terms and conditions
  privacy.html        # Privacy policy
static/
  css/
    style.css         # Main stylesheet
    landing.css       # Landing page-specific styles
  js/
    main.js           # Client-side JavaScript
requirements.txt      # Python dependencies (Flask 3.1.3, pytest, etc.)
```

## Development Commands

### Setup & Installation
```bash
# Create virtual environment (if not already done)
python -m venv myclaudeenv

# Activate virtual environment
# On Windows:
myclaudeenv\Scripts\activate
# On macOS/Linux:
source myclaudeenv/bin/activate

# Install dependencies
pip install -r requirements.txt
```

### Running the Application
```bash
# Run Flask development server (debug mode on, port 5001)
python app.py
# App will be available at http://localhost:5001
```

### Testing
```bash
# Run all tests
pytest

# Run a single test file
pytest tests/test_auth.py

# Run tests with verbose output
pytest -v

# Run with coverage
pytest --cov
```

## Key Implementation Notes

### Routes & Workflow

**Implemented routes:**
- `GET /` — Landing page
- `GET /login` — Login form
- `GET /register` — Registration form
- `GET /terms` — Terms and conditions
- `GET /privacy` — Privacy policy

**Placeholder routes** (marked for future implementation with step numbers):
- `/logout` — Step 3
- `/profile` — Step 4
- `/expenses/add` (POST) — Step 7
- `/expenses/<id>/edit` — Step 8
- `/expenses/<id>/delete` — Step 9

### Templates

All templates extend `base.html`, which provides:
- Responsive navbar with branding (Spendly)
- Navigation links (Sign in, Get started)
- Footer with links to Terms/Privacy
- Asset loading (CSS, JS)

When adding new templates:
1. Use `{% extends "base.html" %}`
2. Override `{% block title %}`, `{% block content %}`, or `{% block head %}` as needed
3. Keep template logic minimal — complex logic belongs in Flask route handlers

### Database Module

The `database/db.py` module needs to implement:
- `get_db()` — Returns SQLite connection with row_factory and foreign keys enabled
- `init_db()` — Creates all required tables (IF NOT EXISTS)
- `seed_db()` — Populates sample data for development

This module is imported and used by route handlers for data operations.

### Static Assets

- **CSS files**: Located in `static/css/`. Each page can have its own stylesheet (e.g., `landing.css`) that's loaded in the template's `{% block head %}`.
- **JavaScript**: Main script at `static/js/main.js`. Add event listeners and client-side logic here.

## Testing Strategy

- Use `pytest` with `pytest-flask` fixtures for application context
- Test fixtures should provide a test client via Flask's test client
- Keep tests close to the features they test (group by module/route)

## Common Development Tasks

### Adding a New Route
1. Add the route handler in `app.py` with appropriate HTTP method(s)
2. Create corresponding template in `templates/` (extend `base.html`)
3. If the route needs data, implement the query in `database/db.py` and call it from the route
4. Add tests in the appropriate test file

### Styling a Page
1. Add page-specific styles in `static/css/{page_name}.css`
2. Link it in the template's `{% block head %}` section
3. Use semantic class names; avoid inline styles

### Database Migrations
When the database schema changes, update `database/db.py`:
1. Modify the `init_db()` function's CREATE TABLE statements
2. Re-run `seed_db()` if sample data structure changes
3. Clear the database file and re-initialize for development/testing

## Important Context

This project uses Indian Rupee (₹) currency and targets an Indian audience based on template content ("Track every rupee. Own your finances.").
