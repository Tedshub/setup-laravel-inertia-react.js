# setup-laravel-inertia-react.js

composer create-project laravel/laravel dss-net
cd dss-net
composer require laravel/breeze --dev
php artisan breeze:install react
npm install
npm run dev
php artisan migrate


MAIL_MAILER=smtp
MAIL_HOST=smtp.mailtrap.io
MAIL_PORT=2525
MAIL_USERNAME=xxxx
MAIL_PASSWORD=xxxx
MAIL_ENCRYPTION=tls
MAIL_FROM_ADDRESS="noreply@dss-net.com"
MAIL_FROM_NAME="${APP_NAME}"


php artisan serve
