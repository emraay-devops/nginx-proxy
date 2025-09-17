# Nginx Reverse Proxy Demo

This project demonstrates how to set up nginx as a reverse proxy for multiple web applications. It includes two sample applications:

- **Budget Tracker** (`/budget`) - A personal finance management application
- **Home Roster** (`/roster`) - A household chore management system

## Project Structure

```
nginx-proxy/
├── budget-app/
│   └── index.html          # Budget Tracker application
├── roster-app/
│   └── index.html          # Home Roster application
├── nginx.conf              # Nginx reverse proxy configuration
└── README.md               # This file
```

## Installation on Ubuntu Server

Follow these steps to manually install and configure nginx on an Ubuntu server.

### Prerequisites

- Ubuntu 20.04 LTS or later
- Root or sudo access
- Public IP address: `35.183.32.162`
- Public DNS: `ec2-35-183-32-162.ca-central-1.compute.amazonaws.com`

### Step 1: Update System Packages

```bash
# Update package list
sudo apt update

# Upgrade existing packages
sudo apt upgrade -y

# Install essential packages
sudo apt install -y curl wget unzip software-properties-common
```

### Step 2: Install Nginx

```bash
# Install nginx
sudo apt install -y nginx

# Start nginx service
sudo systemctl start nginx

# Enable nginx to start on boot
sudo systemctl enable nginx

# Check nginx status
sudo systemctl status nginx
```

### Step 3: Configure Firewall

```bash
# Check if ufw is active
sudo ufw status

# Allow HTTP traffic
sudo ufw allow 'Nginx Full'

# Allow SSH (if not already allowed)
sudo ufw allow ssh

# Enable firewall if not already enabled
sudo ufw --force enable

# Check firewall status
sudo ufw status
```

### Step 4: Create Directory Structure

```bash
# Create directories for web applications
sudo mkdir -p /var/www/budget-app
sudo mkdir -p /var/www/roster-app

# Set proper permissions
sudo chown -R www-data:www-data /var/www/
sudo chmod -R 755 /var/www/
```

### Step 5: Deploy Web Applications

```bash
# Copy budget app files
sudo cp -r budget-app/* /var/www/budget-app/

# Copy roster app files
sudo cp -r roster-app/* /var/www/roster-app/

# Set proper ownership
sudo chown -R www-data:www-data /var/www/
```

### Step 6: Configure Nginx Reverse Proxy

```bash
# Backup original nginx configuration
sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/default.backup

# Create new nginx configuration
sudo nano /etc/nginx/sites-available/default
```

Replace the entire content with:

```nginx
# Nginx Reverse Proxy Configuration
server {
    listen 80;
    server_name 35.183.32.162 ec2-35-183-32-162.ca-central-1.compute.amazonaws.com;
    
    # Security headers
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-XSS-Protection "1; mode=block" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header Referrer-Policy "no-referrer-when-downgrade" always;
    add_header Content-Security-Policy "default-src 'self' http: https: data: blob: 'unsafe-inline'" always;

    # Root location - redirect to budget app by default
    location / {
        root /var/www/budget-app;
        index index.html;
        try_files $uri $uri/ =404;
    }

    # Budget App - accessible at /budget or /budget/
    location /budget {
        alias /var/www/budget-app;
        index index.html;
        try_files $uri $uri/ /budget/index.html;
        
        # Remove /budget prefix from requests
        location ~ ^/budget/(.*)$ {
            alias /var/www/budget-app;
            try_files /$1 /$1/ /budget/index.html;
        }
    }

    # Roster App - accessible at /roster or /roster/
    location /roster {
        alias /var/www/roster-app;
        index index.html;
        try_files $uri $uri/ /roster/index.html;
        
        # Remove /roster prefix from requests
        location ~ ^/roster/(.*)$ {
            alias /var/www/roster-app;
            try_files /$1 /$1/ /roster/index.html;
        }
    }

    # Health check endpoint
    location /health {
        access_log off;
        return 200 "healthy\n";
        add_header Content-Type text/plain;
    }

    # Static files caching
    location ~* \.(css|js|png|jpg|jpeg|gif|ico|svg)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
        access_log off;
    }

    # Gzip compression
    gzip on;
    gzip_vary on;
    gzip_min_length 1024;
    gzip_proxied any;
    gzip_comp_level 6;
    gzip_types
        text/plain
        text/css
        text/xml
        text/javascript
        application/json
        application/javascript
        application/xml+rss
        application/atom+xml
        image/svg+xml;

    # Error pages
    error_page 404 /404.html;
    error_page 500 502 503 504 /50x.html;
    
    location = /404.html {
        root /var/www/html;
        internal;
    }
    
    location = /50x.html {
        root /var/www/html;
        internal;
    }
}
```

### Step 7: Test and Reload Nginx

```bash
# Test nginx configuration
sudo nginx -t

# If test passes, reload nginx
sudo systemctl reload nginx

# Check nginx status
sudo systemctl status nginx
```

### Step 8: Create Landing Page

```bash
# Create a simple landing page
sudo nano /var/www/budget-app/index.html
```

Add this content to create a landing page that redirects to the budget app:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Nginx Reverse Proxy Demo</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 0; padding: 20px; background: #f5f5f5; }
        .container { max-width: 800px; margin: 0 auto; background: white; padding: 40px; border-radius: 10px; box-shadow: 0 2px 10px rgba(0,0,0,0.1); }
        h1 { color: #333; text-align: center; margin-bottom: 30px; }
        .apps { display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 20px; }
        .app-card { padding: 20px; border: 2px solid #e0e0e0; border-radius: 8px; text-align: center; transition: all 0.3s ease; }
        .app-card:hover { border-color: #007bff; transform: translateY(-2px); }
        .app-card h2 { color: #007bff; margin-bottom: 10px; }
        .app-card p { color: #666; margin-bottom: 20px; }
        .btn { display: inline-block; padding: 12px 24px; background: #007bff; color: white; text-decoration: none; border-radius: 5px; transition: background 0.3s ease; }
        .btn:hover { background: #0056b3; }
        .health { margin-top: 30px; padding: 20px; background: #d4edda; border: 1px solid #c3e6cb; border-radius: 5px; }
    </style>
</head>
<body>
    <div class="container">
        <h1>🏠 Nginx Reverse Proxy Demo</h1>
        <p style="text-align: center; color: #666; margin-bottom: 40px;">Welcome to the nginx reverse proxy demonstration. Choose an application to explore:</p>
        <div class="apps">
            <div class="app-card">
                <h2>💰 Budget Tracker</h2>
                <p>A comprehensive budget management application for tracking income and expenses.</p>
                <a href="/budget" class="btn">Open Budget App</a>
            </div>
            <div class="app-card">
                <h2>🏠 Home Roster</h2>
                <p>A household chore management system for organizing tasks between occupants.</p>
                <a href="/roster" class="btn">Open Roster App</a>
            </div>
        </div>
        <div class="health">
            <h3>✅ System Status</h3>
            <p>Nginx reverse proxy is running successfully. All applications are accessible through their respective paths.</p>
            <p><strong>Available endpoints:</strong></p>
            <ul>
                <li><code>/</code> - This landing page</li>
                <li><code>/budget</code> - Budget Tracker application</li>
                <li><code>/roster</code> - Home Roster application</li>
                <li><code>/health</code> - Health check endpoint</li>
            </ul>
        </div>
    </div>
</body>
</html>
```

### Step 9: Verify Installation

```bash
# Check if nginx is running
sudo systemctl status nginx

# Test the health endpoint
curl http://localhost/health

# Test the applications
curl http://localhost/budget
curl http://localhost/roster
```

### Step 10: Access Your Applications

Once everything is set up, you can access your applications at:

- **Main Landing Page**: `http://35.183.32.162/`
- **Budget Tracker**: `http://35.183.32.162/budget`
- **Home Roster**: `http://35.183.32.162/roster`
- **Health Check**: `http://35.183.32.162/health`

## Optional: SSL/HTTPS Configuration

To add SSL/HTTPS support, you'll need SSL certificates. Here's how to set it up with Let's Encrypt:

### Install Certbot

```bash
# Install snapd
sudo apt install -y snapd

# Install certbot
sudo snap install --classic certbot

# Create symlink
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

### Obtain SSL Certificate

```bash
# Get SSL certificate
sudo certbot --nginx -d 35.183.32.162 -d ec2-35-183-32-162.ca-central-1.compute.amazonaws.com

# Test automatic renewal
sudo certbot renew --dry-run
```

## Troubleshooting

### Common Issues

1. **Nginx won't start**
   ```bash
   # Check nginx configuration
   sudo nginx -t
   
   # Check nginx logs
   sudo journalctl -u nginx
   ```

2. **403 Forbidden Error**
   ```bash
   # Check file permissions
   sudo chown -R www-data:www-data /var/www/
   sudo chmod -R 755 /var/www/
   ```

3. **502 Bad Gateway**
   ```bash
   # Check if nginx is running
   sudo systemctl status nginx
   
   # Check nginx error logs
   sudo tail -f /var/log/nginx/error.log
   ```

4. **Can't access from external IP**
   ```bash
   # Check firewall status
   sudo ufw status
   
   # Check if port 80 is open
   sudo netstat -tlnp | grep :80
   ```

### Useful Commands

```bash
# Restart nginx
sudo systemctl restart nginx

# Reload nginx configuration
sudo systemctl reload nginx

# Check nginx configuration
sudo nginx -t

# View nginx access logs
sudo tail -f /var/log/nginx/access.log

# View nginx error logs
sudo tail -f /var/log/nginx/error.log

# Check nginx status
sudo systemctl status nginx
```

## Security Considerations

1. **Keep system updated**: Regularly update Ubuntu and nginx
2. **Configure firewall**: Only open necessary ports
3. **Use HTTPS**: Implement SSL/TLS for production
4. **Regular backups**: Backup your configuration and data
5. **Monitor logs**: Regularly check nginx logs for issues

## Performance Optimization

1. **Enable gzip compression**: Already configured in the nginx config
2. **Set up caching**: Static files are cached for 1 year
3. **Use CDN**: Consider using a CDN for static assets
4. **Monitor performance**: Use tools like `htop` and `nginx-status`

## Next Steps

1. **Add more applications**: Extend the reverse proxy for additional services
2. **Implement load balancing**: Add multiple backend servers
3. **Set up monitoring**: Use tools like Prometheus and Grafana
4. **Add authentication**: Implement basic auth or OAuth
5. **Database integration**: Connect applications to databases

## Support

For issues or questions:
1. Check nginx logs: `/var/log/nginx/`
2. Verify configuration: `sudo nginx -t`
3. Check system resources: `htop`, `df -h`
4. Review firewall settings: `sudo ufw status`

---

**Note**: Remember to replace the IP address `35.183.32.162` and DNS name `ec2-35-183-32-162.ca-central-1.compute.amazonaws.com` with your actual server's IP address and domain name.
