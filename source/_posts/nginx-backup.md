---
title: nginx-backup
date: 2021-04-20 10:14:52
tags: nginx
---

#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
	use epoll;
    worker_connections  1024;
}
stream {
    server {
        listen 2096 ;
        proxy_pass 45.137.216.107:30002;
        
    }

}

http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';
    
    #access_log  logs/access.log  main;
    
    sendfile        on;
    #tcp_nopush     on;
    
    #keepalive_timeout  0;
    keepalive_timeout  65;
    
    #gzip  on;
    
    server {
        listen       8081;
        server_name  www.windychild.com;
    	#rewrite ^(.*)$ https://${server_name}$1 permanent;
        #charset koi8-r;
    	root   /usr/local/webserver/nginx/html;
        #access_log  logs/host.access.log  main;


​        

        #error_page  404              /404.html;
    
        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    
        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}
    
        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        location ~ \.php$ {
            root           html;
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
            include        fastcgi_params;
        }
    
        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }





    # HTTPS server
    #
    server {
        listen       4438 ssl http2;
        server_name  www.windychild.com;
#		root   /usr/local/webserver/nginx/html;

        ssl_certificate      cert/fullchain.pem;
        ssl_certificate_key  cert/privkey.pem;
    
        ssl_session_cache    shared:SSL:1m;
        ssl_session_timeout  5m;
    	ssl_protocols  TLSv1.3;
        
    	ssl_ciphers  ECDHE-RSA-AES128-SHA:ECDHE-RSA-AES256-SHA:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-CHACHA20-POLY1305:TLS_AES_128_GCM_SHA256:TLS_AES_256_GCM_SHA384:TLS_CHACHA20_POLY1305_SHA256;
        ssl_prefer_server_ciphers  on;
    
        location / {
            root   /usr/local/webserver/nginx/html;
            index index.php index.html index.htm;
    		try_files $uri $uri/ /index.php index.php;
    		
    		if ($http_upgrade = "websocket") {
    			proxy_pass http://localhost:1443;  
    		}
    		
    		proxy_http_version 1.1;
    		proxy_set_header Upgrade websocket;
    		proxy_set_header Connection upgrade;
    		
        }


​		
​		
		location ~ \.php$ {
	            fastcgi_pass 127.0.0.1:9000;
	            fastcgi_index index.php;
	            fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
	            include fastcgi_params;
	    }


​		


    }


​	
​	
​    

}