# Flask Project

A small Flask web app plus a set of static HTML pages for a restaurant site ("Pasta Bella").

## Project Structure

```
flask/
├── app.py               # Flask application (birthday lookup)
├── billionaires.xlsx    # Data file used by app.py (you must add this)
├── requirements.txt     # Python dependencies
├── README.md
├── static/
│   └── style.css        # Stylesheet
└── templates/
    ├── index.html       # Home page
    ├── menu.html        # Menu page
    ├── about.html       # About page
    └── contact.html     # Contact page
```

## What It Does

`app.py` looks up billionaires whose birthday falls on a given day.

| Route        | Behavior                                              |
|--------------|-------------------------------------------------------|
| `/`          | Shows billionaires whose birthday is **today**        |
| `/<date>`    | Shows billionaires whose birthday matches that date   |

Only the month and day are compared, not the year. If nobody matches, it shows `No Birthday Available`.

Example: `http://127.0.0.1:5000/2026-03-15` lists everyone born on March 15.

### Data file format

`billionaires.xlsx` must be in the same folder as `app.py`, with these columns in A to D:

| A    | B             | C       | D                  |
|------|---------------|---------|--------------------|
| Name | Date of birth | Country | Net worth (in $ B) |

## Requirements

- Python 3.9 or newer
- Packages listed in `requirements.txt` (Flask, pandas, openpyxl)

## Setup and Run

```bash
# 1. (Optional) create a virtual environment
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

# 2. Install dependencies
pip install -r requirements.txt

# 3. Run the app
python app.py
```

Then open http://127.0.0.1:5000 in your browser.

## Notes

- The HTML files in `templates/` (Pasta Bella pages) are **not yet connected** to `app.py`. To serve them, add routes such as:

  ```python
  from flask import render_template

  @app.route("/menu")
  def menu():
      return render_template("menu.html")
  ```

  Add similar routes for `/about` and `/contact`. Note the dynamic `/<d>` route would otherwise capture those paths, so define the specific routes before it.
- `app.run(debug=True)` runs at module level. For production, wrap it in `if __name__ == "__main__":` and use a proper WSGI server (e.g. gunicorn) with `debug` turned off.
