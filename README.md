# Deploy Laravel on Infomaniak

How to deploy an Laravel app (v. 6.x.x) on Infomaniak shared hosting

## Prepare site & access on Infomaniak

- Create a site on Infomaniak
- Create an SSH access for this site

## Configure the hosting

- PHP version should be >= 7.2 (ideally 7.3)

### Install Composer

1. SSH to Infomaniak: `ssh user@host -P`
1. `cd ~`
1. `mkdir .composer`
1. `export COMPOSER_HOME="~/.composer"`
1. `curl -sS https://getcomposer.org/installer | php -d allow_url_fopen=On`

> To allow the launch of Composer from everywhere, type `export PATH=$PATH:/composerPath` where composerPath is given at the install

> If you want to type `composer` instead of `composer.phar`, move to the composer directory and type `mv composer.phar composer`

> More: https://www.infomaniak.com/fr/support/faq/2118/installer-composer-sur-un-hebergement-web-ou-un-serveur-cloud

## Upload your Laravel app

### Via git

1. `cd` to the correct directory
1. `git clone` your project

#### If the project is private and you have 2FA, you can use Github Token

1. Create a token https://github.com/settings/tokens with the correct rights (repo)
1. `git clone` your repo with https
1. Enter your username and the token for the password

## Make the Infomaniak site point to the `/public` folder

## Update the .env file with the correct info

## Configure Laravel

1. `composer install`
1. `php artisan key:generate`
1. `php artisan migrate:fresh --seed`

## Optimization

- `composer install --optimize-autoloader --no-dev`
- `php artisan config:cache`
- `php artisan route:cache`

> More https://laravel.com/docs/6.x/deployment

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

## Infomaniak Support FAQ

> https://www.infomaniak.com/en/support/faq/2119/installing-laravel-51-on-a-web-hosting-or-a-cloud-server
