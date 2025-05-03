# Laravel Deployment

This guide covers the process of deploying a Laravel application on your Ubuntu VPS server, including project setup, file permissions, and SSL configuration.

## Project Setup

### Cloning Your Project

1. Navigate to web directory:
   ```bash
   cd /var/www/html
   ```

2. Clone your project:
   ```bash
   git clone your_project_git_ssh_url
   ```

### Setting File Permissions

1. Set proper directory permissions:
   ```bash
   sudo chmod 755 -R /var/www/html
   sudo chmod 755 -R /var/www/html/project-name
   ```

2. Set proper ownership:
   ```bash
   sudo chown -R www-data:www-data .
   sudo chown -R www-data storage
   sudo chown -R www-data storage/framework
   ```

3. Set storage permissions:
   ```bash
   sudo chmod g+w -R storage
   sudo chmod g+w -R storage/framework
   sudo chmod g+w -R storage/framework/sessions/
   sudo chmod g+w -R storage/logs/
   sudo chmod -R 777 storage/app/ storage/framework/ storage/logs/ bootstrap/cache/
   ```

4. Update dependencies and clear cache:
   ```bash
   composer update
   php artisan optimize:clear
   ```

## Nginx Configuration

### Setting Up Nginx Server Block

1. Create a new server configuration:
   ```bash
   sudo nano /etc/nginx/sites-available/server-conf
   ```

2. Create symbolic link:
   ```bash
   sudo ln -s /etc/nginx/sites-available/server-conf /etc/nginx/sites-enabled/
   ```

3. Test and restart Nginx:
   ```bash
   sudo nginx -t
   sudo systemctl restart nginx
   ```

## SSL Certificate Installation

### Installing and Configuring SSL

1. Install Certbot:
   ```bash
   sudo apt update
   sudo apt install certbot
   sudo apt install python3-certbot-nginx
   ```

2. Obtain SSL certificate:
   ```bash
   sudo certbot --nginx
   ```

3. Test certificate renewal:
   ```bash
   sudo certbot renew --dry-run
   ```

4. Restart Nginx:
   ```bash
   sudo service nginx restart
   ```

5. Setup automatic renewal:
   ```bash
   sudo crontab -e
   ```
   Add the following line:
   ```cron
   0 */12 * * * certbot renew
   ```

## Git Configuration

### Managing Git Cache

1. Clear Git cache:
   ```bash
   git rm -r --cached .
   git add .
   git commit -am 'git cache cleared'
   git push
   ```

## Next Steps

After deploying your Laravel application, proceed to [Additional Services](05-additional-services.md) to set up Supervisor and other services.

## Troubleshooting

### Common Issues

1. Permission issues:
   - Verify file ownership and permissions
   - Check Laravel storage directory permissions
   - Ensure proper Nginx user permissions

2. SSL certificate problems:
   - Verify domain DNS settings
   - Check Certbot logs
   - Ensure ports 80 and 443 are open

### Best Practices

1. Environment configuration:
   - Use proper .env settings
   - Secure sensitive information
   - Configure proper logging

2. Performance optimization:
   - Enable PHP OPcache
   - Configure proper caching
   - Use Laravel's built-in optimization commands

3. Security measures:
   - Keep Laravel framework updated
   - Regular security audits
   - Implement proper authentication
   - Use HTTPS for all traffic