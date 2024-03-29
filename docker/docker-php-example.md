# Docker PHP Example

#docker #php #docker-example 

I needed to run an old PHP project; The documentation of it suggested an old version of Vagrant with some hacky modifications. I tried that, but it did not work as expected. I also tried the newest version of it, but I encountered similar issues. 
Solving these issues was getting out of scope because this requirement was so a non-technical person could run the project the easiest way possible. By this point, [[docker]] was the obvious option.

I found a pretty easy way to run a PHP project in docker in this article: [Setup a basic Local PHP Development Environment in Docker](https://dev.to/truthseekers/setup-a-basic-local-php-development-environment-in-docker-kod)

The following configuration was the tiniest version of the [[docker-compose]] file that I came up with:

```yaml
version: "3.1"

services:
  php:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - 80:80
    volumes:
      - ./html:/var/www/html/
  db:
    image: mysql
    command: --default-authentication-plugin=mysql_native_password
    restart: always
    ports:
      - 3306:3306
    environment:
      MYSQL_ROOT_PASSWORD: example
      MYSQL_ROOT_HOST: db
      MYSQL_ALLOW_EMPTY_PASSWORD: 1

```

Because I needed to run some commands, I had to create the following [[docker-file]]


```dockerfile
# Docker image with configured apache
FROM php:7.4-apache
# Download the latest yii version and place it in the www directory
ADD https://github.com/yiisoft/yii/releases/download/1.1.27/yii-1.1.27.8f9404.tar.gz /var/www
# Extract and rename the yii framework
RUN cd /var/www && tar -xf yii-1.1.27.8f9404.tar.gz && mv yii-1.1.27.8f9404 yii && rm yii-1.1.27.8f9404.tar.gz
# Install pdo_mysql to allow the project to connect to a database
RUN docker-php-ext-install pdo pdo_mysql
# Allow mod_rewrite
RUN a2enmod rewrite
```


---

## Symphony example

After some digging, I stumbled upon this different repo that I did not have time to check, [Symfony Docker Compose](https://github.com/kasteckis/symfony-docker-compose)

Seems worth giving it a try.
