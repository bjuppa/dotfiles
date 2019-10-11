# Laravel websites

- Set up [Laravel's scheduled tasks](https://laravel.com/docs/scheduling) to run as the webserver user: `crontab -e -u www-data`
- Add these bits to nginx website configuration in `/etc/nginx/sites-available/`:

        # Headers (from Laravel docs)
        add_header X-Frame-Options "SAMEORIGIN";
        add_header X-XSS-Protection "1; mode=block";
        add_header X-Content-Type-Options "nosniff";

        # Add index.php to the list if you are using PHP
        index index.php index.html index.htm index.nginx-debian.html;

        # Use Laravel (from Laravel docs)
        location / {
                try_files $uri $uri/ /index.php?$query_string;
        }

        # "Boring files" (from Severs for Hackers)
        location = /favicon.ico { log_not_found off; access_log off; }
        location = /robots.txt  { log_not_found off; access_log off; }

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        location ~ \.php$ {
                include snippets/fastcgi-php.conf;

                # With php7.1-fpm:
                fastcgi_pass unix:/run/php/php7.1-fpm.sock;
        }

        # Blocking Access to Files (from Laravel docs & Servers for Hackers)
        location ~ /\.(?!well-known).* {
                deny all;
                access_log off;
                log_not_found off;
        }
