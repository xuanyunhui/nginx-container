Nginx 1.10 server and a reverse proxy server docker image
=========================================================

The `centos/rh-nginx-110-centos7` image provides an nginx 1.10 server and a reverse proxy server. The image can be used as a base image for other applications based on nginx 1.10 web server.


To pull the `centos/rh-nginx-110-centos7` image, run the following command as root:
```
docker pull centos/rh-nginx-110-centos7
```

S2I build support
-------------
Nginx server image can be extended using S2I tool.
S2I build folder structure:

|    Folder name              |    Description                            |
| :-------------------------- | ----------------------------------------- |
|  ./nginx-cfg/*.conf         | Should contain all nginx configuration we want to include into image |
|  ./nginx-default-cfg/*.conf | Contains any nginx config snippets to include in the default server block |
|  ./                         | Should contain nginx application source code                         |

Configuration
-------------
The nginx container image supports the following configuration variable, which can be set by using the `-e` option with the docker run command:


|    Variable name       |    Description                            |
| :--------------------- | ----------------------------------------- |
|  `NGINX_LOG_TO_VOLUME` | By default, nginx logs into standard output, so the logs are accessible by using the docker logs command. When `NGINX_LOG_TO_VOLUME` is set, nginx logs into `/var/opt/rh/rh-nginx110/log/nginx/`, which can be mounted to host system using the Docker volumes. |
