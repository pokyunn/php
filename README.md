![Docker Image CI](https://github.com/pokyunn/php/workflows/Docker%20Image%20CI/badge.svg)


## Generate and push image

```
docker login
docker build -t [username]/php:[tag-name] [path]
docker push [username]/php:[tag-name]

ex:

docker build -t pokyunn/php:8.3-fpm-alpine ./8.3/alpine/fpm --no-cache
docker push pokyunn/php:8.3-fpm-alpine
```

## Cleanup local

```
docker container prune -f
docker image prune -af
```

### Cleanup everything
`docker system prune -af`


### Test
```
docker container run --rm -v $(pwd):/app/ pokyunn/php:8.3-fpm-alpine php /app/test.php
docker container run --rm -v $(pwd):/app/ pokyunn/php:8.3-fpm-alpine php -v
```
