### Introduction
Docker environment configuration for Laravel development.
Includes local end production settings.   
>Repository provides:
> - Laravel 6
> - PHP 7.4
> - Nginx: 1.14
> - MySql 8.0
> - Redis
> - Node latest

### Installation
**Note:** Local environment tested only on Ubuntu. Feel free create an Issue for other OS.   
1. **Clone repository**
    ````
    mkdir {YOUR_PROJECT_FOLDER}
    cd {YOUR_PROJECT_FOLDER}
    git clone git@github.com:dmsemenov/laravel-docker-environment.git .
    ````
2. **Configure main `.env` file**
    ````
    cp .env.example .env
    ````
   Set PROJECT_NAME variable.  
   Set PROJECT_FOLDER variable for example `backend` or whatever you want - by default it is main. 
3. **Configure main `docker-compose.yml` file**
   ````
   cp docker-compose.yml.local docker-compose.yml 
   ````
   Set variables for mysql container. Docker will create database and user at first UP.  
   MYSQL_ROOT_PASSWORD:  
   MYSQL_DATABASE: ${PROJECT_NAME}  
   MYSQL_USER: ${PROJECT_NAME}  
   MYSQL_PASSWORD:
4. **Configure App `.env`**
    ````
    # By default ${PROJECT_FOLDER}=main 
    cp ${PROJECT_FOLDER}/.env.example cp ${PROJECT_FOLDER}/.env 
    ````
   Set variables from *step 3* 
       DB_HOST=mysql  
       DB_DATABASE=  
       DB_USERNAME=  
       DB_PASSWORD=  

5. **Run Application**
   ````
   docker-compose up -d --build
   docker-compose exec -uwww-data php-fpm composer install
   docker-compose exec php-fpm php artisan key:generate
   # Base UI
   docker-compose exec -uwww-data php-fpm composer require laravel/ui --dev
   docker-compose exec -uwww-data php-fpm php artisan ui bootstrap --auth
   
   # Build frontend
   docker-compose run --rm --no-deps -unode node bash -ci 'npm i&&npm run dev'
   
   # Migrate
   docker-compose exec -uwww-data php-fpm php artisan migrate
   ````
6. **Run http://localhost:8989/register**   
   
