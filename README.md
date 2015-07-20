# Reverse-Proxy-for-Google
[Nginx] setup Nginx as a reverse proxy for Google

#### Configuration
```nginx
server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass https://www.google.com;
        proxy_set_header Host www.google.com;
        proxy_set_header Referer https://www.google.com;

        proxy_set_header User-Agent $http_user_agent;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header Accept-Encoding "";
        proxy_set_header Accept-Language $http_accept_language;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

        sub_filter google.com example.com;
        sub_filter_once off;
    }
}
```
OK, type in your domain name (such as example.com) on the browser, you can access Google now.   
Keep on going
<br>
#### Using SSL
```nginx
server {
    listen 80;
    server_name example.com;
    location / {
        rewrite ^/(.*)$ https://$host$1 permanent;
    }
}

server {
    listen 443 ssl;
    server_name example.com;

    ssl_certificate      /etc/ssl/example.crt;
    ssl_certificate_key  /etc/ssl/example.key;

    ssl_session_cache    shared:SSL:1m;
    ssl_session_timeout  5m;

    location / {
        proxy_pass https://www.google.com;
        proxy_set_header Host www.google.com;
        proxy_set_header Referer https://www.google.com;

        proxy_set_header User-Agent $http_user_agent;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header Accept-Encoding "";
        proxy_set_header Accept-Language $http_accept_language;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

        sub_filter google.com example.com;
        sub_filter_once off;
    }
}
```
