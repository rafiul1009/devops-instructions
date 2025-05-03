# PHP Environment Setup

This guide covers the installation and configuration of PHP, Composer, and related components for your Ubuntu VPS server.

## PHP Installation

### Adding PHP Repository

1. Install required software:
   ```bash
   sudo apt install software-properties-common -y
   ```

2. Add Ondřej Surý PPA for PHP:
   ```bash
   sudo add-apt-repository ppa:ondrej/php
   sudo apt update
   ```

### Installing PHP and Extensions

1. Install PHP 8.2:
   ```bash
   sudo apt install --no-install-recommends php8.2
   ```

2. Install required PHP extensions:
   ```bash
   sudo apt-get install -y php8.2-cli php8.2-common php8.2-mysql php8.2-zip \
   php8.2-gd php8.2-mbstring php8.2-curl php8.2-xml php8.2-bcmath php8.2-fpm
   ```

3. Restart PHP-FPM:
   ```bash
   sudo systemctl restart php8.2-fpm
   ```

4. Configure PHP alternatives:
   ```bash
   sudo update-alternatives --set php /usr/bin/php8.2
   sudo update-alternatives --config phpize
   sudo update-alternatives --config php-config
   ```

5. Verify PHP installation:
   ```bash
   php -v
   ```

## Composer Installation

1. Download Composer installer:
   ```bash
   curl -sS https://getcomposer.org/installer -o /tmp/composer-setup.php
   ```

2. Verify installer hash:
   ```bash
   HASH=`curl -sS https://composer.github.io/installer.sig`
   php -r "if (hash_file('SHA384', '/tmp/composer-setup.php') === '$HASH') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
   ```

3. Install Composer globally:
   ```bash
   sudo php /tmp/composer-setup.php --install-dir=/usr/local/bin --filename=composer
   ```

4. Verify Composer installation:
   ```bash
   composer
   ```

## PHP Configuration

### Configuring php.ini

1. Open PHP configuration file:
   ```bash
   sudo nano /etc/php/8.2/fpm/php.ini
   ```

2. Update the following settings:
   ```ini
   max_execution_time = 300
   max_input_time = 600
   max_input_vars = 10000
   memory_limit = 512M
   post_max_size = 256M
   upload_max_filesize = 50M
   ```

3. Enable required extensions:
   ```ini
   extension=curl
   extension=ffi
   extension=ftp
   extension=fileinfo
   extension=gd
   extension=gettext
   extension=gmp
   extension=intl
   extension=imap
   extension=mbstring
   extension=exif
   extension=mysqli
   extension=openssl
   extension=pdo_mysql
   extension=pdo_sqlite
   extension=soap
   extension=sockets
   extension=sodium
   extension=sqlite3
   extension=tidy
   extension=xsl
   extension=zip
   ```

4. Restart PHP-FPM:
   ```bash
   sudo systemctl restart php8.2-fpm
   ```

## Next Steps

After setting up PHP and Composer, proceed to [Database Setup](03-database-setup.md) to configure MySQL and phpMyAdmin.

## Troubleshooting

### Common Issues

1. PHP-FPM fails to start:
   - Check error logs: `sudo tail -f /var/log/php8.2-fpm.log`
   - Verify configuration: `php-fpm8.2 -t`
   - Check PHP version compatibility

2. Composer memory issues:
   - Increase PHP memory limit
   - Run Composer with `--no-dev` flag for production
   - Use `composer dump-autoload --optimize`

### Best Practices

- Regularly update PHP and extensions
- Monitor PHP-FPM process status
- Keep Composer dependencies up to date
- Implement proper error logging
- Use appropriate PHP settings for your application needs