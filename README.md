### Introduction
Provide Docker environment configuration for Laravel development. 
Repo includes:
 - Laravel 6
 - PHP 7.4
 - Nginx: 1.14
 - MySql 8.0
 - Redis
 - Node latest

Branch `develop` consist of local and production configuration.


###Installation
1. ````cp .env.example .env && cp docker-compose.yml.local docker-compose.yml ````
2. Set PROJECT_NAME variable in `.env` file
3. Set database variables in `docker-compose.yml`
    ````
        # By default ${PROJECT_FOLDER}=main 
        cp ${PROJECT_FOLDER}/.env.example cp ${PROJECT_FOLDER}/.env 
    ````

4. Set database config in `${PROJECT_FOLDER}/.env`
    ``docker-compose exec php-fpm php artisan key:generate``