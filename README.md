# Laravel Lite in Docker

- [Xdebug](#xdebug)
- [Nginx path & laravel index](#nginx-path--laravel-index)

## Xdebug

Setup `client_host` in [docker/app/conf.d/xdebug.ini](docker/app/conf.d/xdebug.ini) for your docker0 gateway

```ini
# linux
xdebug.client_host=172.17.0.1

# macos
xdebug.client_host=host.docker.internal
```

Setup Xdebug port in **both** [.vscode/launch.json](.vscode/launch.json) and `xdebug.client_port` in [docker/app/conf.d/xdebug.ini](docker/app/conf.d/xdebug.ini)

## Nginx path & laravel index

- [docker-compose.yml](docker-compose.yml)

  ```yaml
  services:
    app:
      volumes:
        - ./src:/var/www
    nginx:
      volumes:
        - ./src:/var/www
  ```

- [docker/nginx/nginx.conf](docker/nginx/nginx.conf)

  ```nginx
  http {
    server {
      index index.html index.php;
      root /var/www/public;

      location / {
        try_files $uri $uri/ /index.php$is_args$args;
      }

      location ~* \.php$ {
        fastcgi_index index.php;
      }
    }
  }

  ```

- [.vscode/launch.json](.vscode/launch.json)

  ```jsonc
  {
    "version": "0.2.0",
    "configurations": [
      {
        "name": "Listen for Xdebug",
        "type": "php",
        "request": "launch",
        "port": 9003,
        "pathMappings": {
          "/var/www": "${workspaceFolder}/src"
        }
      },
    ]
  }

  ```
