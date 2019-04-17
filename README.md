## Generate and push image

```
docker login
docker build -t pokyunn/php:tagname .
docker push pokyunn/php:tagname

ex:

docker build -t pokyunn/php:7.2-fpm-alpine ./7.2/alpine/fpm
docker push pokyunn/php:7.2-fpm-alpine
```

## Cleanup local 

```
docker container prune -f
docker image prune -af
```