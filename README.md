# Docker-Mssql-Laravel

## Install

### Docker
software containerization platform

Install [https://download.docker.com/win/stable/InstallDocker.msi](https://download.docker.com/win/stable/InstallDocker.msi)

### Kitematic
an installer to quickly and easily install and setup a Docker environment

Install [https://github.com/docker/toolbox/releases/tag/v1.12.5](https://github.com/docker/toolbox/releases/tag/v1.12.5)

### Bash for windows 10 (Not recommend)
This is an option. See [http://www.windowscentral.com/how-install-bash-shell-command-line-windows-10](http://www.windowscentral.com/how-install-bash-shell-command-line-windows-10)

### Cmder (Recommend instead of bash for windows)
Download cmder included git. See [http://cmder.net/](http://cmder.net/)

## Run Docker

**Mac user should comment out mssql volumes**

`vi docker/docker-compose.yml`

```
...
mssql:
    build:
        context: mssql
    ports:
        - "11433:1433"
#    volumes:
#        - ./mssql/data:/var/opt/mssql/data
```


```
cd docker
docker-compose up -d
```

## Init Database

Connect local MSSQL Server and create database using mssql clients.

* [http://dbeaver.jkiss.org/](http://dbeaver.jkiss.org/)
* [https://msdn.microsoft.com/en-us/library/mt238290.aspx](https://msdn.microsoft.com/en-us/library/mt238290.aspx)
* [http://www.heidisql.com/](http://www.heidisql.com/)

**Administrator Account**

* ID: sa
* Password: Password1234

**Sample Init**

```
CREATE DATABASE testdb;
USE testuser;
CREATE LOGIN testuser WITH PASSWORD = 'TestUser1234';
CREATE USER testuser FOR LOGIN testdb;
ALTER SERVER ROLE sysadmin ADD MEMBER [testuser];
```


## Laravel


### Host

**Clone Repository**

```
git clone https://github.com/harryoh/docker-mssql-laravel.git
```

**Connect to docker**

```
cd docker-mssql-laravel/docker
docker exec -it docker_php_1 /bin/bash
```

### Container

**Configure environment for Laravel**

copy example env file to .env

```
cd www
cp .env.example .env
vi .env
```

change database configuration

```
...
DB_CONNECTION=sqlsrv
DB_HOST=mssql
DB_PORT=11433
DB_DATABASE=testdb
DB_USERNAME=testuser
DB_PASSWORD=TestUser1234
...
```

**Generate Key**

```
php artisan key:generate
```

**Install third party libraries**

```
$ composer install
```

**Database Migration**

```
$ php artisan migrate
```

### Browser

(docker-compose should be running.)

Open 'http://localhost:8080'