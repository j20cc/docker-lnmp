### Docker集成多项目LNMP环境(php7.2-fpm+mysql5.7+nginx+redis+composer)

---

#### - 目录结构
```php
├── README.md
├── docker-compose.yml
├── logs
│  └── default_access.log
├── nginx
│  └── default.conf
├── php-fpm
│  ├── Dockerfile
│  ├── php-ini-overrides.ini
│  └── sources.list     //阿里云apt源
└── www  //项目目录
    └── default
```

#### - 如何使用

1. 确保已经安装`docker`和`docker-compose`
2. `git clone`到本地，`cd /path/to/docker-lnmp`，启动`docker-compose up -d`
3. 启动完毕就可以访问[http://localhost:8000](http://localhost:8000)看到`phpinfo`页面了

#### - 实操新建一个laravel项目

1. `docker-compose ps`查看所有服务是否已启动
2. 进入php-fpm容器`docker-compose exec php-fpm bash`，并新建一个laravel项目
```
app$ cd www && composer create-project --prefer-dist laravel/laravel blog "5.5.*"
```
3. 复制一份nginx配置`cp docker-lnmp/nginx/default.conf docker-lnmp/nginx/blog.test.conf`，并修改
```
server_name blog.test

...

access_log /app/logs/blog_access.log;   //可选
#error_log /app/logs/blog_error.log;    //可选
root /app/www/blog/public;

...

location / {
    try_files $uri $uri/ /index.php?$query_string;
}
```
4. 重启Nginx，`docker-compose exec webserver nginx -s reload`
5. 在本地hosts添加`127.0.0.1    blog.test`，浏览器打开[http://blog.test:8000](http://blog.test:8000)
6. 连接mysql，redis请在`.env`中host为(docker-compose.yml的services的名字)
```
DB_HOST=mysql
REDIS_HOST=redis
```
7. navicate连接mysql
```
host: 127.0.0.1
username: root
password: secret    //在docker-compose.yml中配置的MYSQL_ROOT_PASSWORD=secret
port: 8002
```