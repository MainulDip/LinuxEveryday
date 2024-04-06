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
add `~*` before the path to specify regular expression.
```conf
# accepting and handling /count/Int url path
location ~* /count/[0-9] {
    root /site;
    try_files /index.html =404;

}
```

### Redirect and Rewrite:
`307` is the code for http redirect. From inside of location block, `return 307 /redirect-path` is used
```conf
location /url {
    return 307 /redirect-path;
}
```
Rewrite is used to show other url path's content instead of redirecting
```conf
rewrite ^/number/(\w+) /count/$1; # request on url /number/int will return response from /count/int without redirecting
location ~* /count/[0-9] {
    root /site;
    try_files /index.html =404;
}
```

### Load Balancer | Round Robin:
```conf
http {
    # defining round robin with 4 server instance running in the background
    upstream nameOfTheServer {
        server 123.0.0.1:1111;
        server 123.0.0.1:2222
        server 123.0.0.1:3333;
        server 123.0.0.1:4444;
    }

    server {
        listen 8080;
        location / {
            proxy_pass http://nameOfTheServer/; // utilizing defined round robin
        }
    }
}
```



### Custom Domain Mapping In Localhost and Docker Nginx:
1st, set `127.0.0.1 customname.com` as the last entry of the `/etc/hosts` file. 

2nd, listen on port 80 (80 for http and 443 for https). To serve for multiple domain, add `server_name` directive when configuring nginx's `default.conf`.

* example docker-compose for virtual host setup.

```yaml
services:
  web:
    image: nginx:latest
    container_name: dockng1
    volumes:
    - ./app:/usr/share/nginx/html/ # mount app directory to container's html directory
    - ./default.conf:/etc/nginx/conf.d/default.conf # mounting custom config to override nginx's default config
    ports:
    - "80:80"
```

* example default.conf, which will replace `/etc/nginx/conf.d/default.conf`.

```conf
server {
    server_name www.foobar.com foobar.com;
    location / {
        root /usr/share/nginx/html/;
    }
}

server {
    server_name www.barfoo.com barfoo.com; # if server_name is not specified, only first server will be serverd, both are listning on port 80 and no way to differentiate the request incomming
    location / {
        root /usr/share/nginx/html/;
        try_files /barfoo.html =404; // without `=404` or anyting fallback in try_files directive, nginx will throw error. try_files required to include at leat 2 options  
    }
}
```

### Docker Nginx Workflows:
`sudo docker exec -t -i container_name /bin/bash` to hop into