# Installation Instructions

To install Laravel on Manjaro/Arch Linux, follow these steps:

**Install PHP and Apache web server:**

```bash
sudo pacman -S php apache apr apr-util composer php-apache php-cgi xdebug

```

**Resolving File Upload Size Bug:**

- If you encounter an issue where files fail to upload despite attempts and you receive a `The screenshot failed to upload` error, it likely indicates that the file size exceeds the permitted `upload_max_filesize` set in your `php.ini` configuration file. Follow these steps to address the problem:

```php
array:1 [ // app/Http/Controllers/ScreenshotController.php:17
  "screenshot" => 
Illuminate\Http
\
UploadedFile {#328
    -test: false
    -originalName: "screenshot.png"
    -mimeType: "image/png"
    -error: 1 <--- this line here
    #hashName: null
    path: "/tmp"
    filename: "php1Mn9jp"
    basename: "php1Mn9jp"
    pathname: "/tmp/php1Mn9jp"
    extension: ""
    realPath: "/tmp/php1Mn9jp"
    aTime: 2024-03-20 16:22:02
    mTime: 2024-03-20 16:22:02
    cTime: 2024-03-20 16:22:02
    inode: 6343
    size: 5911218
    perms: 0100600
    owner: 1000
    group: 1000
    type: "file"
    writable: true
    readable: true
    executable: false
    file: true
    dir: false
    link: false
  }
]
```

Upon examining the `dd()` output, it becomes apparent that the `screenshot` field is present in the request, yet an error arises during the upload process. Specifically, the `error: 1` property within the `UploadedFile` object suggests a hiccup during file upload.

This error code, `1`, aligns with `UPLOAD_ERR_INI_SIZE`, signifying that the file surpasses the limit defined by the `upload_max_filesize` directive in your php.ini configuration.

To rectify this issue, consider elevating the `upload_max_filesize` directive within your php.ini configuration file to accommodate larger file uploads. Here's a step-by-step guide to help you:

1. Locate your php.ini file. This file is typically situated in the `/etc/php/` directory for Linux systems or within `C:\xampp\php` for XAMPP users on Windows.

2. Open the php.ini file using a text editor.

3. Hunt down the `upload_max_filesize` directive. It typically appears as follows:

   ```php
   upload_max_filesize = 2M
   ```

4. Adjust the value to accommodate the size of the files you intend to upload. For instance:

   ```php
   upload_max_filesize = 10M
   ```

5. Save the alterations made to the php.ini file.

6. Restart your web server, whether it's Apache or Nginx, to ensure the changes take effect.

**Enable PHP Extensions:**

Edit the `php.ini` file and enable the required extensions like `bcmath` and `zip`.

- Uncomment the following from the `php.ini` file which can be found using the `php --ini` command.

```
extension=pdo_mysql
extension=mysqli
```

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

## Instructors

- Yahoo Baba
- Laracasts
- Amir Kamizi + [His Blog](https://github.com/amirkamizi/learn-php?tab=readme-ov-file)
- Bitfumes
- PHP TechLife
