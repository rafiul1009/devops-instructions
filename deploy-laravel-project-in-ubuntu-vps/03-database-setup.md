# Database Setup

This guide covers the installation and configuration of MySQL database server and phpMyAdmin for your Ubuntu VPS server.

## MySQL Installation

### Installing MySQL Server

1. Update package list:
   ```bash
   sudo apt update
   ```

2. Install MySQL server:
   ```bash
   sudo apt install mysql-server
   ```

3. Access MySQL shell:
   ```bash
   sudo mysql
   ```

4. Set root password:
   ```sql
   ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'your_strong_password';
   exit;
   ```

## MySQL Configuration

### Configuring MySQL Settings

1. Edit MySQL configuration file:
   ```bash
   sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
   ```

2. Update the following settings:
   ```ini
   key_buffer_size = 128M
   max_allowed_packet = 256M
   ```

3. Restart MySQL service:
   ```bash
   sudo service mysql restart
   ```

## phpMyAdmin Installation

### Installing and Configuring phpMyAdmin

1. Install phpMyAdmin:
   ```bash
   sudo apt update
   sudo apt install phpmyadmin
   ```

2. Create symbolic link:
   ```bash
   sudo ln -s /usr/share/phpmyadmin /var/www/html/phpmyadmin
   ```

3. Set proper permissions:
   ```bash
   sudo chmod -R 755 /var/www/html/phpmyadmin
   sudo chown -R www-data:www-data /var/www/html
   sudo chown -R www-data:www-data /var/www/html/phpmyadmin
   ```

### Configuring Nginx for phpMyAdmin

1. Edit Nginx default configuration:
   ```bash
   cd /etc/nginx/sites-available
   sudo nano default
   ```

2. Add the following configuration:
   ```nginx
   location /phpmyadmin {
       root /var/www/html;
       index index.php index.html index.htm;

       location ~ ^/phpmyadmin/(.+\.php)$ {
           try_files $uri =404;
           root /var/www/html;
           fastcgi_pass unix:/run/php/php8.2-fpm.sock;
           fastcgi_index index.php;
           fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
           include /etc/nginx/fastcgi_params;
       }

       location ~* ^/phpmyadmin/(.+\.(jpg|jpeg|gif|png|svg|webp|ico|tif|tiff|bmp|jfif|css|js|html|htm|xml|txt|json|eot|ttf|woff|woff2|ts|mp4|avi|mkv|mov|wmv|flv|webm|mpeg|3gp|ogg|m4v|asf|vob|mp3|wav|aac|wma|m4a|doc|docx|pdf|rtf|odt|xls|xlsx|ppt|pptx|csv))$ {
           root /var/www/html;
       }
   }
   ```

3. Restart Nginx and PHP-FPM:
   ```bash
   sudo systemctl restart nginx
   sudo systemctl restart php8.2-fpm
   ```

## Security Best Practices

1. Use strong passwords for database users
2. Regularly backup your databases
3. Keep MySQL and phpMyAdmin updated
4. Restrict access to phpMyAdmin by IP if possible
5. Monitor database logs for suspicious activity

## Next Steps

After setting up the database environment, proceed to [Laravel Deployment](04-laravel-deployment.md) to deploy your Laravel application.

## Troubleshooting

### Common Issues

1. MySQL connection issues:
   - Check MySQL service status: `sudo systemctl status mysql`
   - Verify MySQL configuration
   - Check user permissions and authentication method

2. phpMyAdmin access problems:
   - Verify Nginx configuration
   - Check PHP-FPM status
   - Ensure proper file permissions
   - Review error logs: `sudo tail -f /var/log/nginx/error.log`

### Performance Optimization

1. Monitor MySQL performance:
   - Use MySQL slow query log
   - Optimize database indexes
   - Configure proper buffer sizes

2. Regular maintenance:
   - Analyze and optimize tables
   - Monitor disk space usage
   - Clean up unnecessary data and logs