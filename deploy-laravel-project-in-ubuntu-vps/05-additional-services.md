# Additional Services

This guide covers the setup and configuration of additional services required for your Laravel application, including Supervisor for queue management and Reverb for WebSocket functionality.

## Supervisor Installation

### Installing Supervisor

1. Update package list and install Supervisor:
   ```bash
   sudo apt-get update
   sudo apt-get install supervisor
   ```

## Queue Worker Configuration

### Setting Up Laravel Queue Worker

1. Create queue worker configuration:
   ```bash
   sudo nano /etc/supervisor/conf.d/queue-worker.conf
   ```

2. Add the following configuration:
   ```ini
   [program:queue-worker]
   process_name = %(program_name)s_%(process_num)02d
   command=php /var/www/html/app-folder/artisan queue:listen
   autostart=true
   autorestart=true
   user=root
   numprocs=1
   redirect_stderr=true
   stdout_logfile=/var/www/html/app-folder/public/worker.log
   ```

3. Update Supervisor:
   ```bash
   sudo supervisorctl reread
   sudo supervisorctl update
   ```

4. Start queue worker:
   ```bash
   sudo supervisorctl start queue-worker:*
   ```

5. Enable Supervisor on system startup:
   ```bash
   sudo systemctl enable supervisor
   ```

## Reverb Setup

### Configuring Laravel Reverb

1. Create Reverb worker configuration:
   ```bash
   sudo nano /etc/supervisor/conf.d/reverb-worker.conf
   ```

2. Add the following configuration:
   ```ini
   [program:reverb-worker]
   process_name = %(program_name)s_%(process_num)02d
   command=php /var/www/html/app-folder/artisan reverb:start
   autostart=true
   autorestart=true
   user=root
   numprocs=1
   redirect_stderr=true
   minfds=10000
   stdout_logfile=/var/www/html/app-folder/public/reverb.log
   ```

3. Update and start Reverb worker:
   ```bash
   sudo supervisorctl reread
   sudo supervisorctl update
   sudo supervisorctl start reverb-worker:*
   ```

## Log Management

### Monitoring Application Logs

1. View Nginx error logs:
   ```bash
   sudo tail -f /var/log/nginx/error.log
   ```

2. View Nginx access logs:
   ```bash
   sudo tail -f /var/log/nginx/access.log
   ```

3. Clear log files if needed:
   ```bash
   sudo truncate -s 0 /var/log/nginx/error.log
   sudo truncate -s 0 /var/log/nginx/access.log
   ```

## Next Steps

After setting up additional services, proceed to [Node.js and PM2](06-nodejs-and-pm2.md) to configure Node.js environment and process management.

## Troubleshooting

### Common Issues

1. Supervisor issues:
   - Check process status: `sudo supervisorctl status`
   - Review worker logs in specified log files
   - Verify configuration syntax
   - Ensure proper file permissions

2. Queue worker problems:
   - Monitor worker logs
   - Check Laravel queue configuration
   - Verify database connection
   - Ensure proper job class namespaces

3. Reverb connection issues:
   - Check WebSocket server status
   - Verify port availability
   - Review client connection settings
   - Monitor Reverb logs

### Best Practices

1. Queue management:
   - Monitor queue size and processing speed
   - Implement proper error handling
   - Configure job retry and timeout settings
   - Use queue priorities when needed

2. Log rotation:
   - Implement log rotation policies
   - Monitor disk space usage
   - Archive old logs regularly
   - Set appropriate log levels

3. Performance monitoring:
   - Monitor system resources
   - Track queue performance metrics
   - Set up alerts for critical issues
   - Regular maintenance checks