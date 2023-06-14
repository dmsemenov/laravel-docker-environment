# Laravel docker environment
### Introduction
Docker environment configuration for Laravel development.
Contains settings for local development for new project or already existing.   
>Repository provides:
- Laravel ^9.0 || ^10.0
- PHP 8.2
- Nginx: stable
- MySql 8.0
- Xdebug 3.2.0
- Redis
- Node: latest

>Note: If you want to use PHP 7 ``git clone --depth 1 --branch php-7.x git@github.com:dmsemenov/laravel-docker-environment.git``
  
### Installation
**Note:** Local environment tested on Ubuntu and macOS. Feel free to create an Issue for other OS.   
1. **Clone repository**
    ````
    mkdir {YOUR_NEW_FOLDER}
    cd {YOUR_NEW_FOLDER}
    git clone git@github.com:dmsemenov/laravel-docker-environment.git .
    ````
2. **Configure main `.env` file**
    ````
    cp .env.example .env
    ````
   Set `PROJECT_NAME` variable.  
   Set `PROJECT_FOLDER` variable for example 'backend' or whatever you want. By default it is `main`.


3. **Configure main `docker-compose.yml` file**
   ````
   # Local development
   cp docker-compose.yml.example docker-compose.yml
   ````
   Set variables for mysql container. Docker will create database and user at first UP.
   ````
   MYSQL_ROOT_PASSWORD:  
   MYSQL_DATABASE: ${PROJECT_NAME}  
   MYSQL_USER: ${PROJECT_NAME}  
   MYSQL_PASSWORD:
   ````

3. **Install Application**  
   Install or copy your existing application to `PROJECT_FOLDER`.  
   ````
   # Run containers
   docker-compose up -d --build
   # Install new
   docker-compose exec php-fpm composer create-project --prefer-dist laravel/laravel .
   ````

4. **Configure Application `.env`**

   Set variables from *step 3* 
   ````
   DB_HOST=mysql  
   DB_DATABASE=  
   DB_USERNAME=  
   DB_PASSWORD=  
   ````
5. **Run Application**
   ````
   # Run containers
   docker-compose up -d
   
   # Base UI. If needed!
   docker-compose exec -uwww-data php-fpm composer require laravel/ui --dev
   docker-compose exec -uwww-data php-fpm php artisan ui bootstrap --auth
   
   # Build frontend
   docker-compose run --rm --no-deps -unode node bash -ci 'npm i&&npm run dev'
   
   # Migrate
   docker-compose exec -uwww-data php-fpm php artisan migrate
   
   docker-compose exec -uwww-data php-fpm composer install
   ````
6. **Launch http://localhost:8989/register**