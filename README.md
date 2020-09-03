# Invoicing

Background: This is an older invoicing project that is being revisited. I had completed a 4 year residential carpentry apprenticeship program and acquired my journeyman (national) license through the Home Builders Association in 2012. From that point, I branched off onto my own as a General Contractor and started contract-based jobs on bank-owned properties. I'd like to think that I am a very organzied person, so after a couple of contract jobs, I felt that I needed to come up with an invoicing system that fit my needs.  Needless to say that I did not want to spend the money to purchase Quickbooks. After discussing with my clients about their needs (and frustrations with previous contractors), I was able to form a game plan. By the way, the finished product had been written in PHP5, HTML/CSS with MYSQL database integration. 

  - Client needs:
    - access to website
    - receive bids
    - receive or view invoice
    - view or download images attached to invoice (before, during and after images of property)
    
  - Business needs:
    - access to website via desktop computer or mobile
    - track expenses, vehicle usage, receipts
    - reporting system for taxes
    - dashboard to view the latest (open bids, unpaid invoices, etc)
    - create bids
    - create invoices
    - upload and attach images to invoices
    - create SKU system to avoid retyping
    - to do list for the website (changelog)
    
Current: After just graduating in August 2020 from Eastern Washington University (EWU) with a Bachelor's Degree in Computer Science and User Experience Design Certificate, I am searching for a career opportunity. Of course they want to see stuff that I have done (not homework assignments). I would be embarassed to show them this old project, so I am doing a major overhaul on it. I am attempting to do the Back-end in Golang and/or Django (as 2 separate projects). Ultimately, I will integrate Amazon Web Services into this as well.

Tools:

- Front-End Development: 
  - [ReactJS](http://reactjs.org/)
  - Javascript
  - HTML/HTML5
  - CSS/CSS3
- Back-End Development: 
  - [Django](https://www.djangoproject.com/) with [Python3](https://www.python.org/)
  - [Golang](https://golang.org/)
- Database: [MariaDB](https://mariadb.org/)
- Version Control: [Git](https://git-scm.com/)
- Cloud: [AWS](https://aws.amazon.com/)
- IDE: [Visual Studio Code](https://code.visualstudio.com/)

## 

## Step 1. Database

### Database reDesign

#### UML

- Tool: 

  - [draw.io](https://www.diagrams.net/)

Original Database Structure
![old database design](https://github.com/jcampbell18/invoicing/blob/master/UML/DatabaseDiagram_original.jpg)

Improved Database Structure
![new database design](https://github.com/jcampbell18/invoicing/blob/master/UML/DatabaseDiagram_v1-1.jpg)

#### Script for new Database design

- Tools:

  - [XAMPP](https://www.apachefriends.org/)
    - used PhpMyAdmin to create database and structure, and populate data

  - [MYSQL Workbench](https://www.mysql.com/products/workbench/)
    - used to create schema, foreign keys, scripts, etc.
    - note: not needed creating the database and data...use phpmyadmin
      - in fact, unable to run the sql scripts in MySQL Workbench due to PRMARY KEY duplicate error

- Links:

  - [Create Database/Structure](https://github.com/jcampbell18/invoicing/blob/master/DatabaseScript/structure.sql)
  - [Populate Database with Data](https://github.com/jcampbell18/invoicing/blob/master/DatabaseScript/data.sql)

## Step 2. Create Back-End

### Django

- References:

  - [Build a REST API in 30 minutes with Django REST framework](https://medium.com/swlh/build-your-first-rest-api-with-django-rest-framework-e394e39a482c)

  - [Data Flair](https://data-flair.training/blogs/install-django/)

  - [Django Best Practices: Projects vs Apps](https://learndjango.com/tutorials/django-best-practices-projects-vs-apps)

#### Environment Setup

1. Install [Python3](https://www.python.org/downloads/)

2. Install [Git Bash](https://git-scm.com/downloads)

3. Update pip (a good habit to have)

  <code>python -m pip install --upgrade pip</code>

4. Setup virtual environment

  4.1. command: python -m venv {virtual environment name}

  <code>python -m venv venv</code>

5. Activate virtual environment

  <code>source venv/Scripts/activate</code>

  - (venv) will appear on the command line to show you have activated the virtual environment

6. Install packages through [Python Package Index](https://pypi.org/)

  6.1. install [Django](https://pypi.org/project/Django/)

  <code>pip install django</code>

  6.2. install [Django REST framework](https://pypi.org/project/djangorestframework/)

  <code>pip install djangorestframework</code>

  6.3. install [Django CORS Headers](https://pypi.org/project/django-cors-headers/)

  <code>pip install django-cors-headers</code>

  6.4. install [Django MySQL](https://pypi.org/project/django-mysql/)

  <code>pip install django-mysql</code>

  6.5. create list of installed packages
  
  <code>pip freeze > requirements.txt</code>

7. Start Django project

  7.1. command: django-admin.py startproject {project name}

  <code>django-admin.py startproject backroom</code>

  7.2. command: cd {project name}

  <code>cd backroom</code>

  7.3. edit settings.py in {project name}

    7.3.1. in 'INSTALLED_APPS', add lines (at end):

      <code>'rest_framework',</code>
      <code>'corsheaders',</code>
      <code>'django_mysql', </code>

    7.3.2. in 'MIDDLEWARE', add line (at top):

      <code>'corsheaders.middleware.CorsMiddleware',</code>

8. Verify it works

  8.1. command: cd {project name}

  <code>cd backroom</code>

  8.2. run server 

  <code>python manage.py runserver</code>

  8.3. with Internet browser (e.g., Chrome), go to URL:

    <code>http://127.0.0.1:8000/</code>

  8.4. exit the server in Git Bash: Ctrl + C

9. Django App

  9.1. Migrations

    9.1.1. command for integrating the project (checks for new additions)

      <code>python manage.py makemigrations</code>

    9.12. command for adding the new additions

      <code>python manage.py migrate</code>

  9.3. Django Model

    9.3.1. command: django-admin startapp {app name}

      <code>django-admin startapp access_levels</code>

    9.3.2. edit settings.py in {project name}

      9.3.2.1. in 'INSTALLED_APPS', add lines (at end):

      <code># Apps</code>

      <code>'access_levels',</code>

    9.3.3. edit models.py in {app name}

      <code>
      from django.db import models

      class AccessLevel(models.Model):
          name = models.TextField(max_length = 50)
          description = models.TextField(max_length = 250)
      </code>

    9.3.4. command for integrating the project (checks for new additions)

      <code>python manage.py makemigrations</code>

    9.3.5. command for adding the new additions

      <code>python manage.py migrate</code>

  9.x. create super user account

  <code>python manage.py createsuperuser</code> 

10. Views

11. Templates


### Golang

- References:

  - [Creating a RESTful API With Golang](https://tutorialedge.net/golang/creating-restful-api-with-golang/)
  - [REST API in Golang](https://golangdocs.com/rest-api-in-golang)

## Git

Git Reference: [Atlassian](https://www.atlassian.com/git/tutorials)

Git Tutorials: [Git Immersion](https://gitimmersion.com/index.html)

### Common commands used

- configuration (also in .gitconfig)
  - add/view user name

    <code>git config --global user.name</code>

  - add/view user email

    <code>git config --global user.email</code>

- cloning

  <code>git clone https://github.com/username/repository-name.git</code>

- check status

  <code>git status</code>

- staging changes
  - by filename

    <code>git add filename</code>

  - all changes

    <code>git add .</code>

- commiting changes

  <code>git commit -m "message"</code>

- uploading/pushing to repository

  <code>git push</code>

- downloading/pulling from repository

  <code>git pull</code>