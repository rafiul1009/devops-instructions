# Initial Server Setup

This guide covers the initial setup steps for your Ubuntu VPS server, including server access, web server installation, and basic security configuration.

## Server Access

### Connecting to Your VPS

1. Use PuTTY or any SSH client to connect to your VPS using the password provided by your hosting provider:
   ```bash
   # Connect using your server's IP address
   ssh username@your_server_ip
   ```

## Web Server Setup

### Installing Nginx

1. Update the package list:
   ```bash
   sudo apt update
   ```

2. Install Nginx:
   ```bash
   sudo apt install nginx
   ```

3. Verify the installation:
   ```bash
   systemctl status nginx
   ```

4. Test Nginx configuration:
   ```bash
   sudo nginx -t
   ```

5. Restart Nginx:
   ```bash
   sudo systemctl restart nginx
   ```

## Firewall Configuration

### Setting Up UFW (Uncomplicated Firewall)

1. Check the current firewall status:
   ```bash
   sudo ufw status
   ```

2. Allow Nginx HTTP traffic:
   ```bash
   sudo ufw allow 'Nginx HTTP'
   ```

3. Allow Nginx HTTPS traffic:
   ```bash
   sudo ufw allow 'Nginx Full'
   ```

4. Apply the firewall rules:
   ```bash
   sudo ufw reload
   ```

## Next Steps

After completing the initial server setup, proceed to [PHP Environment Setup](02-php-environment-setup.md) to configure PHP and related components.

## Troubleshooting

### Common Issues

1. If Nginx fails to start:
   - Check the error logs: `sudo tail -f /var/log/nginx/error.log`
   - Verify the configuration: `sudo nginx -t`
   - Ensure no other service is using port 80/443

2. If firewall blocks access:
   - Verify UFW rules: `sudo ufw status`
   - Ensure SSH access is allowed
   - Check if the correct ports are open

### Security Best Practices

- Regularly update your system: `sudo apt update && sudo apt upgrade`
- Monitor server logs for suspicious activity
- Keep backups of important configuration files
- Use strong passwords and consider setting up SSH key authentication