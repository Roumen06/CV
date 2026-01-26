# Deploying to Raspberry Pi with Nginx

This guide will help you deploy your CV/portfolio website on a Raspberry Pi using Nginx as the web server.

## Prerequisites

- Raspberry Pi (preferably Pi 4) with Raspberry Pi OS installed
- SSH access to your Raspberry Pi
- Basic command line knowledge
- Domain name (optional, but recommended)

## Step 1: Update Your Raspberry Pi

```bash
sudo apt update
sudo apt upgrade -y
```

## Step 2: Install Nginx

```bash
sudo apt install nginx -y
```

Verify Nginx is running:
```bash
sudo systemctl status nginx
```

## Step 3: Configure Firewall (if using UFW)

```bash
sudo apt install ufw -y
sudo ufw allow 'Nginx Full'
sudo ufw allow OpenSSH
sudo ufw enable
```

## Step 4: Create Website Directory

```bash
sudo mkdir -p /var/www/cv-portfolio
sudo chown -R $USER:$USER /var/www/cv-portfolio
sudo chmod -R 755 /var/www/cv-portfolio
```

## Step 5: Upload Website Files

Transfer your website files to the Raspberry Pi using SCP or rsync:

### Using SCP (from your local machine):
```bash
scp -r /path/to/cv-portfolio/* pi@raspberrypi.local:/var/www/cv-portfolio/
```

### Using rsync (from your local machine):
```bash
rsync -avz --progress /path/to/cv-portfolio/* pi@raspberrypi.local:/var/www/cv-portfolio/
```

## Step 6: Configure Nginx

Create a new Nginx configuration file:

```bash
sudo nano /etc/nginx/sites-available/cv-portfolio
```

Add the following configuration:

```nginx
server {
    listen 80;
    listen [::]:80;
    
    # Replace with your domain name or IP address
    server_name your-domain.com www.your-domain.com;
    
    root /var/www/cv-portfolio;
    index index.html;
    
    # Enable gzip compression
    gzip on;
    gzip_vary on;
    gzip_min_length 1024;
    gzip_types text/plain text/css text/xml text/javascript application/javascript application/xml+rss application/json;
    
    location / {
        try_files $uri $uri/ =404;
    }
    
    # Cache static assets
    location ~* \.(jpg|jpeg|png|gif|ico|css|js|svg|woff|woff2|ttf|eot)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }
    
    # Security headers
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-XSS-Protection "1; mode=block" always;
    add_header Referrer-Policy "no-referrer-when-downgrade" always;
    
    # Deny access to hidden files
    location ~ /\. {
        deny all;
    }
}
```

## Step 7: Enable the Site

```bash
sudo ln -s /etc/nginx/sites-available/cv-portfolio /etc/nginx/sites-enabled/
sudo rm /etc/nginx/sites-enabled/default  # Remove default site (optional)
```

Test the Nginx configuration:
```bash
sudo nginx -t
```

If the test is successful, reload Nginx:
```bash
sudo systemctl reload nginx
```

## Step 8: Configure Domain (Optional)

If you have a domain name:

1. **Set up Dynamic DNS** (if your IP is dynamic):
   - Use a service like No-IP, DuckDNS, or Cloudflare
   - Install ddclient on your Pi for automatic updates

2. **Port Forwarding**:
   - Log into your router
   - Forward port 80 (HTTP) and 443 (HTTPS) to your Raspberry Pi's local IP
   - Find your Pi's IP: `hostname -I`

## Step 9: Set Up SSL with Let's Encrypt (Recommended)

```bash
sudo apt install certbot python3-certbot-nginx -y
```

Obtain and install SSL certificate:
```bash
sudo certbot --nginx -d your-domain.com -d www.your-domain.com
```

Follow the prompts and choose to redirect HTTP to HTTPS.

Certbot will automatically renew certificates. Test renewal:
```bash
sudo certbot renew --dry-run
```

## Step 10: Set Up Automatic Updates (Optional)

Create a deployment script:

```bash
nano ~/deploy-cv.sh
```

Add:
```bash
#!/bin/bash
cd /var/www/cv-portfolio
# Backup current version
sudo cp -r /var/www/cv-portfolio /var/www/cv-portfolio-backup-$(date +%Y%m%d)
# Pull new changes (if using git)
git pull origin main
# Or sync from local machine
# Reload Nginx
sudo systemctl reload nginx
echo "Deployment completed successfully!"
```

Make it executable:
```bash
chmod +x ~/deploy-cv.sh
```

## Monitoring and Maintenance

### Check Nginx logs:
```bash
sudo tail -f /var/log/nginx/access.log
sudo tail -f /var/log/nginx/error.log
```

### Check Nginx status:
```bash
sudo systemctl status nginx
```

### Restart Nginx if needed:
```bash
sudo systemctl restart nginx
```

### Monitor Pi resources:
```bash
htop
```

## Performance Optimization

### Enable HTTP/2 (after SSL setup):

Edit your Nginx config:
```bash
sudo nano /etc/nginx/sites-available/cv-portfolio
```

Change the SSL listen directive:
```nginx
listen 443 ssl http2;
listen [::]:443 ssl http2;
```

Reload Nginx:
```bash
sudo systemctl reload nginx
```

## Troubleshooting

### Site not accessible:
- Check Nginx status: `sudo systemctl status nginx`
- Check firewall: `sudo ufw status`
- Verify port forwarding on router
- Check file permissions: `ls -la /var/www/cv-portfolio`

### 403 Forbidden error:
- Check file ownership: `sudo chown -R www-data:www-data /var/www/cv-portfolio`
- Check permissions: `sudo chmod -R 755 /var/www/cv-portfolio`

### SSL certificate issues:
- Renew certificate: `sudo certbot renew`
- Check certificate status: `sudo certbot certificates`

## Additional Security

### Install Fail2Ban to prevent brute force attacks:
```bash
sudo apt install fail2ban -y
sudo systemctl enable fail2ban
sudo systemctl start fail2ban
```

### Regular updates:
Set up automatic security updates:
```bash
sudo apt install unattended-upgrades -y
sudo dpkg-reconfigure --priority=low unattended-upgrades
```

## Backup Strategy

Create regular backups:
```bash
# Backup website
sudo tar -czf ~/cv-backup-$(date +%Y%m%d).tar.gz /var/www/cv-portfolio

# Backup Nginx config
sudo tar -czf ~/nginx-config-backup-$(date +%Y%m%d).tar.gz /etc/nginx
```

## Remote Access

### Set up SSH key authentication:
```bash
# On your local machine
ssh-keygen -t rsa -b 4096
ssh-copy-id pi@raspberrypi.local
```

### Disable password authentication (after key setup):
```bash
sudo nano /etc/ssh/sshd_config
```
Set: `PasswordAuthentication no`
```bash
sudo systemctl restart sshd
```

Your CV website is now live on your Raspberry Pi! 🎉
