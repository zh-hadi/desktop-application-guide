# Python Desktop Application Development --- Starter Guide

A beginner-friendly, repeatable setup guide for creating Python desktop
applications with **PySide6** on Windows 10.

This guide uses Python 3.14 and builds toward a **POS & Inventory
Management System**. You can reuse the setup and project structure for
future desktop applications.

------------------------------------------------------------------------

## 1. Project goal

Build a desktop application in small, testable steps.

Planned POS & Inventory modules:

-   Login and user roles
-   Dashboard
-   Product and category management
-   Inventory and stock tracking
-   Suppliers and customers
-   Purchases and sales
-   POS checkout
-   Invoice and receipt printing
-   Reports

Do not build all modules at once. First make sure the environment and
basic window work.

## 2. Technology stack

  Technology    Purpose
  ------------- -----------------------------------------
  Python 3.14   Main programming language
  PySide6       Desktop GUI framework (Qt for Python)
  SQLite        Local database for the beginner version
  VS Code       Code editor
  Git           Version control (add when ready)

> Python 3.14 is already installed on the target computer. If a package
> installation fails, read the error carefully and check the package's
> current Python-version support before changing Python versions.

## 3. Install/check prerequisites

Open **Command Prompt** or the VS Code terminal.

Check Python:

``` bat
python --version
```

Expected: `Python 3.14.x`

Check pip:

``` bat
python -m pip --version
```

If `python` is not recognized, try:

``` bat
py --version
```

If neither command works, repair the Python installation and enable
**Add Python to PATH** in the installer.

## 4. Create a new project

Choose a folder for your projects. Example:

``` text
D:\python-desktop\
```

For a new application, create a separate folder:

``` bat
mkdir D:\python-desktop\my_desktop_app
cd /d D:\python-desktop\my_desktop_app
```

Open this folder in VS Code:

``` bat
code .
```

If the `code` command is unavailable, open VS Code manually and use
**File → Open Folder**.

## 5. Create a virtual environment

A virtual environment keeps this project's installed packages separate
from other projects.

``` bat
python -m venv .venv
```

Activate it in **Command Prompt**:

``` bat
.venv\Scripts\activate.bat
```

Activate it in **PowerShell**:

``` powershell
.\.venv\Scripts\Activate.ps1
```

If PowerShell blocks activation, use Command Prompt instead. You do not
need to change the computer's execution policy just for this beginner
project.

When activated, the terminal prompt normally starts with `(.venv)`.

Upgrade pip:

``` bat
python -m pip install --upgrade pip
```

## 6. Install PySide6

With the virtual environment activated:

``` bat
python -m pip install pyside6
```

Verify the installation:

``` bat
python -c "import PySide6; print(PySide6.__version__)"
```

Verify that Qt Widgets can be imported:

``` bat
python -c "from PySide6.QtWidgets import QApplication, QWidget; print('PySide6 is ready!')"
```

Expected:

``` text
PySide6 is ready!
```

## 7. Create the first application

Create a file named `main.py` in the project folder.

``` python
import sys

from PySide6.QtWidgets import (
    QApplication,
    QWidget,
    QLabel,
    QPushButton,
    QVBoxLayout,
)


class MainWindow(QWidget):
    def __init__(self):
        super().__init__()

        self.setWindowTitle("My First PySide6 App")
        self.resize(500, 300)

        self.message_label = QLabel("Welcome to Python desktop development!")
        self.click_button = QPushButton("Click me")

        self.click_button.clicked.connect(self.handle_button_click)

        layout = QVBoxLayout()
        layout.addWidget(self.message_label)
        layout.addWidget(self.click_button)

        self.setLayout(layout)

    def handle_button_click(self):
        self.message_label.setText("Button clicked successfully!")


def main():
    app = QApplication(sys.argv)

    window = MainWindow()
    window.show()

    sys.exit(app.exec())


if __name__ == "__main__":
    main()
```

Run it:

``` bat
python main.py
```

A window should open. Click the button and confirm that the label
changes.

## 8. Understand the first application

  Code                     Meaning
  ------------------------ -----------------------------------------
  `QApplication`           Creates and manages the GUI application
  `QWidget`                Basic window/widget class
  `QLabel`                 Displays text
  `QPushButton`            Creates a clickable button
  `QVBoxLayout`            Arranges widgets vertically
  `clicked.connect(...)`   Connects a button click to a function
  `show()`                 Displays the window
  `app.exec()`             Starts Qt's event loop
  `__init__()`             Initializes a new object
  `self`                   Refers to the current object

## 9. Recommended starter folder structure

Start small:

``` text
my_desktop_app/
│
├── .venv/
├── main.py
├── README.md
└── requirements.txt
```

As the application grows, move to a structure like this:

``` text
my_desktop_app/
│
├── .venv/
├── main.py
├── requirements.txt
├── README.md
│
├── app/
│   ├── __init__.py
│   ├── views/
│   │   ├── __init__.py
│   │   └── main_window.py
│   ├── models/
│   │   └── __init__.py
│   ├── services/
│   │   └── __init__.py
│   └── database/
│       └── __init__.py
│
├── assets/
│   ├── icons/
│   └── images/
└── tests/
```

Keep responsibilities separate:

-   **views**: windows, screens, forms and widgets.
-   **models**: application data and domain objects.
-   **services**: business rules and application operations.
-   **database**: database connection and persistence code.
-   **assets**: icons and images.
-   **tests**: automated tests.

Do not create empty layers just to follow a pattern. Add folders when
the project needs them.

## 10. Save dependencies

After installing the packages needed by the project:

``` bat
python -m pip freeze > requirements.txt
```

For a future setup, create and activate a new virtual environment, then
install:

``` bat
python -m pip install -r requirements.txt
```

The `.venv` folder itself should not be copied between computers or
committed to Git. Re-create it using the commands in this guide.

## 11. Daily development workflow

Whenever you return to this project:

1.  Open the project folder in VS Code.

2.  Open a terminal in that folder.

3.  Activate the virtual environment:

    ``` bat
    .venv\Scripts\activate.bat
    ```

4.  Run the application:

    ``` bat
    python main.py
    ```

5.  Make one small change at a time.

6.  Run again and test the change.

To leave the virtual environment:

``` bat
deactivate
```

## 12. Troubleshooting

### `python` is not recognized

Try:

``` bat
py --version
```

If the Python launcher works, use `py` in place of `python`, or repair
PATH through the Python installer.

### `No module named PySide6`

The virtual environment may not be active, or PySide6 may not be
installed in it.

``` bat
.venv\Scripts\activate.bat
python -m pip install pyside6
```

### PowerShell says script execution is disabled

Use Command Prompt and activate with:

``` bat
.venv\Scripts\activate.bat
```

### The window opens and closes immediately

Run the program from the VS Code terminal:

``` bat
python main.py
```

Read the traceback shown in the terminal. Fix the first relevant error
rather than guessing.

### Package installation fails on Python 3.14

Copy the complete pip error. Check whether the current PySide6 release
supports your Python version and Windows architecture. Do not randomly
install multiple Python versions or packages.

## 13. Beginner learning roadmap

Follow this order:

1.  Python basics: variables, strings, numbers, conditions, loops and
    functions.
2.  OOP: classes, objects, constructors, methods and inheritance.
3.  PySide6 basics: `QApplication`, `QWidget`, `QMainWindow`, labels,
    buttons and inputs.
4.  Layouts: vertical, horizontal and grid layouts.
5.  Signals and slots: button clicks and other events.
6.  Forms and tables: validation, `QComboBox`, `QTableWidget` and
    model/view concepts.
7.  SQLite: connect, create tables, insert, read, update and delete.
8.  Application structure: separate views, models and services.
9.  POS features: products, stock, cart, checkout, customers and
    invoices.
10. Testing and packaging: test important workflows and package the app
    for Windows.

## 14. First-project completion checklist

-   [ ] Python version checked.
-   [ ] Project folder created.
-   [ ] Virtual environment created.
-   [ ] Virtual environment activated.
-   [ ] pip upgraded.
-   [ ] PySide6 installed.
-   [ ] PySide6 import test passed.
-   [ ] `main.py` created.
-   [ ] Desktop window launched.
-   [ ] Button click changed the label.
-   [ ] `requirements.txt` generated.

## 15. Reusable commands --- quick reference

Run these commands in Command Prompt for each new project (change the
project name):

``` bat
mkdir D:\python-desktop\my_new_app
cd /d D:\python-desktop\my_new_app
python -m venv .venv
.venv\Scripts\activate.bat
python -m pip install --upgrade pip
python -m pip install pyside6
```

Create `main.py`, paste the first-application example from Section 7,
then run:

``` bat
python main.py
```

------------------------------------------------------------------------

**Learning rule:** Understand each small example, type it yourself, run
it, and change one thing to see what happens. Build the POS & Inventory
system one working feature at a time.
