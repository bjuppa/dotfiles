# Laravel websites

- Set up [Laravel's scheduled tasks](https://laravel.com/docs/scheduling) to run as the webserver user:
  `sudo crontab -e -u www-data`
- Add these bits to nginx website configuration in `/etc/nginx/sites-available/`:

        # Headers (from Laravel docs)
        add_header X-Frame-Options "SAMEORIGIN";
        add_header X-XSS-Protection "1; mode=block";
        add_header X-Content-Type-Options "nosniff";

        # Index file order (from Laravel docs)
        index index.html index.htm index.php;

        # Charset (from Laravel docs)
        charset utf-8;

        # Use Laravel (from Laravel docs)
        location / {
                try_files $uri $uri/ /index.php?$query_string;
        }

        # "Boring files" (from Severs for Hackers & Laravel docs)
        location = /favicon.ico { log_not_found off; access_log off; }
        location = /robots.txt  { log_not_found off; access_log off; }

        # Error page (from Laravel docs)
        error_page 404 /index.php;

        # Pass PHP scripts to FastCGI server (from Laravel docs)
        location ~ \.php$ {
                fastcgi_pass unix:/var/run/php/php7.3-fpm.sock;
                fastcgi_index index.php;
                fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
                include fastcgi_params;
        }

        # Blocking Access to Files (from Laravel docs & Servers for Hackers)
        location ~ /\.(?!well-known).* {
                deny all;
                access_log off;
                log_not_found off;
        }
