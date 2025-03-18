---
layout: single
title:  "Introduction to Django"
category: web
tag: [Web, Django]
toc: true
author_profile: false
sidebar:
    nav: "docs"
---

<div class="notice--success">
<h4>Please have a read</h4>
<ul>
	<li>[Welcome!](https://dae-y.github.io/notice/first/)</li>
<ul>
</div>

A summary of Web App Frameworks COMP3011 lecture 1

## What is a Framework?  
A framework is a foundational structure that simplifies and standardizes the development process.  
- It provides a pre-built structure, including libraries, tools, and guidelines for software development.  
- Frameworks focus on **reusability, efficiency, and standardization**.  
- They often include pre-written code libraries and **Application Programming Interfaces (APIs)** to streamline development.  

## Key Django Files and Their Purpose  

### Project-Level Files  
- **`__init__.py`**: Marks a directory as a Python package, allowing module imports.  
- **`settings.py`**: Stores configuration settings for the Django project, including database setup, installed apps, and middleware.  
- **`urls.py`**: Maps URLs to corresponding views or functions that handle requests.  
- **`wsgi.py`**: Serves as the entry point for WSGI-compatible web servers, crucial for deploying Django applications.  
- **`manage.py`**: A command-line utility for managing the Django project (e.g., running the development server, migrations, and creating apps).  

### App-Level Files  
- **`admin.py`**: Registers models with Django’s admin interface for easy management.  
- **`apps.py`**: Contains configuration settings for the app, such as its name and metadata.  
- **`models.py`**: Defines data models, representing the database structure and handling data interactions.  
- **`tests.py`**: Used to write automated tests to verify the correctness of the application.  
- **`urls.py`**: Defines the URL patterns specific to the app.  
- **`views.py`**: Contains view functions or classes that handle user requests and return responses.  

## Django's Architecture: Model-View-Template (MVT)  
Django follows the **MVT pattern**, similar to MVC but with a slight variation.  

- **Model**  
  - Represents the data structure of the application.  
  - Defines database schema using Django models.  
  - Handles Create, Read, Update, and Delete (CRUD) operations.  
  - Defined in **`models.py`**.  

- **View**  
  - Manages application logic.  
  - Handles user input and interacts with the model.  
  - Returns responses (HTML, JSON, etc.).  
  - Defined in **`views.py`**.  

- **Template**  
  - Defines how data is presented to the user.  
  - Uses Django’s templating language to render dynamic HTML pages.  
  - Stored in a **`templates/`** directory within the app.  

Django provides a robust framework for building web applications efficiently, with a focus on clean architecture and scalability.  

