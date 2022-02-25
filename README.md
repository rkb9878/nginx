# nginx

### for node server or django server

server {
	listen 80 default_server;
	listen [::]:80 default_server;
	server_name <domain>;
	location / {
        	proxy_pass http://127.0.0.1:3001;
		proxy_connect_timeout       300;
		proxy_send_timeout          300;
		proxy_read_timeout          300;
		send_timeout                300;
    	}

}




### for react js or html deployment 



server{
	listen 80;
	listen [::]:80;
	root /home/ubuntu/gazaari-admin/build/;
	index index.html index.html;
	server_name <domain>;
	location / {
                try_files $uri /index.html =404;
        }
}

