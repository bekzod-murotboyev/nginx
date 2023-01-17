# *NGINX*

---

### *1. Create your own package for nginx*
- #### The index package for nginx is `/var/www/html`
- #### We need to create aur own package into `/var/www`
```
sudo mkdir /var/www/project
```
---

### *2. Copy all files from build to `project` package*
```
sudo cp -r <path>/build/* /var/www/project/
```
--

### *3. Change nginx configuration*
```
sudo vim /etc/nginx/sites-available/default
```
- #### *Change them like this:*
<img width="666" alt="old" src="https://user-images.githubusercontent.com/102135015/184506291-6cca37a2-c7af-40a0-925b-9c34b9b88ef6.png">


<img width="582" alt="new" src="https://user-images.githubusercontent.com/102135015/184506298-f9f689c8-07ec-4f8c-97f2-e7c2c542b425.png">

### Nginx SSL configuration for ran port
```
server {
  listen 80;

  server_name YOUR_DOMAIN(bp.uz);
  return 301 https://YOUR_DOMAIN$request_uri;
}
server {
  listen 443 ssl http2;
  server_name YOUR_DOMAIN;
  ssl_certificate /root/bp-bundle.pem; # YOU SHOULD PUT HERE PATH TO BUNDLE CERTIFICATE
  ssl_certificate_key /root/bp-privkey.pem; # YOU SHOULD PUT HERE PATH TO PRIVATE KEY OF CERTIFICATE
  ssl_protocols       TLSv1 TLSv1.1 TLSv1.2 TLSv1.3;
  root PATH_TO_ROOT_DIRECTORY(/home/admin/);

  add_header X-Frame-Options "SAMEORIGIN";
  add_header X-Content-Type-Options "nosniff";

  index index.html index.php;

  charset utf-8;

  location / {
  proxy_pass LOCAL_URL(http://localhost:8080);
  proxy_http_version 1.1;
  proxy_set_header Upgrade $http_upgrade;
  proxy_set_header Connection 'upgrade';
  proxy_set_header Host $host;
  proxy_cache_bypass $http_upgrade;
}

# WEBSOCKET SSL CONFIGURATION
#location /ws {
#    proxy_pass   WEBSOCKET_BASE_URL(http://localhost:8080/ws);
#    proxy_http_version 1.1;
#    proxy_set_header   Upgrade $http_upgrade;
#    proxy_set_header   Connection "upgrade";
#    proxy_set_header   Host $host;
#  }

location = /favicon.ico { access_log off; log_not_found off; }
location = /robots.txt  { access_log off; log_not_found off; }

error_page 404 /index.php;

location ~ /\.(?!well-known).* {
deny all;
}
}
```

### Nginx SSL configuration for built index.html file
```
server {
    listen 80;

    server_name YOUR_DOMAIN(ex: bp.uz);
    return 301 https://YOUR_DOMAIN$request_uri;
}
server {
    listen 443 ssl http2;
    server_name YOUR_DOMAIN;
    ssl_certificate /root/bpnet-bundle.crt; # YOU SHOULD PUT HERE PATH TO BUNDLE CERTIFICATE
    ssl_certificate_key /root/bpnet.key; # YOU SHOULD PUT HERE PATH TO PRIVATE KEY OF CERTIFICATE
    ssl_protocols       TLSv1 TLSv1.1 TLSv1.2 TLSv1.3;
    root PATH_TO_BUILD_DIRECTORY(/home/admin/devrs/build);

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-Content-Type-Options "nosniff";

    index index.html;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```
- After changing just restart nginx and website ready to work!
```
sudo systemctl restart nginx
```
