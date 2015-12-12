Dockerised Drupal container using PHP 7.0 on Ubuntu 14.04 and configured with Drupal tools.

For example of how to use this container, see [docker-drupal-project-example](https://github.com/andrewholgate/docker-drupal-project-example)

# Installed

- PHP 7.0.x with production settings.
- [HTTP/2](https://en.wikipedia.org/wiki/HTTP/2)
- Apache 2.4 with PHP-FPM and Prefork MPM configured for HTTP & HTTPS and with minimal modules installed.
- MySQL 5.6 client
- [Linux troubleshooting tools](http://www.linuxjournal.com/magazine/hack-and-linux-troubleshooting-part-i-high-load)
- [git](http://git-scm.com/) (latest version)
- [Composer](https://getcomposer.org/) - PHP dependency management.
- Rsyslog and common log directory
- Guest user (`ubuntu`)

# Installation

## Create Presistant Database data-only container

```bash
# Build database image based off MySQL 5.6
sudo docker run -d --name mysql-drupal-php70 mysql:5.6 --entrypoint /bin/echo MySQL data-only container for Drupal PHP 7.0 MySQL
```

## Build Drupal Base Image

```bash
# Clone Drupal docker repository
git clone https://github.com/andrewholgate/docker-drupal-php70.git
cd docker-drupal-php70

# Build docker image
sudo docker build --rm=true --tag="drupal-php70" . | tee ./build.log
```

## Build Project using Docker Compose

```bash
# Customise docker-compose.yml configurations for environment.
cp docker-compose.yml.dist docker-compose.yml
vim docker-compose.yml

# Build docker containers using Docker Compose.
sudo docker-compose build
sudo docker-compose up -d
```

## Host Access

From the host server, add the web container IP address to the hosts file.

```bash
# Add IP address to hosts file.
./hosts.sh
```

## Logging into Web Front-end

```bash
# Using the container name of the web frontend.
sudo docker exec -it dockerdrupalphp70_drupalphp70web_1 su - ubuntu
```

# TODO

- Switch to use Unix socket for FPM
- Install upload progress Apache module
- Add [Twig C extension](http://twig.sensiolabs.org/doc/installation.html) when [compatible with PHP7](https://github.com/twigphp/Twig/issues/1935#issuecomment-162519975)
