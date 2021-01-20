# + chromedriver + selenium 

sudo yum update -y

curl -sL https://rpm.nodesource.com/setup_12.x | sudo bash -

sudo yum install -y nodejs

sudo npm install -g pm2

sudo yum install ./google-chrome-stable_current_x86_64.rpm

npm install selenium-webdriver chrome chromedriver moment moment-timezone 

sudo yum install epel-release -y
sudo yum install nginx
sudo vi /etc/nginx/nginx.conf



            # For more information on configuration, see:
            #   * Official English Documentation: http://nginx.org/en/docs/
            #   * Official Russian Documentation: http://nginx.org/ru/docs/

            user nginx;
            worker_processes auto;
            error_log /var/log/nginx/error.log;
            pid /run/nginx.pid;

            # Load dynamic modules. See /usr/share/doc/nginx/README.dynamic.
            include /usr/share/nginx/modules/*.conf;

            events {
                worker_connections 1024;
            }

            http {
                log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                                  '$status $body_bytes_sent "$http_referer" '
                                  '"$http_user_agent" "$http_x_forwarded_for"';

                access_log  /var/log/nginx/access.log  main;

                sendfile            on;
                tcp_nopush          on;
                tcp_nodelay         on;
                keepalive_timeout   65;
                types_hash_max_size 2048;

                include             /etc/nginx/mime.types;
                default_type        application/octet-stream;

                # Load modular configuration files from the /etc/nginx/conf.d directory.
                # See http://nginx.org/en/docs/ngx_core_module.html#include
                # for more information.
                include /etc/nginx/conf.d/*.conf;

                server {
                    listen       80 default_server;
                    listen       [::]:80 default_server;
                    server_name  _;
                    root         /home/centos/express_mongo/front/build/;

                    index index.html index.htm
                    # Load configuration files for the default server block.
                    include /etc/nginx/default.d/*.conf;

                    location / {
                            add_header Access-Control-Allow-Origin *;
                            proxy_pass http://127.0.0.1:3001;
                            proxy_http_version 1.1;
                            proxy_set_header Upgrade $http_upgrade;
                            proxy_set_header Connection "Upgrade";
                            # allow socket.io
                            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                            proxy_set_header Host $host;
                            proxy_set_header Origin "";
                    }


                    error_page 404 /404.html;
                    location = /404.html {
                    }

                    error_page 500 502 503 504 /50x.html;
                    location = /50x.html {
                    }
                }
            # Settings for a TLS enabled server.
            #
            #    server {
            #        listen       443 ssl http2 default_server;
            #        listen       [::]:443 ssl http2 default_server;
            #        server_name  _;
            #        root         /usr/share/nginx/html;
            #
            #        ssl_certificate "/etc/pki/nginx/server.crt";
            #        ssl_certificate_key "/etc/pki/nginx/private/server.key";
            #        ssl_session_cache shared:SSL:1m;
            #        ssl_session_timeout  10m;
            #        ssl_ciphers HIGH:!aNULL:!MD5;
            #        ssl_prefer_server_ciphers on;
            #
            #        # Load configuration files for the default server block.
            #        include /etc/nginx/default.d/*.conf;
            #
            #        location / {
            #        }
            #
            #        error_page 404 /404.html;
            #        location = /404.html {
            #        }
            #
            #        error_page 500 502 503 504 /50x.html;
            #        location = /50x.html {
            #        }
            #    }

            }
