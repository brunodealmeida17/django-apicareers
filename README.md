# API Documentation

  

## Routes

  

-  **GET /careers/**

- Description: Retrieve a list of all careers.

- Response: List of career objects.

  

-  **GET /careers/{OBJECT_ID}/**

- Description: Retrieve details of a specific career.

- Response: Career object.

  

-  **POST /careers/**

- Description: Create a new career.

- Request Body: Career object.

- Response: Created career object.

  

-  **PUT /careers/{OBJECT_ID}/**

- Description: Update an existing career.

- Request Body: Updated career object.

- Response: Updated career object.

  

-  **DELETE /careers/{OBJECT_ID}/**

- Description: Delete an existing career.

- Response: Success message.

  

## Models

  

### Career

-  **id** (int): The unique identifier for the career.

-  **username** (str): The username of the career owner.

-  **create_datetime** (datetime): The datetime when the career was created.

-  **title** (str): The title of the career.

-  **content** (str): The content of the career.

  

Using the API Locally

Requirements

Python 3.6 or higher

  

Django 2.2.9

django-filter 2.2.0

djangorestframework 3.11.0

Markdown 3.1.1 - pytz 2019.3

sqlparse 0.3.0

  

## Using the API Locally

  

### Requirements

- Python 3.10 or higher

- Django 5.0.2-

- djangorestframework 3.14.0

  

### Installation

1. Make sure you have Python installed on your system. You can download the latest version of Python at [python.org](https://www.python.org/).

  

2. Clone this repository to your local development environment:

	`https://github.com/brunodealmeida17/django-apicareers`

  

3. Create a virtual environment to isolate the API dependencies. Navigate to the project root directory and execute the following command:

	`python -m venv myenv`

This will create a virtual environment named "myenv".

  

4. Activate the virtual environment:

- On Windows:

	```myenv\Scripts\activate```

- On macOS/Linux:

	```	source myenv/bin/activate```

  

5. Install the API dependencies. In the project root directory, execute the following command:

	`pip install -r requirements.txt`

  

This will install all the necessary dependencies to run the API locally.

  

6. Apply the database migrations:

	`python manage.py migrate`

  

7. Start the Django development server:

	`python manage.py runserver`

  
  

# Deploy Server Linux/Ubuntu usage Script configuration Gunicorn e nginx

  

## Introduction

This script automates the setup process for a Django project with Celery, Redis, Django Channels, and Gunicorn on a Linux server.

  

## Usage

1.  **Download the script**:

- Clone the repository containing the deployment script:

  

	```https://github.com/brunodealmeida17/django-apicareers```

  

2.  **Navigate to the directory**:

- Move into the cloned directory:

	```cd django-apicareers/Deploy```

  

3.  **Run the script**:

- Execute the deployment script:

	```sudo bash install.sh```

  

4.  **Follow the prompts**:

- The script will prompt you to enter the SERVER_NAME and PORTA (port) for your server.

  

5.  **Installation process**:

- The script will automatically install the required dependencies, create necessary directories, configure NGINX, set up Gunicorn, generate SSL/TLS certificates (if desired), and start the server.

  

6.  **Check status**:

- After running the script, check the status of Gunicorn and NGINX services:

	```sudo systemctl status your_app_name.service```
	```sudo systemctl status nginx```

  

7.  **Access the application**:

- Once the deployment process is complete, you can access your Django application using the configured SERVER_NAME and PORTA (port).


# Django Project with Docker

This project uses Docker to simplify the deployment and setup of a Django environment with PostgreSQL and Nginx. Follow the steps below to properly configure the environment.

## Requirements

- [Docker](https://www.docker.com/)
- [Docker Compose](https://docs.docker.com/compose/)

## Setup

### 1. Cloning the Repository

Clone this repository to your local machine:

```
git https://github.com/brunodealmeida17/django-apicareers
cd django-apicareers
```

### 2. Environment Variables Configuration

You need to configure the required environment variables before running the project.

1. Locate the `.env-example` file in the root of the project.
2. Rename it to `.env`:

   ```
   mv .env-example .env
   ```
### Environment Variables Configuration

Open the `.env` file and configure the necessary environment variables. Below are the variables that need to be set:

```
SECRET_KEY=                                # Django secret key
DEBUG=False                                

## Superuser Credentials
SUPER_USER_NAME=root                       # Superuser username
SUPER_USER_PASSWORD=root                   # Superuser password
SUPER_USER_EMAIL=admin@email.com           # Superuser email

## Database Settings
DATABASE_ROOT_PASSWORD=your_db_root_password   # Database root password
DATABASE_DB=your_database_name                 # Database name
DATABASE_USER=your_database_user               # Database user
DATABASE_PASSWORD=your_db_password             # Database password
```

### Modifying the Dockerfile and Nginx Configuration

Before building the containers, you need to adjust the `Dockerfile` and the Nginx configuration file.

1. **Modify the Dockerfile** to reflect your project's specific needs. The file may be located in the project root or inside a `docker` folder. Ensure that you include any necessary dependencies or adjustments for your application.

2. **Modify the `nginx/default.conf` file** as needed. This Nginx configuration file, found in the `nginx/` folder, should be adjusted to ensure correct routing for the application. Example modifications include:

   - Updating the listening ports.
   - Properly configuring the reverse proxy for the Django service.
   - Ensuring that the static and media configurations are correct.

---

### Entrypoint Script (`entrypoint.sh`)

An `entrypoint.sh` file is used to automate important steps such as:

- Applying database migrations.
- Creating the superuser.
- Running the `collectstatic` command to serve static files.

Check the contents of the `entrypoint.sh` file to customize it as needed. Here's an example of how the script can be configured:

```bash
python manage.py migrate --no-input
python manage.py collectstatic --no-input

DJANGO_SUPERUSER_PASSWORD=$SUPER_USER_PASSWORD python manage.py createsuperuser --username $SUPER_USER_NAME --email $SUPER_USER_EMAIL --noinput

gunicorn apicareers.wsgi:application --bind 0.0.0.0:8000
```