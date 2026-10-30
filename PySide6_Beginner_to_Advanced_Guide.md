# PySide6 Desktop Development: Beginner to Advanced

A practical, step-by-step learning guide for building Windows desktop
applications with **Python 3.14** and **PySide6**.

**Learning goal:** Start from zero, understand desktop interfaces, and
gradually build a professional **POS and Inventory Management System**.

## 1. What you will learn

-   Create a PySide6 project and run a desktop window.
-   Design interfaces with labels, buttons, text fields, tables,
    layouts, and menus.
-   Connect button clicks to Python functions.
-   Open a new window, switch pages, go back, and close a window.
-   Build a multi-page application with navigation.
-   Validate forms and show messages.
-   Display, add, update, and delete records in tables.
-   Store data in SQLite.
-   Organize code using reusable widgets, classes, and separate modules.
-   Build a maintainable POS and inventory application.

**Learning rule:** Complete each lesson in order. Type the code
yourself, run it, and experiment.

## 2. Tools and setup

You need Windows 10/11, Python 3.14, VS Code (recommended), and PySide6.

Check Python in PowerShell:

``` powershell
python --version
```

Create a project:

``` powershell
mkdir pyside6-learning
cd pyside6-learning
python -m venv .venv
```

Activate in PowerShell:

``` powershell
.\.venv\Scripts\Activate.ps1
```

If PowerShell blocks activation, use Command Prompt:

``` cmd
.venv\Scripts\activate.bat
```

Install PySide6:

``` powershell
python -m pip install --upgrade pip
python -m pip install PySide6
python -c "import PySide6; print(PySide6.__version__)"
```

If a version number appears, installation succeeded.

## 3. Recommended project structure

Start simply and add folders as you progress:

``` text
pyside6-learning/
├── .venv/
├── main.py
├── requirements.txt
├── app/
│   ├── __init__.py
│   ├── main_window.py
│   ├── pages/
│   │   ├── __init__.py
│   │   ├── home_page.py
│   │   ├── products_page.py
│   │   └── settings_page.py
│   ├── widgets/
│   ├── services/
│   ├── repositories/
│   └── database/
└── assets/
    ├── icons/
    └── images/
```

Save dependencies when needed:

``` powershell
python -m pip freeze > requirements.txt
```

------------------------------------------------------------------------

# Part A --- PySide6 fundamentals

## Lesson 01 --- Your first desktop window

**Goal:** Create and display a desktop window.

Create `main.py`:

``` python
import sys
from PySide6.QtWidgets import QApplication, QWidget

app = QApplication(sys.argv)

window = QWidget()
window.setWindowTitle("My First PySide6 App")
window.resize(800, 500)

window.show()

sys.exit(app.exec())
```

Run:

``` powershell
python main.py
```

### Understand the code

  Code                 Meaning
  -------------------- --------------------------------------------------
  `QApplication`       Starts and manages the GUI application
  `QWidget`            A basic window or UI component
  `setWindowTitle()`   Sets the window title
  `resize()`           Sets the initial window size
  `show()`             Makes the window visible
  `app.exec()`         Starts the event loop and waits for user actions
  `sys.exit()`         Exits the process with the application's result

**Practice:** Change the title, resize the window to 1000 × 600, run it
again, and close it.

**Completion check:** Your first window opens and closes correctly.

## Lesson 02 --- Labels, buttons, and text input

**Goal:** Put basic controls inside a window.

Replace `main.py` with:

``` python
import sys
from PySide6.QtWidgets import (
    QApplication, QWidget, QLabel, QPushButton,
    QLineEdit, QVBoxLayout
)

app = QApplication(sys.argv)

window = QWidget()
window.setWindowTitle("Basic Controls")
window.resize(400, 250)

title = QLabel("Welcome to PySide6")
name_input = QLineEdit()
name_input.setPlaceholderText("Enter your name")

button = QPushButton("Submit")
result = QLabel("Your name will appear here")

def show_name():
    name = name_input.text().strip()

    if name:
        result.setText(f"Hello, {name}!")
    else:
        result.setText("Please enter your name.")

button.clicked.connect(show_name)

layout = QVBoxLayout()
layout.addWidget(title)
layout.addWidget(name_input)
layout.addWidget(button)
layout.addWidget(result)

window.setLayout(layout)
window.show()

sys.exit(app.exec())
```

### New concepts

-   `QLabel`: displays text.
-   `QPushButton`: creates a clickable button.
-   `QLineEdit`: accepts one-line text.
-   `QVBoxLayout`: places widgets vertically.
-   `button.clicked.connect(show_name)`: calls `show_name()` when
    clicked.

**Practice:** Change the button text, add another label, test an empty
name, and connect a second button to another function.

**Completion check:** Clicking Submit displays the entered name.

## Lesson 03 --- Layouts: organize your interface

**Goal:** Arrange controls cleanly.

  Layout             Purpose
  ------------------ ------------------------------------------
  `QVBoxLayout`      Arrange widgets vertically
  `QHBoxLayout`      Arrange widgets horizontally
  `QGridLayout`      Arrange widgets in rows and columns
  `QFormLayout`      Arrange labels and form fields
  `QStackedWidget`   Show one page from a collection of pages

Example:

``` python
import sys
from PySide6.QtWidgets import (
    QApplication, QWidget, QPushButton,
    QVBoxLayout, QHBoxLayout, QLabel
)

app = QApplication(sys.argv)
window = QWidget()
window.setWindowTitle("Layout Practice")

title = QLabel("Dashboard")
button_one = QPushButton("Products")
button_two = QPushButton("Customers")

button_row = QHBoxLayout()
button_row.addWidget(button_one)
button_row.addWidget(button_two)

main_layout = QVBoxLayout()
main_layout.addWidget(title)
main_layout.addLayout(button_row)

window.setLayout(main_layout)
window.show()
sys.exit(app.exec())
```

**Practice:** Create a form with Product Name, Price, Quantity, and Save
controls. Try `QFormLayout` and `QGridLayout`.

## Lesson 04 --- Signals and slots (clickable actions)

**Goal:** Understand how controls trigger actions.

A **signal** is an event emitted by a widget. A **slot** is a function
or method that receives the event.

``` python
def save_product():
    print("Product saved")

button.clicked.connect(save_product)
```

Other useful signals:

``` python
line_edit.textChanged.connect(handle_text_change)
combo_box.currentTextChanged.connect(handle_selection)
check_box.toggled.connect(handle_checkbox)
```

**Practice:** Create Add, Update, Delete, and Clear buttons. Connect
each to a separate function. Use `print()` to verify each action.

------------------------------------------------------------------------

# Part B --- Windows and page navigation

## Lesson 05 --- Open a second window and close it

**Goal:** Open a new window by clicking a button.

Create `main.py`:

``` python
import sys
from PySide6.QtWidgets import (
    QApplication, QWidget, QPushButton, QLabel, QVBoxLayout
)


class DetailsWindow(QWidget):
    def __init__(self):
        super().__init__()

        self.setWindowTitle("Details Window")
        self.resize(400, 250)

        title = QLabel("This is the details window")
        close_button = QPushButton("Close This Window")
        close_button.clicked.connect(self.close)

        layout = QVBoxLayout()
        layout.addWidget(title)
        layout.addWidget(close_button)
        self.setLayout(layout)


class MainWindow(QWidget):
    def __init__(self):
        super().__init__()

        self.setWindowTitle("Main Window")
        self.resize(500, 300)
        self.details_window = None

        title = QLabel("Main Page")
        open_button = QPushButton("Open Details Window")
        open_button.clicked.connect(self.open_details)

        layout = QVBoxLayout()
        layout.addWidget(title)
        layout.addWidget(open_button)
        self.setLayout(layout)

    def open_details(self):
        self.details_window = DetailsWindow()
        self.details_window.show()


app = QApplication(sys.argv)
window = MainWindow()
window.show()
sys.exit(app.exec())
```

### Understand

-   `DetailsWindow` is a separate window class.
-   `self.details_window` keeps a reference to the window.
-   `show()` displays it.
-   `close()` closes that window.

**Practice:** Add a second button inside `DetailsWindow` that also
closes it. Observe what happens when you open the details window more
than once.

## Lesson 06 --- Multi-page application with QStackedWidget

**Goal:** Switch between Home, Products, and Settings inside one main
window.

For a business desktop application, this is often more convenient than
opening many separate windows.

Create `main.py`:

``` python
import sys
from PySide6.QtWidgets import (
    QApplication, QWidget, QLabel, QPushButton,
    QVBoxLayout, QHBoxLayout, QStackedWidget
)


class HomePage(QWidget):
    def __init__(self):
        super().__init__()
        layout = QVBoxLayout()
        layout.addWidget(QLabel("Home Page"))
        layout.addWidget(QLabel("Welcome to the dashboard."))
        layout.addStretch()
        self.setLayout(layout)


class ProductsPage(QWidget):
    def __init__(self):
        super().__init__()
        layout = QVBoxLayout()
        layout.addWidget(QLabel("Products Page"))
        layout.addWidget(QLabel("Manage your products here."))
        layout.addStretch()
        self.setLayout(layout)


class SettingsPage(QWidget):
    def __init__(self):
        super().__init__()
        layout = QVBoxLayout()
        layout.addWidget(QLabel("Settings Page"))
        layout.addWidget(QLabel("Change application settings here."))
        layout.addStretch()
        self.setLayout(layout)


class MainWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("My Business Application")
        self.resize(900, 600)

        home_button = QPushButton("Home")
        products_button = QPushButton("Products")
        settings_button = QPushButton("Settings")

        navigation = QHBoxLayout()
        navigation.addWidget(home_button)
        navigation.addWidget(products_button)
        navigation.addWidget(settings_button)

        self.pages = QStackedWidget()
        self.pages.addWidget(HomePage())       # index 0
        self.pages.addWidget(ProductsPage())    # index 1
        self.pages.addWidget(SettingsPage())    # index 2

        home_button.clicked.connect(
            lambda: self.pages.setCurrentIndex(0)
        )
        products_button.clicked.connect(
            lambda: self.pages.setCurrentIndex(1)
        )
        settings_button.clicked.connect(
            lambda: self.pages.setCurrentIndex(2)
        )

        main_layout = QVBoxLayout()
        main_layout.addLayout(navigation)
        main_layout.addWidget(self.pages)
        self.setLayout(main_layout)


app = QApplication(sys.argv)
window = MainWindow()
window.show()
sys.exit(app.exec())
```

### Important concepts

-   `QStackedWidget` holds multiple page widgets.
-   `addWidget()` adds a page.
-   `setCurrentIndex(0)` displays the first page.
-   `setCurrentIndex(1)` displays the second page.
-   `lambda` lets you pass an argument when the button is clicked.

**Practice:** Add Customers and Reports pages, create buttons for them,
and make Home the first page shown.

**Completion check:** Buttons switch pages without opening separate
windows.

## Lesson 07 --- Back button and page history

**Goal:** Return to the previously visited page.

A simple Home button always returns to Home:

``` python
home_button.clicked.connect(
    lambda: self.pages.setCurrentIndex(0)
)
```

For actual Back navigation, maintain a history list. Add this inside
`MainWindow`.

In `__init__`, after creating `self.pages`:

``` python
self.page_history = []
```

Add these methods to the class:

``` python
def navigate_to(self, index):
    current_index = self.pages.currentIndex()

    if current_index != index:
        self.page_history.append(current_index)
        self.pages.setCurrentIndex(index)

def go_back(self):
    if self.page_history:
        previous_index = self.page_history.pop()
        self.pages.setCurrentIndex(previous_index)
```

Create and connect a Back button:

``` python
back_button = QPushButton("Back")
back_button.clicked.connect(self.go_back)
```

Connect page buttons using the history-aware method:

``` python
products_button.clicked.connect(
    lambda: self.navigate_to(1)
)
settings_button.clicked.connect(
    lambda: self.navigate_to(2)
)
```

**Practice:** Navigate Home → Products → Settings. Click Back and
confirm that you return to Products, then Home.

**Note:** This is a basic history example. Later, learn to handle
repeated pages, disabled Back buttons, and page-specific state.

## Lesson 08 --- Sidebar and professional dashboard

**Goal:** Build a business-app layout.

``` text
+------------------------------------------------------+
| Application Name                         User Menu   |
+------------------+-----------------------------------+
| Dashboard        |                                   |
| Products         |           Current Page            |
| Categories       |                                   |
| Customers        |                                   |
| Suppliers        |                                   |
| Purchases        |                                   |
| Sales            |                                   |
| Reports          |                                   |
| Settings         |                                   |
+------------------+-----------------------------------+
```

Study:

-   `QMainWindow`
-   `QWidget`
-   `QFrame`
-   `QStackedWidget`
-   `QToolBar`
-   `QMenuBar`
-   `QStatusBar`
-   `QSplitter`

**Practice:** Replace horizontal navigation buttons with a left sidebar.
Keep page content inside `QStackedWidget`.

------------------------------------------------------------------------

# Part C --- Forms, tables, and user experience

## Lesson 09 --- Form validation and message boxes

**Goal:** Prevent invalid input and show feedback.

Study `QMessageBox`, `QIntValidator`, `QDoubleValidator`, `QComboBox`,
and `QDateEdit`.

Example:

``` python
from PySide6.QtWidgets import QMessageBox

def save_product():
    name = name_input.text().strip()

    if not name:
        QMessageBox.warning(
            window,
            "Validation Error",
            "Product name is required."
        )
        return

    QMessageBox.information(
        window,
        "Success",
        f"{name} saved successfully."
    )
```

**Practice:** Require a product name and valid price before saving.

## Lesson 10 --- Tables and product lists

**Goal:** Display product records in a table.

Study `QTableWidget`, `QTableWidgetItem`, `QHeaderView`, sorting, and
row selection.

Example:

``` python
import sys
from PySide6.QtWidgets import (
    QApplication, QWidget, QTableWidget,
    QTableWidgetItem, QVBoxLayout
)

app = QApplication(sys.argv)
window = QWidget()
window.setWindowTitle("Product Table")
window.resize(600, 400)

table = QTableWidget()
table.setColumnCount(3)
table.setHorizontalHeaderLabels(
    ["Product Name", "Price", "Quantity"]
)

products = [
    ("Keyboard", "1200", "10"),
    ("Mouse", "600", "20"),
    ("Monitor", "15000", "5"),
]

table.setRowCount(len(products))

for row, product in enumerate(products):
    for column, value in enumerate(product):
        table.setItem(row, column, QTableWidgetItem(value))

layout = QVBoxLayout()
layout.addWidget(table)
window.setLayout(layout)
window.show()
sys.exit(app.exec())
```

**Practice:** Add a search box and filter the displayed products.

## Lesson 11 --- Add, edit, delete, and clear

**Goal:** Build a complete product form.

Create Product Name, Price, and Quantity inputs, plus Save, Update,
Delete, and Clear buttons and a product table.

Recommended order:

1.  Add a row to the table.
2.  Select a table row and load its values into the form.
3.  Update the selected row.
4.  Delete the selected row after confirmation.
5.  Clear the form.
6.  Validate all fields.

Start with in-memory Python data. Connect a database only after these
actions work.

## Lesson 12 --- Styling with Qt Style Sheets (QSS)

**Goal:** Make the application look professional.

Qt Style Sheets are similar to CSS, but not exactly the same.

``` python
window.setStyleSheet("""
    QWidget {
        font-family: Arial;
        font-size: 14px;
    }

    QPushButton {
        background-color: #2563eb;
        color: white;
        border: none;
        padding: 10px 16px;
        border-radius: 6px;
    }

    QPushButton:hover {
        background-color: #1d4ed8;
    }

    QLineEdit {
        padding: 8px;
        border: 1px solid #cbd5e1;
        border-radius: 5px;
    }
""")
```

Study colors, typography, padding, margins, borders,
hover/pressed/disabled states, object names, and reusable QSS files.

**Practice:** Create a light theme and a dark theme.

------------------------------------------------------------------------

# Part D --- Data and application architecture

## Lesson 13 --- SQLite database basics

**Goal:** Save products so they remain available after the app closes.

Learn to create a database and tables, insert/read/update/delete
records, use parameterized SQL, and handle errors.

Example table:

``` sql
CREATE TABLE IF NOT EXISTS products (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT NOT NULL,
    price REAL NOT NULL,
    quantity INTEGER NOT NULL DEFAULT 0
);
```

Example connection:

``` python
import sqlite3

connection = sqlite3.connect("app.db")

connection.execute("""
    CREATE TABLE IF NOT EXISTS products (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        name TEXT NOT NULL,
        price REAL NOT NULL,
        quantity INTEGER NOT NULL DEFAULT 0
    )
""")

connection.commit()
connection.close()
```

**Important:** Use parameterized queries for user input. Never build SQL
by joining raw user-entered strings.

## Lesson 14 --- Separate UI, business logic, and database code

**Goal:** Avoid putting the entire application in one Python file.

Suggested structure:

``` text
app/
├── main_window.py
├── pages/
│   └── products_page.py
├── services/
│   └── product_service.py
├── repositories/
│   └── product_repository.py
└── database/
    └── connection.py
```

  Layer        Responsibility
  ------------ ------------------------------------------
  UI / Page    Shows controls and receives user actions
  Service      Applies business rules
  Repository   Reads and writes database records
  Database     Manages connections and schema

Flow:

``` text
User clicks Save
       |
       v
ProductsPage
       |
       v
ProductService
       |
       v
ProductRepository
       |
       v
SQLite Database
```

Later, learn dependency injection and automated tests.

## Lesson 15 --- Qt Designer and reusable widgets

**Goal:** Learn visual interface design and component reuse.

Study Qt Designer, `.ui` files, loading UI files, custom widgets,
reusable form components, dialog windows, resource files, and icons.

You can design a form visually in Qt Designer or create the interface
directly in Python. Learn both approaches.

## Lesson 16 --- Background tasks and responsive applications

**Goal:** Keep the UI responsive when a task takes time.

Study the Qt event loop, `QThread`, `QObject`, signals between threads,
progress bars, cancellation, and error reporting.

Use background tasks for large file imports, report generation, or slow
network requests.

**Important:** Do not update Qt widgets directly from a worker thread.
Send results to the GUI thread using signals.

## Lesson 17 --- Advanced desktop application features

After you can build a working CRUD application, study:

-   Application settings with `QSettings`
-   Logging and error handling
-   Keyboard shortcuts and actions
-   Menus and toolbars
-   File dialogs
-   CSV and Excel import/export
-   PDF report generation
-   Printing
-   User login and role-based permissions
-   Automated tests

## Lesson 18 --- Package a Windows application

**Goal:** Package your application so another Windows user can run it.

Learn PyInstaller, application icons, data/resource files, SQLite
database location, configuration files, and clean installation testing.

Starting command:

``` powershell
python -m pip install pyinstaller
pyinstaller --windowed --name MyDesktopApp main.py
```

This is only a starting point. A real application may need additional
data files, icons, hidden imports, and configuration. Test the packaged
app on a clean Windows environment.

------------------------------------------------------------------------

# Part E --- Final project: POS and Inventory Management System

Build this project after completing the fundamentals.

## Suggested modules

  Module        Main features
  ------------- --------------------------------------
  Dashboard     Sales summary, stock summary
  Products      Add, edit, delete, search
  Categories    Product categories
  Customers     Customer records
  Suppliers     Supplier records
  Purchases     Stock purchasing
  Sales / POS   Cart, checkout, invoice
  Inventory     Stock in, stock out, adjustments
  Reports       Daily and monthly reports
  Users         Login, roles, permissions
  Settings      Business information and preferences

## Suggested project structure

``` text
pos_inventory/
├── main.py
├── requirements.txt
├── app/
│   ├── main_window.py
│   ├── pages/
│   │   ├── dashboard_page.py
│   │   ├── products_page.py
│   │   ├── customers_page.py
│   │   ├── suppliers_page.py
│   │   ├── purchases_page.py
│   │   ├── sales_page.py
│   │   └── reports_page.py
│   ├── widgets/
│   ├── services/
│   ├── repositories/
│   ├── database/
│   ├── models/
│   └── resources/
├── assets/
│   ├── icons/
│   └── styles/
└── tests/
```

## Development milestones

1.  Build the main window and sidebar.
2.  Add page navigation and Back navigation.
3.  Create the Products page UI.
4.  Implement product form validation.
5.  Display products in a table.
6.  Implement add, edit, delete, and search.
7.  Connect the product module to SQLite.
8.  Build categories and suppliers.
9.  Implement purchase and stock updates.
10. Build the POS cart and checkout.
11. Generate invoices and reports.
12. Add login and permissions.
13. Test and package the application.

**Do not build all modules at once.** Complete and test one module
before starting the next.

------------------------------------------------------------------------

# Daily learning routine

A practical 60-minute session:

  Time         Activity
  ------------ ---------------------------------------
  10 minutes   Review the previous lesson
  20 minutes   Learn the new concept
  20 minutes   Type and run the code
  10 minutes   Change the code and solve a challenge

For every lesson:

1.  Read the goal.
2.  Create the required file.
3.  Type the code yourself.
4.  Run it in the terminal.
5.  Compare the result with the completion check.
6.  Try the practice tasks.
7.  Write down any error and how you fixed it.

------------------------------------------------------------------------

# Progress tracker

Mark a lesson complete only after you have run the code and completed
its practice task.

-   [ ] Lesson 01 --- First window
-   [ ] Lesson 02 --- Labels, buttons, and text input
-   [ ] Lesson 03 --- Layouts
-   [ ] Lesson 04 --- Signals and slots
-   [ ] Lesson 05 --- Open and close another window
-   [ ] Lesson 06 --- Multi-page navigation with QStackedWidget
-   [ ] Lesson 07 --- Back button and page history
-   [ ] Lesson 08 --- Sidebar dashboard
-   [ ] Lesson 09 --- Form validation and message boxes
-   [ ] Lesson 10 --- Tables and product lists
-   [ ] Lesson 11 --- Add, edit, delete, and clear
-   [ ] Lesson 12 --- Qt Style Sheets
-   [ ] Lesson 13 --- SQLite
-   [ ] Lesson 14 --- Application architecture
-   [ ] Lesson 15 --- Qt Designer and reusable widgets
-   [ ] Lesson 16 --- Background tasks
-   [ ] Lesson 17 --- Advanced features
-   [ ] Lesson 18 --- Packaging
-   [ ] Final Project --- POS and Inventory System

# How to ask for the next lesson

After completing a lesson, send:

> My PySide6 Lesson 01 status: success. Please teach me Lesson 02 step
> by step in Bangla. Explain every new line, show the expected output,
> give me practice tasks, and wait for my result before moving to the
> next lesson.

If you get an error, send:

> My PySide6 lesson number is \_\_. Here is my complete error and code.
> Explain the reason in Bangla and help me fix it without skipping the
> concept.

## Learning rule

**One lesson at a time.** Do not jump to databases, advanced
architecture, or packaging before you understand widgets, layouts,
signals, and page navigation.

Your first target is to complete Lessons 01--07 and build a small
application where you can open pages, move between pages, go back, and
close windows.
