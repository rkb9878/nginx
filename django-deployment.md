# CREATE DJANGO PROJECT

  ```
        #!/bin/bash  

      if ! [ -d "env" ];
      then
          echo "Directory DOES NOT exists.!!!!"
          echo "--------Env Creating------------------------"
          echo "python3 -m venv env" | python3 -m venv env
          echo "--------Env Cration Done-------------"
      fi

      source env/bin/activate
      pip install django
      pip install djangorestframework
      pip install django-cors-headers
      pip install python-decouple
      pip install gunicorn

      echo "Enter Django Project Name: "
      read name
      if  [ -d $name ];then
          echo "Folder all ready exits"
      else
          django-admin startproject $name
      fi
      pip freeze>>requirements.txt
      echo "Project ready for Runging"
      echo "Run this following command"
      echo "================================"
      echo "source env/bin/activate"
      echo "cd $name"
      echo "python manage.py runserver 0.0.0.0:8000"
      echo "================================"


  ```

#  CREATE GUNICORN SOCKER FILE FOR THIS PROJECT
  ### sudo vim /etc/systemd/system/<PROJECT_FILE>.socket
  ```
    [Unit]
    Description=gunicorn socket
    [Socket]
    ListenStream=/run/<project_name>.sock
    [Install]
    WantedBy=sockets.target
  ```
# CREATING GUNICORN SERVICES FILE FOR PROJECT
  ### sudo vim /etc/systemd/system/<PROJECT_FILE>.service
  ```
    [Unit]
    Description=gunicorn daemon
    Requires=dep.socket
    After=network.target
    [Service]
    User=rahul
    Group=rahul
    WorkingDirectory=<PROJECT FILE LOCATION>
    ExecStart=/home/rahul/learnDjangoDep/env/bin/gunicorn --workers 3 --bind unix:/run/<project_name>.sock \
        <PROJECT_NAME>.wsgi:application
    [Install]
    WantedBy=multi-user.target
  ```

# NGINX

  ```
    server {
          listen 8000;
          listen [::]:8000;
          server_name <DOMAIN>;
          location / {
                  proxy_set_header Host $http_host;
                  proxy_set_header X-Real-IP $remote_addr;
                  proxy_set_header X-Forwarder-For $proxy_add_x_forwarded_for;
                  proxy_set_header X-Forwarded-Proto $scheme;
                  proxy_pass http://unix:/run/<project_name>.sock;
                  proxy_connect_timeout       300;
                  proxy_send_timeout          300;
                  proxy_read_timeout          300;
                  send_timeout                300;
          }

          location /static/{
                  root <Location of the folder>;
          }

          location /media/{
                  root <location of the folder>;
          }

  }
  ```
  
  # RELOAD ALL SERVICES AND RESTART
    
    - sudo systemctl daemon-reload
    - sudo systemctl restart nginx.service
    - sudo systemctl restart <project_name>



