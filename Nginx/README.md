### Context and Directives:
Contexts are blocks followed by `{}` and Directives are like statements ends with `;`. Directives are placed inside Contexts to define its context/scope.

like `http{...}` is a context for handling http request in nginx server. http can host a `server{}` context as well. Here `listen 8080` and `root filepath` are directive, which are statements for the actual config.

```conf
http {
    server {
        listen 8080;
        root /home/siteRootDirectory;
    }
}

events {}
```

Nginx needs to be reloaded once `nginx.conf` get updates by `nginx -S reload`

### Mime types and include directive:
Nginx comes with most used mime type out-of-the-box inside `mime.types` file. `include mime.type` directive is used to include those inside http context. Without this, we need to manually add mime types as `types{}` context inside http context

### Location Block | root & alias:
`location /path {}` context is used to specify ulr request endpoints. The path will be appended as directory in `root` directive specified inside of it

`alias directory/exact-directory` can also be used to omit the appending path
```conf
# ... other configs
http {
    include mime.types;
    server {
        listen 8080
        root /home/siteRootDirectory;

        location /directory {
            root /home/siteRootDirectory; # results as /home/siteRootDirectory/directory
        }

        location /mirror {
            alias /home/siteRootDirectory/directory; # exact directory, /mirror directory will not be appended
        }
    }
}
# ...
```

### Directory entrypoint | index.html vs `try_files` directive:
By default, nginx will server `index.html` as entry point. Using `try_files /directory/other.html` we can override this or set fallback or set http error if nothing match.
```conf
location /noIndexHTMLDirectory {
    root /home/siteRootDirectory;
    try_files /noIndexHTMLDirectory/other.html;
}
```

using try_files, multiple fallback option can also be set, and if none succeed, error code can also be thrown

`try_files /first.html /second.html /index.html =404;` here, nginx will try to server whatever exists first, if nothing found, a 404 will be thrown

### Regular expression 
add `~*` before the path to accept regular expression
```conf

location ~* /count/[0-9] {
    root /site;
    try_files /index.html =404;

}
```


### Custom Domain Mapping In Localhost and Docker Nginx:
1st, set `127.0.0.1 customname.com` as the last entry of the `/etc/hosts` file. 

2nd, either add environment entries of `n` & `n` or set servername in `nginx.conf` file

* example docker-compose for virtual host setup.

```yaml
services:
  web:
    image: nginx:latest
    volumes:
    - ./app:/etc/nginx/templates
    ports:
    - "8080:80"
    environment:
    - NGINX_HOST=customname.com
    - NGINX_PORT=80
```

* example `nginx.conf` config for virtual host. full path is `/usr/local/etc/nginx/nginx.conf`

```conf
server {
    listen 80;
    listen [::]:80;
    server_name customname.com www.customname.com;
    access_log /var/log/nginx/mysite.com.access.log;
    error_log /var/log/nginx/mysite.com.error.log;    
    location / {
        proxy_pass http://127.0.0.1:3000;       
        proxy_http_version 1.1;        
        proxy_set_header Upgrade $http_upgrade;               
        proxy_set_header Connection ‘upgrade’;
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

### Docker Nginx Workflows:
`sudo docker exec -t -i container_name /bin/bash`