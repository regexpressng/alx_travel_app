# 🛫 ALX Travel App

This project is a **Django-based backend application** for managing travel listings.  
It includes REST APIs, Swagger documentation, Celery for background tasks, and MySQL as the database.  

---

## 📑 Objectives
- Set up Django project with necessary dependencies.
- Configure database connection using MySQL with environment variables.
- Add Swagger documentation with **drf-yasg**.
- Enable CORS headers for cross-origin API access.
- Initialize Git repository for version control.

---

## ⚙️ Installation & Setup
 - First we create an environment with venv
```bash
python3 -m venv venv
```

- Then we activate our environment
```bash
source venv/bin/activate
```

 -  Intall Django 
 ```bash
 python3 -m pip install Django
 ```

 - We create a django project named alx_travel_app
 ```bash
 django-admin startproject alx_travel_app
 ```

- Create an app called listing for the alx-travel-app
```bash
python manage.py startapp listings
```
 - Install all dependencies
 - Add required apps to the list of installed apps in the settings file( refer to configure setting.py in project setup)
 -setup your mysql database for django
 ```bash
 sudo apt-get install build-essential python3-dev default-libmysqlclient-dev pkg-config
 pip install mysqlclient django-environ
 ```
 - Setup environment variables for the database
 - Setup your mysql database
 ```sql
 CREATE DATABASE alx_travel;
 CREATE USER 'alx_user'@'localhost' IDENTIFIED BY 'yourpassword';
 GRANT ALL PRIVILEGES ON alx_travel.* TO 'alx_user'@'localhost';
 FLUSH PRIVILEGES;
 ```

- Setup your swagger-api documentation (## Swagger API Docs)

 - Run the django app
 ```bash
 python manage.py runserver
 ```

  


### 1️⃣ Clone the Repository
```bash
git clone https://github.com/your-username/alx_travel_app.git
cd alx_travel_app
```

### 2️⃣ Create Virtual Environment
```bash
python3 -m venv venv
source venv/bin/activate   # On Windows use venv\Scripts\activate
```

### 3️⃣ Install Dependencies
```bash
pip install -r requirements.txt
```

---

## 📦 Dependencies

Your `requirements.txt` should include:

```
Django>=5.0
djangorestframework
django-cors-headers
mysqlclient
celery
drf-yasg
django-environ
```

---

## 🚀 Project Setup

### 1️⃣ Create Django Project
```bash
django-admin startproject alx_travel_app
cd alx_travel_app
python manage.py startapp listings
```

### 2️⃣ Configure `settings.py`

**Installed Apps**
```python
INSTALLED_APPS = [
    ...
    'rest_framework',
    'corsheaders',
    'drf_yasg',
    'listings',
]
```

**Middleware**
```python
MIDDLEWARE = [
    "corsheaders.middleware.CorsMiddleware",
    "django.middleware.common.CommonMiddleware",
    ...
]
```

**CORS**
```python
CORS_ALLOW_ALL_ORIGINS = True
```

**Database (MySQL with `django-environ`)**
```python
import environ
import os

env = environ.Env()
environ.Env.read_env(os.path.join(BASE_DIR, '.env'))

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': env('DB_NAME'),
        'USER': env('DB_USER'),
        'PASSWORD': env('DB_PASSWORD'),
        'HOST': env('DB_HOST'),
        'PORT': env('DB_PORT', default='3306'),
    }
}
```

---

### 3️⃣ Environment Variables (`.env`)
```
DB_NAME=alx_travel
DB_USER=root
DB_PASSWORD=yourpassword
DB_HOST=localhost
DB_PORT=3306
```

---

## 📖 Swagger API Docs

Install **drf-yasg** and configure it in `urls.py`:

```python
from django.contrib import admin
from django.urls import path, re_path
from rest_framework import permissions
from drf_yasg.views import get_schema_view
from drf_yasg import openapi

schema_view = get_schema_view(
   openapi.Info(
      title="ALX Travel API",
      default_version='v1',
      description="API documentation for ALX Travel App",
   ),
   public=True,
   permission_classes=(permissions.AllowAny,),
)

urlpatterns = [
    path('admin/', admin.site.urls),
    re_path(r'^swagger/$', schema_view.with_ui('swagger', cache_timeout=0), name='schema-swagger-ui'),
]
```

Now, API documentation will be available at:

```
http://localhost:8000/swagger/
```

---

## 🐇 Celery & RabbitMQ Setup

### 1️⃣ Install RabbitMQ
```bash
sudo apt-get install rabbitmq-server
sudo service rabbitmq-server start
```

### 2️⃣ Configure Celery in `alx_travel_app/__init__.py`
```python
from __future__ import absolute_import, unicode_literals
from .celery import app as celery_app

__all__ = ('celery_app',)
```

### 3️⃣ Create `alx_travel_app/celery.py`
```python
import os
from celery import Celery

os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'alx_travel_app.settings')

app = Celery('alx_travel_app')
app.config_from_object('django.conf:settings', namespace='CELERY')
app.autodiscover_tasks()
```

### 4️⃣ Run Celery Worker
```bash
celery -A alx_travel_app worker -l info
```

---

## 📝 Git Initialization

```bash
git init
git add .
git commit -m "Initial Django project setup with Swagger, MySQL, and Celery"
```

---

## ✅ Next Steps
- Add API endpoints inside the **listings** app.
- Write unit tests for API endpoints.
- Configure CI/CD for automated testing and deployment.

---

## 📂 Example Files

### `requirements.txt`
```
Django>=5.0
djangorestframework
django-cors-headers
mysqlclient
celery
drf-yasg
django-environ
```

### `.env.example`
```
DB_NAME=alx_travel
DB_USER=root
DB_PASSWORD=yourpassword
DB_HOST=localhost
DB_PORT=3306
```

---

## 📜 License
This project is licensed under the MIT License.08003999