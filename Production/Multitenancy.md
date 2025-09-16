Perfect! Let's proceed with the multi-tenancy approach using a single bench instance. Here's how to set it up:

## Step 1: Verify Both Sites Exist in Your Bench

```bash
cd ~/genhrms-bench-16
bench --site list
```

## Step 2: Configure Each Site for Multi-tenancy

### 1. Configure the HRMS site:
```bash
bench --site hrms.gennextit.com set-config host_name hrms.gennextit.com
```

### 2. Configure the Recruiter site:
```bash
bench --site recruiter.gennextit.com set-config host_name recruiter.gennextit.com
```

## Step 3: Create Nginx Configuration

### 1. Create the nginx config file:
```bash
sudo nano /etc/nginx/sites-available/gennextit
```

### 2. Add this configuration:
```nginx
server {
    listen 80;
    server_name hrms.gennextit.com recruiter.gennextit.com;
    
    location / {
        proxy_pass http://127.0.0.1:8003;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header X-Frappe-Site-Name $host;
    }
    
    # For websocket support
    location /socket.io {
        proxy_pass http://127.0.0.1:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

## Step 4: Enable Nginx Configuration

```bash
# Remove default config if exists
sudo rm /etc/nginx/sites-enabled/default

# Enable your config
sudo ln -s /etc/nginx/sites-available/gennextit /etc/nginx/sites-enabled/

# Test configuration
sudo nginx -t

# Restart nginx
sudo systemctl restart nginx
```

## Step 5: Verify Frappe Multi-tenancy Setup

### 1. Check site configs:
```bash
cat sites/hrms.gennextit.com/site_config.json
cat sites/recruiter.gennextit.com/site_config.json
```

### 2. Both should have their respective host_name configured.

## Step 6: Test the Setup

### 1. Test using curl:
```bash
# Test HRMS site
curl -H "Host: hrms.gennextit.com" http://localhost

# Test Recruiter site
curl -H "Host: recruiter.gennextit.com" http://localhost
```

### 2. Test from your local machine (add to hosts file if DNS not set):
```bash
# Add to /etc/hosts on your local machine
your-server-ip hrms.gennextit.com
your-server-ip recruiter.gennextit.com
```

## Step 7: SSL Certificate (Optional but Recommended)

```bash
sudo certbot --nginx -d hrms.gennextit.com -d recruiter.gennextit.com
```

## Important Notes:

1. **Single Port**: Both sites will be served through port 8003
2. **Automatic Routing**: Frappe will automatically detect the hostname and serve the correct site
3. **No Supervisor Changes**: You don't need to change your existing supervisor configuration
4. **Shared Resources**: Both sites share the same workers and Redis instances, but have separate databases

## Troubleshooting:

If you encounter issues, check:

1. **Site exists**: `bench --site list`
2. **Nginx logs**: `sudo tail -f /var/log/nginx/error.log`
3. **Frappe logs**: `tail -f logs/web.log`
4. **Site configs**: Verify `host_name` is set correctly in each site's config

This approach is much cleaner and more efficient than running multiple bench instances on different ports!


## Enable server scripts

```bash
bench set-config -g server_script_enabled 1
```

