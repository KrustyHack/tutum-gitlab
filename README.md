# tutum-gitlab

Tutum stackfile for deploying GitLab (https) with Jenkins (support docker jobs with Dind)

[![Deploy to Tutum](https://s.tutum.co/deploy-to-tutum.svg)](https://dashboard.tutum.co/stack/deploy/)

## GitLab 8.0.2

- sameersbn/gitlab:8.0.2
- jpetazzo/dind:latest
- jenkins:latest
- sameersbn/postgresql:9.4
- sameersbn/redis:latest

## Nginx vhosts configurations example

### For GitLab

    server {
        server_name gitlab.mysite.com;
        listen 80;
        rewrite ^ https://$server_name$request_uri? permanent;
    }
    server {
        server_name gitlab.mysite.com;
        listen 443;
        ssl on;
        proxy_ssl_session_reuse off;
        ssl_certificate /etc/nginx/ssl/mysite-chained.crt;
        ssl_certificate_key /etc/nginx/ssl/mysite.key;
        access_log /var/log/nginx/gitlab.mysite.com-access.log;
        error_log /var/log/nginx/gitlab.mysite.com-error.log;
        location / {
            proxy_pass https://myserver:4430;
        }
    }

### For Jenkins

    server {
        server_name jenkins.mysite.com;
        listen 80;
        rewrite ^ https://$server_name$request_uri? permanent;
    }
    server {
        server_name jenkins.mysite.com;
        listen 443;
        ssl on;
        proxy_ssl_session_reuse off;
        ssl_certificate /etc/nginx/ssl/mysite-chained.crt;
        ssl_certificate_key /etc/nginx/ssl/mysite.key;
        access_log /var/log/nginx/jenkins.mysite.com-access.log;
        error_log /var/log/nginx/jenkins.mysite.com-error.log;
        location / {
            proxy_pass http://myserver:8080;
        }
    }