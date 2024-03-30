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
By default, nginx will server `index.html` as entry point. Using `try_files /directory/other.html` we can set alternative.
```conf
location /noIndexHTMLDirectory {
    root /home/siteRootDirectory;
    try_files /noIndexHTMLDirectory/other.html;
}
```

using try_files, multiple fallback option can also be set, and if none succeed, error code can also be thrown

`try_files /first.html /second.html /index.html =404;` here, nginx will try to server whatever exists first, if nothing found, a 404 will be thrown