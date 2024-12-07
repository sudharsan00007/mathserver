# Ex.05 Design a Website for Server Side Processing
# Date:02/12/2024
# AIM:
To design a website to calculate the power of a lamp filament in an incandescent bulb in the server side.

# FORMULA:
P = I2R
P --> Power (in watts)
 I --> Intensity
 R --> Resistance

# DESIGN STEPS:
## Step 1:
Clone the repository from GitHub.

## Step 2:
Create Django Admin project.

## Step 3:
Create a New App under the Django Admin project.

## Step 4:
Create python programs for views and urls to perform server side processing.

## Step 5:
Create a HTML file to implement form based input and output.

## Step 6:
Publish the website in the given URL.

# PROGRAM : SUDHARSAN.S(24009664)
~~~
index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Filament Power Calculator</title>
    <!-- Include Bootstrap CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@300;500&family=Oswald:wght@500&display=swap" rel="stylesheet">
    <style>
        body {
            background: linear-gradient(135deg, #f6d365 0%, #fda085 100%);
            font-family: 'Roboto', sans-serif;
            color: #333;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }
        .card {
            width: 100%;
            max-width: 500px;
            border: none;
            border-radius: 20px;
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.15);
            background: #fff;
        }
        .card-header {
            background-color: #4e73df;
            color: #fff;
            border-radius: 20px 20px 0 0;
            text-align: center;
            padding: 1.5rem;
            font-family: 'Oswald', sans-serif;
            font-size: 1.8rem;
            font-weight: 500;
        }
        .formula {
            text-align: center;
            font-family: 'Oswald', sans-serif;
            color: #4e73df;
            font-size: 1.2rem;
            margin-bottom: 1.5rem;
        }
        .form-control {
            border-radius: 10px;
            font-size: 1rem;
        }
        .btn {
            background-color: #4e73df;
            border: none;
            border-radius: 10px;
            font-size: 1rem;
            padding: 10px 15px;
        }
        .btn:hover {
            background-color: #3751c9;
        }
        .result {
            margin-top: 20px;
            text-align: center;
            color: #28a745;
            font-weight: 600;
        }
        footer {
            margin-top: 20px;
            text-align: center;
            font-size: 0.9rem;
            color: #6c757d;
        }
    </style>
</head>
<body>
    <div class="card">
        <div class="card-header">
            Filament Power Calculator
        </div>
        <div class="card-body p-4">
            <div class="formula">
                Power = V² / R
            </div>
            <form method="POST">
                {% csrf_token %}
                <div class="mb-3">
                    <label for="voltage" class="form-label">Voltage (V):</label>
                    <input type="text" id="voltage" name="voltage" class="form-control" placeholder="Enter voltage in volts" required>
                </div>
                <div class="mb-3">
                    <label for="resistance" class="form-label">Resistance (Ω):</label>
                    <input type="text" id="resistance" name="resistance" class="form-control" placeholder="Enter resistance in ohms" required>
                </div>
                <button type="submit" class="btn btn-primary w-100">Calculate</button>
            </form>
            {% if result is not None %}
                <div class="result mt-4">
                    <strong>Calculated Power:</strong> {{ result }} W
                </div>
            {% endif %}
        </div>
    </div>
    <footer>
        &copy; 2024 Filament Calculator | Crafted with ❤ and Bootstrap
    </footer>
    <!-- Include Bootstrap JS -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
~~~
~~~
views.py
from django.shortcuts import render

def index(request):
    result = None  # To store the calculation result
    if request.method == 'POST':
        try:
            # Retrieve inputs from the POST request
            voltage = float(request.POST.get('voltage'))
            resistance = float(request.POST.get('resistance'))
            
            # Validate resistance to avoid division by zero
            if resistance > 0:
                result = (voltage ** 2) / resistance
                
                # Log inputs and outputs to the server console
                print(f"Input received - Voltage: {voltage}V, Resistance: {resistance}Ω")
                print(f"Calculated Power: {result}W")
            else:
                result = "Resistance must be greater than zero."
                print("Error: Resistance must be greater than zero.")
        except ValueError:
            result = "Invalid input. Please enter numeric values."
            print("Error: Invalid input. Non-numeric values entered.")
    else:
        print("GET request received; no calculation performed.")
    
    return render(request, 'index.html', {'result': result})
~~~

~~~
urls.py
"""
URL configuration for lamp_power project.

The `urlpatterns` list routes URLs to views. For more information please see:
    https://docs.djangoproject.com/en/5.1/topics/http/urls/
Examples:
Function views
    1. Add an import:  from my_app import views
    2. Add a URL to urlpatterns:  path('', views.home, name='home')
Class-based views
    1. Add an import:  from other_app.views import Home
    2. Add a URL to urlpatterns:  path('', Home.as_view(), name='home')
Including another URLconf
    1. Import the include() function: from django.urls import include, path
    2. Add a URL to urlpatterns:  path('blog/', include('blog.urls'))
"""
from django.contrib import admin
from django.urls import path
from myapp import views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', views.index, name='index'),
]
~~~

~~~
settings.py:
"""
Django settings for sam project.

Generated by 'django-admin startproject' using Django 5.1.2.

For more information on this file, see
https://docs.djangoproject.com/en/5.1/topics/settings/

For the full list of settings and their values, see
https://docs.djangoproject.com/en/5.1/ref/settings/
"""

from pathlib import Path

# Build paths inside the project like this: BASE_DIR / 'subdir'.
BASE_DIR = Path(__file__).resolve().parent.parent


# Quick-start development settings - unsuitable for production
# See https://docs.djangoproject.com/en/5.1/howto/deployment/checklist/

# SECURITY WARNING: keep the secret key used in production secret!
SECRET_KEY = 'django-insecure-232-^9^p(5-hw-wtue)l^w884c0!15cjvjx!rr!#&+w-s5em%u'

# SECURITY WARNING: don't run with debug turned on in production!
DEBUG = True

ALLOWED_HOSTS = []


# Application definition

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'myapp',
]

MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]

ROOT_URLCONF = 'sam.urls'

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]

WSGI_APPLICATION = 'sam.wsgi.application'


# Database
# https://docs.djangoproject.com/en/5.1/ref/settings/#databases

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}


# Password validation
# https://docs.djangoproject.com/en/5.1/ref/settings/#auth-password-validators

AUTH_PASSWORD_VALIDATORS = [
    {
        'NAME': 'django.contrib.auth.password_validation.UserAttributeSimilarityValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.MinimumLengthValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.CommonPasswordValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.NumericPasswordValidator',
    },
]


# Internationalization
# https://docs.djangoproject.com/en/5.1/topics/i18n/

LANGUAGE_CODE = 'en-us'

TIME_ZONE = 'UTC'

USE_I18N = True

USE_TZ = True


# Static files (CSS, JavaScript, Images)
# https://docs.djangoproject.com/en/5.1/howto/static-files/

STATIC_URL = 'static/'

# Default primary key field type
# https://docs.djangoproject.com/en/5.1/ref/settings/#default-auto-field

DEFAULT_AUTO_FIELD = 'django.db.models.BigAutoField'
~~~
# SERVER SIDE PROCESSING:![Screenshot 2024-12-07 204900](https://github.com/user-attachments/assets/690b645d-b941-450e-91ef-9df3f9b2f50a)


# RESULT:
The program for performing server side processing is completed successfully.
