# 第四节 辅助命令：反斜杠

符号：\

作用：如果一个命令特别长，那么可以使用反斜杠表示到下一行继续输入

示例：

> ./configure \
> --prefix=/usr/local/nginx \
> --pid-path=/var/run/nginx/nginx.pid \
> --lock-path=/var/lock/nginx.lock \
> --error-log-path=/var/log/nginx/error.log \
> --http-log-path=/var/log/nginx/access.log \
> --with-http_gzip_static_module \
> --http-client-body-temp-path=/var/temp/nginx/client \
> --http-proxy-temp-path=/var/temp/nginx/proxy \
> --http-fastcgi-temp-path=/var/temp/nginx/fastcgi \
> --http-uwsgi-temp-path=/var/temp/nginx/uwsgi \
> --http-scgi-temp-path=/var/temp/nginx/scgi

[上一条](verse04-04-shutdown.html) [回目录](verse04-00-index.html) [下一条](verse04-06-curl.html)