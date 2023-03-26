
# Sitech_intern_Docker_Django
In this task we create Docker files to run Django application and the postgres database in a separated docker containers locally. 

To be honest, I have followed the below tutorial to create a docker file to run the Django app in a docker container, and as an extra step, I created another docker file to build a docker image that runs the Postgres database in a Docker container. 

  [https://www.digitalocean.com/community/tutorials/how-to-build-a-django-and-gunicorn-application-with-docker](url)
  
  
# Step-1 Clone App Repository
     mkdir polls-project
     cd polls-project
     git clone https://github.com/qaisstarawneh/Sitech_intern_Docker_Django.git
     
# Step-2 Edit dockerfile.postgres
set your database name and username and password 
     
      vim dockerfile.postgres
  ```
  output:
  FROM postgres:latest
  ENV POSTGRES_USER=Your_username
  ENV POSTGRES_PASSWORD=Your_password
  ENV POSTGRES_DB=Your_Database_name
  ``` 
  # Step-3 Edit Database settings in mysite/settings.py 
  Add your changes to the database
  ```
DATABASES = {
     'default': {
         'ENGINE': 'django.db.backends.{}'.format(
             os.getenv('DATABASE_ENGINE', 'sqlite3')
         ),
         'NAME': os.getenv('DATABASE_NAME', 'Database_name'),
         'USER': os.getenv('DATABASE_USERNAME', 'Your_username'),
         'PASSWORD': os.getenv('DATABASE_PASSWORD', 'Your_password'),
         'HOST': os.getenv('DATABASE_HOST', '127.0.0.1'),
         'PORT': os.getenv('DATABASE_PORT', 5432),
         'OPTIONS': json.loads(
             os.getenv('DATABASE_OPTIONS', '{}')
         ),
     }
 }
```

# Step-4 Configuring the Running Environment
Edit env file 
 ```
DJANGO_SECRET_KEY=Your_secret_key
DEBUG=True
DJANGO_ALLOWED_HOSTS=*
DATABASE_ENGINE=postgresql_psycopg2
DATABASE_NAME=Your_database_name
DATABASE_USERNAME=Your_database_username
DATABASE_PASSWORD=Your_database_password 
DATABASE_HOST=172.17.0.2
DATABASE_PORT=5432
DJANGO_LOGLEVEL=info
```

# Step-5 Build Docker images 
Build postgres image:
```
docker build -f dockerfile.postgres -t Name:Tag .
```
Build Django app image:
```
docker build -f dockerfile -t Name:Tag .
```
# Step-6 run Docker images
Run postgres image:

```
docker run --env-file env -p 5432:5432 Name:Tag
```

Run Django image:

1.
```
docker run --env-file env Name:Tag sh -c "python manage.py makemigrations && python manage.py migrate"
```
2.
```
docker run -i -t --env-file env Name:Tag sh
```
3.
```
# python manage.py createsuperuser
```
Enter a username, email address, and password for your user, and after creating the user, hit CTRL+D to quit the container and kill it.

4.
```
docker run --env-file env -p 80:8000 Name:Tag
```

# Step-7 Visit localhost/polls
To make sure that your application has been running, visit http://localhost/polls 


![image](https://user-images.githubusercontent.com/121809992/227419473-354d06fd-59ef-438b-baea-d4fc66c57240.png)



And visit http://localhost/admin 


![image](https://user-images.githubusercontent.com/121809992/227419688-a6935826-7661-43a8-ba8c-0d4d9c59d14a.png)
