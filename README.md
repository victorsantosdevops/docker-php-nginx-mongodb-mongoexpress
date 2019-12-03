# Docker PHP-FPM 7.3 & Nginx 1.16 & MongoDB latest & Mongo Express latest anon Alpine Linux
Example PHP-FPM 7.3 & Nginx 1.16 setup for Docker, build on [Alpine Linux](http://www.alpinelinux.org/).
The image is only +/- 35MB large.

Repository: https://github.com/victorsantosdevops/docker-php-nginx-mongodb-mongoexpress

* Built on the lightweight and secure Alpine Linux distribution
* Very small Docker image size (+/-35MB)
* Uses PHP 7.3 for better performance, lower cpu usage & memory footprint
* Optimized for 100 concurrent users
* Optimized to only use resources when there's traffic (by using PHP-FPM's ondemand PM)
* The servers Nginx, PHP-FPM and supervisord run under a non-privileged user (nobody) to make it more secure
* The logs of all the services are redirected to the output of the Docker container (visible with `docker logs -f <container name>`)
* Follows the KISS principle (Keep It Simple, Stupid) to make it easy to understand and adjust the image to your needs


## Usage

Build the Image:

    sudo docker build -t victorsantosdevops/alpine-nginx-php7-mongo-mongoexpress .

Start the Docker container:

    docker run -p 80:8080 victorsantosdevops/alpine-nginx-php7-mongo-mongoexpress

See the PHP info on http://localhost, or the static html page on http://localhost/test.html

Or mount your own code to be served by PHP-FPM & Nginx
    
    sudo docker run -it -p 80:8080 -v "$(pwd)/src:/var/www/html" -w "/var/www/html" victorsantosdevops/alpine-nginx-php7-mongo-mongoexpress
    
Start MongoDB and Mongo Express using Docker Compose after change configuration in docker-compose.yaml file
    
    docker-compose up

## Configuration
In [config/](config/) you'll find the default configuration files for Nginx, PHP and PHP-FPM.
If you want to extend or customize that you can do so by mounting a configuration file in the correct folder;

Nginx configuration:

    docker run -v "`pwd`/nginx-server.conf:/etc/nginx/conf.d/server.conf" victorsantosdevops/alpine-nginx-php7-mongo-mongoexpress

PHP configuration:

    docker run -v "`pwd`/php-setting.ini:/etc/php7/conf.d/settings.ini" victorsantosdevops/alpine-nginx-php7-mongo-mongoexpress

PHP-FPM configuration:

    docker run -v "`pwd`/php-fpm-settings.conf:/etc/php7/php-fpm.d/server.conf" victorsantosdevops/alpine-nginx-php7-mongo-mongoexpress

_Note; Because `-v` requires an absolute path I've added `pwd` in the example to return the absolute path to the current directory_ 
