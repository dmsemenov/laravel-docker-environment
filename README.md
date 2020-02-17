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

3. **Install Application**  
   Install or copy your existing application to `PROJECT_FOLDER`.  
   ````
   # Install new
   composer create-project --prefer-dist laravel/laravel `PROJECT_FOLDER`
   ````
3. **Configure main `docker-compose.yml` file**
   ````
   # Local development
   cp docker-compose.yml.local docker-compose.yml
   
   # Production
   cp docker-compose.yml.prod docker-compose.yml 
   ````
   Set variables for mysql container. Docker will create database and user at first UP.  
   ````
   MYSQL_ROOT_PASSWORD:  
   MYSQL_DATABASE: ${PROJECT_NAME}  
   MYSQL_USER: ${PROJECT_NAME}  
   MYSQL_PASSWORD:
   ````
4. **Configure Application `.env`**
    ````
    # By default ${PROJECT_FOLDER}=main 
    cp ${PROJECT_FOLDER}/.env.example cp ${PROJECT_FOLDER}/.env 
    ````
   Set variables from *step 3* 
   ````
   DB_HOST=mysql  
   DB_DATABASE=  
   DB_USERNAME=  
   DB_PASSWORD=  
   ````
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
6. **Launch http://localhost:8989/register**   
   
### LetsEncrypt and Production
If you are going to use LetsEncrypt at production look at branch 'letsencrypt' this repository.  
Branch contains ready to use configuration for letsEncrypt in production based on great article [Nginx and Let’s Encrypt with Docker in Less Than 5 Minutes](https://medium.com/@pentacent/nginx-and-lets-encrypt-with-docker-in-less-than-5-minutes-b4b8a60d3a71).  
To use letsEncrypt at production just follow steps below:
2. Initial settings   
   Open ``./init-letsencrypt.sh`` add your domains instead `{YOUR_DOMAIN}` www.`{YOUR_DOMAIN}` and set `{YOUR_EMAIL}`.   
   Run ``./init-letsencrypt.sh``. It will create initial settings.  
   Note: don't forget to add A DNS record for domain with www. 
3. Nginx settings  
   At `.provision/nginx/conf.d/vhost.prod.conf` set `{YOUR_DOMAIN}` variable.
    
