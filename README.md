## Installation

Run - ~~~yml
git clone https://github.com/bagisto/bagisto-docker.git .
~~~

## Docker Compose

- Adjust your Apache, MySQL and PHPMyAdmin port.

  ~~~yml
  version: '3.1'

  services:
      bagisto-php-apache:
          build:
              args:
                  container_project_path: /var/www/html/
                  uid: 1000 # add your uid here
                  user: $USER
              context: .
              dockerfile: ./Dockerfile
          image: bagisto-php-apache
          ports:
              - 80:80 # adjust your port here, if you want to change
          volumes:
              - ./workspace/:/var/www/html/

      bagisto-mysql:
          image: mysql:8.0
          command: --default-authentication-plugin=mysql_native_password
          restart: always
          environment:
              MYSQL_ROOT_HOST: '%'
              MYSQL_ROOT_PASSWORD: root
          ports:
              - 3306:3306 # adjust your port here, if you want to change
          volumes:
              - ./.configs/mysql-data:/var/lib/mysql/

      bagisto-phpmyadmin:
          image: phpmyadmin:latest
          restart: always
          environment:
              PMA_HOST: bagisto-mysql
              PMA_USER: root
              PMA_PASSWORD: root
          ports:
              - 8080:80 # adjust your port here, if you want to change

  volumes:
      mysql-data:
  ~~~

- Run the below command and everything setup for you,

  ~~~sh
  sh setup.sh
  ~~~
