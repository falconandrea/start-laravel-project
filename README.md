# First config and tricks for new Laravel project

<p align="center"><a href="https://laravel.com" target="_blank"><img src="https://raw.githubusercontent.com/laravel/art/master/logo-lockup/5%20SVG/2%20CMYK/1%20Full%20Color/laravel-logolockup-cmyk-red.svg" width="400"></a></p>

## Requirements

-   Laravel
-   Composer
-   Docker
-   Sail

## Initial

### Update file .env

```
APP_NAME="Laravel Project"
APP_URL="http://laravel-project.test"
DB_USERNAME="laravel"
DB_PASSWORD="laravel"
```

### Sail Installation (only for first start)

```
composer require laravel/sail --dev
```

and add alias into your bash/zsh file

```
alias sail='bash vendor/bin/sail'
```

add docker-compose.yml, update .env vars with docker vars

```
php artisan sail:install
```

start docker containers

```
sail up -d
```

and add APP_KEY in .env with

```
sail artisan key:generate
```

connect storage folder

```
sail artisan storage:link
```

### Start up (after first installation)

```
sail up -d
```

Update database with new migrations

```
sail artisan migrate
```

## Notes

-   After update docker-compose.yml, rebuild docker containers with

```
sail build --no-cache
sail up
```

-   When you use Sail, the commands npm, artisan and composer MUST be used with sail command.

## Telescope (for local development)

Follow the guide here: [Laravel Tescope Install guide](https://laravel.com/docs/8.x/telescope#local-only-installation)

For Dashboard Authorization, update the function with this check

```
protected function gate()
{
    Gate::define('viewTelescope', function ($user) {
        return $this->app->environment('local');
    });
}

```

## Ui Tailwind CSS
View on [Git](https://github.com/laravel-frontend-presets/tailwindcss)
```
composer require laravel-frontend-presets/tailwindcss --dev
php artisan ui tailwindcss --auth
npm install && npm run dev
```

### Notes

If you get error `sh: mix: command not found`

```
npm install laravel-mix@latest
```

## For secondary database for test
Create file `.env.testing` duplicated from `.env` and change only this row:
```
DB_HOST=mysql_test
```
Update `docker-compose.yml` with this rows
```
mysql_test:
    image: "mysql:8.0"
    environment:
        MYSQL_ROOT_PASSWORD: "${DB_PASSWORD}"
        MYSQL_DATABASE: "${DB_DATABASE}"
        MYSQL_USER: "${DB_USERNAME}"
        MYSQL_PASSWORD: "${DB_PASSWORD}"
        MYSQL_ALLOW_EMPTY_PASSWORD: "yes"
    networks:
        - sail
```


## License

The Laravel framework is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).
