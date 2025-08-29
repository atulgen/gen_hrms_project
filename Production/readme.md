## Setup for Debian: 


- Installation guide
    - https://docs.frappe.io/framework/user/en/installation#debian--ubuntu


MariaDB 10.6.6+                               (11.3 is recommended on develop)
Python 3.10+
Node 18+
Redis 6                                       (caching and realtime updates)
yarn 1.12+                                    (js dependency manager)
pip 20+                                       (py dependency manager)
wkhtmltopdf (version 0.12.5 with patched qt)  (for pdf generation)
cron                                          (bench's scheduled jobs: automated certificate renewal, scheduled backups)


```bash

sudo apt update

sudo apt install git python-is-python3 python3-dev python3-pip redis-server libmariadb-dev mariadb-server mariadb-client pkg-config

```

## Database


## 2. Bench initialization:

- bench init genhrms-bench --frappe-branch version-15

## 3. Site creation

- https://hrms.gennextit.com/


## 4 App installation:


### ERPNext and HRMS:

- bench get-app erpnext --branch version-15


- bench get-app hrms --branch version-15


- Installing at site: 

    - bench --site hrms.gennextit.com install-app erpnext
    - bench --site hrms.gennextit.com install-app hrms


---


### Accessing site from VPS:


create ngnix site config file at /etc/ngnix/site-available/hrms.gennextit.com

```
# HTTP server - redirects to HTTPS
server {
    listen 80;
    server_name hrms.gennextit.com 82.29.164.162;
    return 301 https://hrms.gennextit.com$request_uri;
}

# HTTPS server - main configuration
server {
    listen 443 ssl http2;
    server_name hrms.gennextit.com;
    
    # SSL Configuration
    ssl_certificate /etc/letsencrypt/live/hrms.gennextit.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/hrms.gennextit.com/privkey.pem;
    
    # SSL Security Settings
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers ECDHE-RSA-AES256-GCM-SHA512:DHE-RSA-AES256-GCM-SHA512:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES256-GCM-SHA384;
    ssl_session_cache shared:SSL:10m;
    ssl_session_timeout 10m;
    ssl_prefer_server_ciphers on;
    
    # Security Headers
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
    add_header X-Frame-Options SAMEORIGIN always;
    add_header X-Content-Type-Options nosniff always;
    add_header X-XSS-Protection "1; mode=block" always;


    client_max_body_size 100M;
    client_body_timeout 120s;
    client_header_timeout 120s;

    
    # Proxy all requests to Frappe
    location / {
        proxy_pass http://127.0.0.1:8002;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        
        # WebSocket support (if Frappe uses WebSockets)
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        
        # Timeout settings
        proxy_connect_timeout 300s;
        proxy_send_timeout 300s;
        proxy_read_timeout 300s;
        
        # Handle large file uploads
        client_max_body_size 100M;
    }

       # Static files (CSS, JS, images)
    location /assets {
        root /home/frappe/genhrms-bench/sites/hrms.gennextit.com/public;
        expires 1y;
        add_header Cache-Control "public";
        try_files $uri =404;
    }

    # For other static files (optional but recommended)
    location /files {
        root /home/frappe/genhrms-bench/sites/hrms.gennextit.com/public;
        expires 1y;
        add_header Cache-Control "public";
        try_files $uri =404;
    }

    # Handle static files (optional optimization)
    location /assets/ {
    root /home/frappe/genhrms-bench/sites;
    expires 1y;
    add_header Cache-Control "public, immutable";
    try_files $uri =404;
    }
  
    # Handle socket.io requests (if Frappe uses real-time features)
    location /socket.io/ {
        proxy_pass http://127.0.0.1:8002;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
    
    # Error and access logs
    error_log /var/log/nginx/frappe_error.log;
    access_log /var/log/nginx/frappe_access.log;
}

```

### Gennerate SSL certificates 

- sudo certbot --nginx -d hrms.gennextit.com

- We need to setup 

    - A Record: hrms.gennextit.com → your-server-ip


- Link to site enabled

```bash
sudo ln -s /etc/nginx/sites-available/hrms.gennextit.com /etc/nginx/sites-enabled/```

```

```
sudo nginx -t
sudo nginx -s reload
```


## Running the bench:

### Initial setup: 

![alt text](image.png)


## 5 Admin 

site administration:
- Create Site admin and password






### 7. Install app in Production 


- bench get-app https://github.com/atulgen/gen_hrms




### Prepare for production

- bench setup production frappe (Not working)

- sudo /home/frappe/.local/bin/bench setup production frappe




### Turning on the server scripts 


- bench set-config -g server_script_enabled 1



