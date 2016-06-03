# go-template-file-replace

A template file replace tool written golang

# Command-line Options

Output console using ENV

```
export webroot=/usr/share/nginx/html
export port=8080
frep -t nginx.conf.in --stdout
```

Output console using arguments

```
frep -t nginx.conf.in --stdout -e port=8080 -e webroot=/usr/share/nginx/html
```

Output to default file (Remove last template file ext)

```
frep -t nginx.conf.in --overwrite -e port=8080
```

Output to specified file

```
frep -t nginx.conf.in:/etc/nginx.conf --overwrite -e port=8080
```

Output multiple files

```
frep -t nginx.conf.in -t redis.conf.in ...
```

If your file uses `{{` and `}}` as part of it's syntax, you can change the template escape characters using the -delims.

```
frep --delims "<%:%>" ...
```

# Template file

Templates use Golang [text/template](http://golang.org/pkg/text/template/). You can access environment variables within a template

```
PATH = {{ .PATH }}
```

There are a few built in functions as well:

* `default $var $default` - Returns a default value for one that does not exist. `{{ default .Env.VERSION "0.1.2" }}`
* `split $string $sep` - Splits a string into an array using a separator string. Alias for strings.Split. `{{ split .Env.PATH ":" }}`

nginx.conf.in

```
server {
    listen {{.port}} default_server;
    listen [::]:{{.port}} default_server ipv6only=on;

    root {{default .webroot "/usr/share/nginx/html"}};
    index index.html index.htm;

    server_name localhost;

    location / {
      access_log off;
    }
}
```
