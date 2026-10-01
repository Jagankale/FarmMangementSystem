# 🌾 Farm Management System

A web-based **Farm Management System** developed using **Python Flask, Flask-SQLAlchemy, SQLite, HTML, CSS, Bootstrap and JavaScript**.

The application helps farmers digitally manage their crop information, farming expenses and agricultural income. It also provides a dashboard for financial analysis, crop-wise profitability reports and PDF report generation.

---

## 📌 Project Overview

Managing crop records, expenses and income manually can be difficult and time-consuming. This project provides a centralized web application where a farmer can maintain farming-related records digitally.

The system allows a registered user to:

* Create and manage an account
* Add and manage crop information
* Record crop-related and general expenses
* Record agricultural income
* Calculate total income and expenses
* Calculate overall profit or loss
* View crop-wise financial information
* View recent farming activities
* Generate profitability reports
* Export farm reports as PDF

Each user's farming information is associated with their user account, so the application displays data based on the currently logged-in user.

---

## 🎯 Objectives

The main objectives of the project are:

1. Digitize basic farm record management.
2. Store crop, income and expense information in a database.
3. Reduce dependency on manual record keeping.
4. Provide automatic income and expense calculations.
5. Calculate overall and crop-wise profit or loss.
6. Provide an easy-to-use web interface.
7. Generate downloadable farm management reports.

---

## 🛠️ Technologies Used

### Frontend

* HTML5
* CSS3
* Bootstrap 5
* JavaScript
* Jinja2 Templates

### Backend

* Python
* Flask

### Database

* SQLite
* Flask-SQLAlchemy
* SQLAlchemy ORM

### Security

* Werkzeug password hashing
* Flask session-based authentication

### Reporting

* HTML reports
* Reports API
* XHTML2PDF for PDF generation

---

## 🏗️ Application Architecture

The application follows a basic client-server architecture:

```text
             USER
               |
               ↓
      HTML / CSS / Bootstrap
        + JavaScript
               |
               ↓
        HTTP Request
               |
               ↓
          Flask App
               |
        Business Logic
               |
               ↓
      Flask-SQLAlchemy
               |
               ↓
          SQLite DB
               |
               ↓
        Query / Result
               |
               ↓
        Flask Response
               |
               ↓
          Web Page
```

---

## 📂 Project Structure

```text
FMS-main/
│
├── app.py
├── farm_db.db
├── README.md
│
├── static/
│   ├── css/
│   │   ├── auth.css
│   │   └── style.css
│   │
│   ├── js/
│   │   ├── login.js
│   │   └── register.js
│   │
│   └── images/
│       └── farm-bg.png
│
└── templates/
    ├── base.html
    ├── home.html
    ├── login.html
    ├── register.html
    ├── crops.html
    ├── crop_dashboard.html
    ├── expenses.html
    ├── income.html
    ├── reports.html
    └── reports_pdf.html
```

---

# 🔐 Authentication Module

The system provides user registration and login functionality.

### Registration

A new user can register using:

* Name
* Email
* Password

The password is not stored as plain text. It is processed using Werkzeug's password hashing functionality.

Conceptually:

```text
User enters password
        ↓
generate_password_hash()
        ↓
Hashed password stored in database
```

### Login

During login:

```text
Email + Password
       ↓
Find user by email
       ↓
check_password_hash()
       ↓
Valid?
  ↓       ↓
 Yes      No
  ↓       ↓
Session   Error
created
```

The logged-in user's ID is stored in the Flask session.

```python
session["user"] = user.id
```

---

# 👨‍🌾 User-Specific Data

The application associates farming records with the logged-in user.

For example, the `Crop`, `Expense` and `Income` models contain:

```python
user_id = db.Column(
    db.Integer,
    db.ForeignKey("user.id"),
    nullable=False
)
```

This allows the application to filter data using the current user's ID.

For example:

```python
Crop.query.filter_by(user_id=session["user"]).all()
```

Therefore, the application retrieves crops belonging to the current user instead of displaying every user's records.

---

# 🌱 Crop Management

The Crop Management module allows the farmer to add and manage crop information.

The crop model contains:

| Field          | Purpose              |
| -------------- | -------------------- |
| `id`           | Primary key          |
| `crop_name`    | Name of crop         |
| `area`         | Cultivated area      |
| `season`       | Crop season          |
| `planted_date` | Planting date        |
| `created_at`   | Record creation time |
| `user_id`      | Owner of the crop    |

### Crop Operations

The system supports:

* Add crop
* View crops
* Edit crop
* Delete crop

This represents the basic **CRUD operations**.

```text
Create → Add Crop
Read   → View Crop
Update → Edit Crop
Delete → Delete Crop
```

---

# 💰 Expense Management

The Expense module allows users to record farming expenses.

Expenses can be associated with a specific crop or treated as general farm expenses.

The expense model contains:

| Field          | Purpose                 |
| -------------- | ----------------------- |
| `id`           | Primary key             |
| `expense_type` | General or crop-related |
| `category`     | Expense category        |
| `description`  | Expense details         |
| `amount`       | Expense amount          |
| `date`         | Expense date            |
| `crop_id`      | Associated crop         |
| `created_at`   | Creation time           |
| `user_id`      | Owner of record         |

Example expenses:

```text
Seeds
Fertilizer
Labour
Pesticides
Equipment
Other farm expenses
```

---

# 💵 Income Management

The Income module records money earned from agricultural activities.

The user can select a crop, enter quantity and price per unit.

The application automatically calculates:

```text
Total Income = Quantity × Price per Unit
```

For example:

```text
Quantity       = 100 quintals
Price per unit = ₹2,000

Total Income = 100 × 2,000
             = ₹2,00,000
```

The calculated amount is stored in the `total_amount` field.

---

# 📊 Dashboard

The home dashboard provides a summary of the user's farming activities.

The dashboard calculates:

* Total number of crops
* Total income
* Total expenses
* Overall profit
* Recent expenses
* Recent income

The profit calculation is:

```text
Profit = Total Income - Total Expense
```

The application also limits recent activity to the latest five income and expense records.

---

# 📈 Crop Dashboard

The Crop Dashboard provides crop-level financial information.

For every crop, the application calculates:

```text
Crop Income
Crop Expense
```

The backend prepares data in the following logical structure:

```text
Crop
 ├── Income
 └── Expense
```

This data is passed to the frontend for dashboard visualization.

The application also calculates overall:

```text
Total Income
Total Expense
Profit
```

---

# 📋 Reports

The Reports module provides a crop-wise profitability summary.

For each crop, the system calculates:

```text
Total Expense
Total Income
Net Profit / Loss
```

The calculation is:

```text
Net Profit = Total Income - Total Expense
```

If the result is positive, it represents profit.

If the result is negative, it represents a loss.

The report also provides overall totals.

---

# 📄 PDF Report Generation

The project supports exporting farm reports as PDF.

The process is:

```text
User requests PDF
       ↓
Flask route
       ↓
Retrieve user's crop/income/expense data
       ↓
Calculate report information
       ↓
Render reports_pdf.html
       ↓
Convert HTML → PDF
       ↓
Return PDF response
       ↓
Download farm_report.pdf
```

The project uses **XHTML2PDF** for PDF generation.

The generated report contains:

* Farmer name
* Email
* Report generation date
* Crop
* Expense
* Income
* Profit/Loss
* Overall totals

---

# 🔌 Reports API

The application also provides a reports endpoint:

```text
/api/reports
```

This endpoint returns report information in a structured format suitable for programmatic access.

The API prepares crop-wise:

```text
Crop
Expense
Income
Profit
```

This demonstrates the project's ability to expose backend data through an API endpoint in addition to rendering HTML pages.

---

# 🗄️ Database Design

The application uses **SQLite** as its database.

The main tables are:

```text
User
Crop
Expense
Income
```

### User

```text
User
-----
id (PK)
name
email (UNIQUE)
password
```

### Crop

```text
Crop
-----
id (PK)
crop_name
area
season
planted_date
created_at
user_id (FK)
```

### Expense

```text
Expense
-------
id (PK)
expense_type
category
description
amount
date
crop_id (FK)
created_at
user_id (FK)
```

### Income

```text
Income
------
id (PK)
crop_id (FK)
quantity
price_per_unit
total_amount
details
date
created_at
user_id (FK)
```

---

# 🔗 Database Relationships

The database uses foreign keys to establish relationships.

```text
User
 |
 | 1
 |
 |------< Crop
 |
 |------< Expense
 |
 |------< Income
```

A crop can have multiple expenses:

```text
Crop 1 -------- * Expense
```

A crop can also have multiple income records:

```text
Crop 1 -------- * Income
```

The `crop_id` field establishes these relationships.

The `user_id` field associates records with their respective user.

---

# 🔄 Complete Application Flow

A typical user flow is:

```text
Register
   ↓
Login
   ↓
Home Dashboard
   ↓
Add Crop
   ↓
Record Expenses
   ↓
Record Income
   ↓
Calculate Income & Expenses
   ↓
Calculate Profit/Loss
   ↓
View Crop Dashboard
   ↓
Generate Reports
   ↓
Export PDF
```

---

# 🔒 Login Protection

The application uses a custom `login_required` decorator.

The decorator checks whether the user ID exists in the Flask session.

Conceptually:

```text
Request protected page
        ↓
Is user logged in?
      /       \
    Yes        No
     ↓          ↓
Allow       Redirect
access      to login
```

This prevents unauthenticated users from directly accessing protected application pages.

---

# 🧮 Business Logic

Important calculations implemented in the project include:

### Income

```text
Total Income = Quantity × Price Per Unit
```

### Overall Profit

```text
Profit = Total Income - Total Expense
```

### Crop Profit

```text
Crop Profit = Crop Income - Crop Expense
```

These calculations are performed on the backend using the stored database records.

---

# 🚀 Installation and Setup

## 1. Clone the repository

```bash
git clone https://github.com/Jagankale/FarmMangementSystem.git
```

## 2. Navigate to the project

```bash
cd FarmMangementSystem/FMS-main
```

## 3. Create a virtual environment

```bash
python -m venv venv
```

## 4. Activate the virtual environment

### Windows

```bash
venv\Scripts\activate
```

## 5. Install dependencies

Install the required packages:

```bash
pip install flask flask-sqlalchemy xhtml2pdf
```

## 6. Run the application

```bash
python app.py
```

The Flask development server will start and the application can be accessed through the local server URL shown in the terminal.

---

# 🧪 Testing

The application can be tested by checking:

* User registration
* Login with valid credentials
* Login with invalid credentials
* Adding crops
* Editing crops
* Deleting crops
* Adding expenses
* Editing expenses
* Deleting expenses
* Adding income
* Editing income
* Deleting income
* Profit calculations
* Crop-wise calculations
* Report generation
* PDF export
* Logout
* Accessing protected pages without login

---

# 🔮 Future Enhancements

Possible future improvements include:

* Stronger form validation
* Improved authentication and authorization
* Password reset functionality
* RESTful API architecture
* Cloud database integration
* Cloud deployment
* More advanced analytics dashboards
* Weather API integration
* Crop recommendation using machine learning
* Yield prediction
* Mobile application
* Notifications and reminders
* Multi-language support for farmers

---

# 👨‍💻 Learning Outcomes

Through this project, I gained practical experience in:

* Python programming
* Flask web development
* Backend routing
* HTML/CSS frontend development
* Bootstrap
* JavaScript
* SQL and relational databases
* SQLAlchemy ORM
* CRUD operations
* User authentication
* Session management
* Database relationships
* Business logic implementation
* Report generation
* PDF generation
* Debugging and testing
* Full-stack application development

---

## 📌 Key Project Highlights

```text
✓ User Registration & Login
✓ Password Hashing
✓ Session-Based Authentication
✓ Crop Management
✓ Expense Management
✓ Income Management
✓ CRUD Operations
✓ Automatic Income Calculation
✓ Profit/Loss Calculation
✓ Crop-Wise Analysis
✓ Dashboard
✓ Reports
✓ Reports API
✓ PDF Export
✓ SQLite Database
✓ SQLAlchemy ORM
✓ Responsive UI using Bootstrap
```

---

## 📜 License

This project is developed for educational and academic purposes.
