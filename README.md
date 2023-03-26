# Sitech_intern_Docker_Django-
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
     
      vim Dockerfile.postgres
  ```
  output:
  FROM postgres
  ARG DATABASE_USERNAME
  ARG DATABASE_PASSWORD
  ARG DATABASE_NAME
  ENV DATABASE_PASSWORD=$DATABASE_PASSWOR
  ENV DATABASE_USERNAME=$DATABASE_USERNAME
  ENV DATABASE_PASSWORD=$DATABASE_PASSWORD
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
DJANGO_SECRET_KEY=Qaiss
DEBUG=True
DJANGO_ALLOWED_HOSTS=*
DATABASE_ENGINE=postgresql_psycopg2
DATABASE_NAME=polls
DATABASE_USERNAME=Qaiss
DATABASE_PASSWORD=Qaiss
DATABASE_HOST=psgrs
DATABASE_PORT=5432
DJANGO_LOGLEVEL=info
```

# Step-5 Build Docker images 
Build postgres image:
```
docker build --build-arg DATABASE_USERNAME=Qaiss --build-arg DATABASE_PASSWORD=Qaiss --build-arg DATABASE_NAME=polls -t polls .
```
Build Django app image:
```
docker build -f Dockerfile -t Name:Tag .
```

# Step-6 Create Network:

```
docker network create Django
```
# Step-7 run Docker images
Run postgres image:

```
docker run --network Django -d -it -p 5432:5432 --hostname psgrs   --name psgrs polls
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

# Step-8 Visit localhost/polls
To make sure that your application has been running, visit http://localhost/polls 


![image](https://user-images.githubusercontent.com/121809992/227419473-354d06fd-59ef-438b-baea-d4fc66c57240.png)



And visit http://localhost/admin 


![image](https://user-images.githubusercontent.com/121809992/227419688-a6935826-7661-43a8-ba8c-0d4d9c59d14a.png)
