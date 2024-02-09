# Installation Instructions

To install Laravel on Manjaro/Arch Linux, follow these steps:

**Install PHP and Apache web server:**

```bash
sudo pacman -S php php-apache
```

**Enable PHP Extensions:**

Edit the `php.ini` file and enable the required extensions like `bcmath` and `zip`.

**Install MySQL:**

```bash
sudo pacman -S mariadb
sudo systemctl start mysqld
sudo mysql_secure_installation
```

**Install Composer:**

```bash
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
sudo php composer-setup.php --install-dir=/bin --filename=composer
```

**Install Laravel Installer:**

```bash
composer global require laravel/installer
```

**Export PATH Variables:**

```bash
echo 'export PATH="$PATH:$HOME/.config/composer/vendor/bin"' >> ~/.bashrc
source ~/.bashrc
```

**Create a New Laravel Application:**

```bash
laravel new application_name
```

These commands will install all the necessary components and set up Laravel on your Manjaro/Arch Linux system. Make sure to replace `application_name` with the desired name for your Laravel project.

## Instructors

- Yahoo Baba
- Laracasts
- Amir Kamizi + [His Blog](https://github.com/amirkamizi/learn-php?tab=readme-ov-file)
- Bitfumes
