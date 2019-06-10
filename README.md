# Simple example of a Symfony 4 project in Docker

This repository contains a minimal example of using these together: 

* [PHP](https://www.php.net) 7
* [Composer](https://getcomposer.org)
* [Symfony](https://symfony.com) 4 framework
* [Nginx](https://nginx.org) Web server
* [Docker](https://www.docker.com)

How I set up the Symfony application:
 
First, I created an empty `app` directory, 
the `docker-compose.yml` file 
and everything in the `docker` directory.

I started the Docker containers via PhpStorm (`docker-compose up` would 
work, too).

Then I ran Composer in the PHP Docker container 
(replacing `$PHPCONTAINERID` with the ID shown by `docker ps`):

```
$ docker exec \
  --workdir /opt --user daemon:daemon \
  -it $PHPCONTAINERID \
  composer create-project symfony/skeleton app
```

That’s all it took to set up the latest Symfony skeleton on the latest 
PHP version! 

At that point, I could access my application on `http://localhost:8080/`. 
Or run Symfony’s `console` command:

```
$ docker exec \
  --workdir /opt/app --user daemon:daemon \
  -it $PHPCONTAINERID \
  bin/console about
 -------------------- --------------------------------- 
  Symfony                                               
 -------------------- --------------------------------- 
  Version              4.3.1
  …                            
 -------------------- --------------------------------- 
  PHP                                                   
 -------------------- --------------------------------- 
  Version              7.3.5        
  … 
``` 

The only thing I changed in the Symfony application afterwards was directing Symfony 
logs to STDOUT so I could see them as Docker container output.
To do this, I added [Monolog](https://github.com/Seldaek/monolog/blob/master/README.md)
via Composer:

```
$ docker exec \
  --workdir /opt/app --user daemon:daemon \ 
  -it $PHPCONTAINERID \
  composer require symfony/monolog-bundle
```

… and modified `app/config/packages/dev/monolog.yaml`, replacing

```
path: "%kernel.logs_dir%/%kernel.environment%.log"
```

with:

```
path: "php://stdout"
```

As the next step, you will likely want to 
[create your first page in Symfony](https://symfony.com/doc/current/page_creation.html)!  