# Deploy Laravel on Infomaniak shared hosting

How to deploy a Laravel app (v. 8.x.x) on Infomaniak shared hosting

## Infomaniak - SSH & Database
- Create an SSH access
- Create a database (with associated user)

## Upload your Laravel app

### Via git

1. `cd` to the correct directory
1. `git clone` your project

#### If the project is private and you have 2FA, you can use Github Token

1. Create a token https://github.com/settings/tokens with the correct rights (repo)
1. `git clone` your repo with https
1. Enter your username and the token for the password

## Infomaniak - Create the "site"

- Create a site on Infomaniak (make sure that the site folder point to the `/public` folder of your Laravel app)
- PHP version should be >= 7.3 (ideally 7.4) - [Laravel Server Requirements](https://laravel.com/docs/8.x/deployment#server-requirements)
- Enable `proc_open` for the site (https://www.infomaniak.com/en/support/faq/173/enabling-shell-exec-proc-open-etc-functions)

## Laravel - Fresh start

1. Create the .env file with the correct info
1. `composer install`
1. `php artisan key:generate`
1. `php artisan migrate:fresh --seed`
1. `php artisan storage:link`

## Optimization

- `composer install --optimize-autoloader --no-dev`
- `php artisan config:cache`
- `php artisan route:cache`
- `php artisan view:cache`

> More https://laravel.com/docs/8.x/deployment

---

## To deploy new changes

### Down the site
`php artisan down`

### Update the site
- `git pull`
- `composer install`
- `php artisan migrate`
- Restart FPM (optional) `echo "" | sudo -S service php7.3-fpm reload`
- Restart queue (optional) `php artisan queue:restart`
- Clear cache (optional) `php artisan cache:clear`

### Up the site
`php artisan up`

---

## Issues

**Force HTTPS**

Add the following line to the `public/.htaccess`
```
RewriteCond %{HTTPS} !=on
RewriteRule ^ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
```

**Index column size too large**

Migrate MySQL to version 5.7+

**Can't use Tinker**

Set in `.env` file the following variable:
```
XDG_CONFIG_HOME=./.psysh
```

⚠️ You may need to clear the cache

**Manually create user using Tinker**
```sql
DB::table('users')->insert(['name'=>'MyUsername','email'=>'thisis@myemail.com','password'=>Hash::make('123456')])
```

---

## Infomaniak Support FAQ

> https://www.infomaniak.com/en/support/faq/2119/installing-laravel-51-on-a-web-hosting-or-a-cloud-server
