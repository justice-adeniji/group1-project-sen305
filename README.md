# SEN305 Service Platform: Enterprise Service Request and Maintenance Platform

**Course**: VUA-SEN305 Web Application Development Practical  
**Institution**: Department of Software Engineering, Veritas University Abuja (VUA)  
**Project**: Enterprise Service Request and Maintenance Platform  

---

## Architecture & Layers

This platform is structured following Django's Model-View-Template (MVT) architecture extended with an enterprise **Service/Selector pattern** for transaction safety, authorization, and maintainability:

```
sen305-service-platform/
├── backend/
│   ├── config/                     # Project configuration
│   │   ├── settings.py             # MySQL database, environ, auth, templates
│   │   ├── urls.py                 # Route delegation to core and requests_app
│   │   ├── asgi.py
│   │   └── wsgi.py
│   ├── core/                       # Public presentation slice (Lab Session 2)
│   │   ├── static/core/css/styles.css # Comprehensive responsive styling
│   │   ├── templates/core/
│   │   │   ├── base.html           # Reusable shell with active navigation & messages
│   │   │   ├── home.html           # Hero section and status panel
│   │   │   ├── about.html          # System roles description
│   │   │   ├── contact.html        # Facilities and support contact details
│   │   │   └── workflow.html       # Visual lifecycle stages
│   │   ├── urls.py                 # Core routing
│   │   └── views.py                # Public views
│   ├── requests_app/               # Domain model & workflows (Lab Sessions 3 & 4)
│   │   ├── models.py               # Category, ServiceRequest, WorkOrder, Comment
│   │   ├── forms.py                # ModelForms, AssignmentForm, ResolutionForm
│   │   ├── selectors.py            # Row-level security read queries & aggregates
│   │   ├── services.py             # Transaction-safe business operations
│   │   ├── urls.py                 # Authenticated CRUD and action routes
│   │   ├── views.py                # POST/Redirect/GET views and actions
│   │   ├── admin.py                # Django Admin integration & filters
│   │   ├── migrations/             # 0001_initial.py and 0002_add_due_at_and_resolution.py
│   │   ├── templates/requests_app/
│   │   │   ├── request_list.html   # Search, filters, summary counts, pagination
│   │   │   ├── request_detail.html # Details, assignment, resolution, comments
│   │   │   └── request_form.html   # Reusable create/edit form
│   │   └── tests/                  # Automated model and CRUD test suite
│   ├── templates/
│   │   ├── 404.html                # Custom error page
│   │   └── registration/login.html # Authentication login template
│   ├── .env.example                # Safe environment variable template
│   ├── .env                        # Local development credentials (git-ignored)
│   ├── manage.py
│   └── requirements.txt            # Python dependencies
├── docs/                           # Lab session practical notes
├── frontend/
└── infrastructure/
```

---

## Lab Increments Implemented

### Lab Session 2: Client-Server Model, HTTP, and Django MVT Foundation
- Scaffolded project structure and `core` app.
- Built `base.html` with responsive CSS navigation, active link highlighting (`aria-current="page"`), and accessibility support.
- Built `home`, `about`, `contact`, and `workflow` views and templates.
- Configured project-level custom `404.html`.

### Lab Session 3: Domain Modelling, MySQL ORM, and Migrations
- Configured environment variables with `django-environ` and MySQL connection (`sen305_servicedesk`).
- Modeled enterprise entities: `Category`, `ServiceRequest` (with UUID primary key, priorities 1-5, status lifecycle), `WorkOrder`, and `Comment`.
- Enforced database constraints: uniqueness, check constraints, foreign key deletion rules (`PROTECT` and `CASCADE`).
- Implemented migrations: `0001_initial.py` and `0002_add_due_at_and_resolution.py`.
- Configured Django Admin with custom list displays, filters, search fields, autocomplete, and inline comments.
- Added automated model integrity tests.

### Lab Session 4: Service Layer, Server-Rendered CRUD, Forms, and Transactions
- **Form Layer**: Safe `ServiceRequestForm`, `CommentForm`, `AssignmentForm`, and `ResolutionForm`.
- **Selector Layer**: Strict row-level security (`service_request_list`, `service_request_filter`, `service_request_get`, `service_request_counts`) ensuring users never see or manipulate unauthorized records.
- **Service Layer**: Transaction-atomic business operations (`@transaction.atomic`, `select_for_update()`) for create, update, technician assignment, and resolution.
- **Views & Templates**: Full CRUD with POST/Redirect/GET, authentication integration, messages, pagination with filter preservation, and rollback protection.
- **Testing**: Comprehensive automated test suite (`test_request_crud.py` and `test_models.py`).

---

## Local Development & Setup

1. **Activate Virtual Environment**:
   ```bash
   source .venv/bin/activate
   ```

2. **Install Dependencies**:
   ```bash
   pip install -r backend/requirements.txt
   ```

3. **Configure Environment Variables**:
   ```bash
   cp backend/.env.example backend/.env
   # Ensure MySQL credentials match your local database
   ```

4. **Run Migrations & System Checks**:
   ```bash
   cd backend
   python manage.py check
   python manage.py migrate
   ```

5. **Run Automated Tests**:
   ```bash
   python manage.py test requests_app -v 2
   ```

6. **Start Server**:
   ```bash
   python manage.py runserver
   ```
