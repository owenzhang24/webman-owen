
# workerman开发脚手架
webman-owen
- Gitee链接:
  https://gitee.com/owenzhang24/owen-workerman


# 项目内容

- app端时间随机数签名,后台jwt-token验证,接口限流
- 登录/支付(苹果,微信,支付宝,QQ,手机短信,手机一键登录,微博),
- redis,redis-queue,
- topthink参数验证,
- 阿里云腾讯云OSS上传,
- mongodb,
- 定时任务crontab,
- wss,websocket+SSL,
- cache,
- aes加密,
- 监控系统TransferStatistics,
- 多应用nginx配置
- 后期添加-ElasticSearch，RabbitMQ


## 感谢

1. workerman 链接：https://www.workerman.net/
2. wolfcode 链接：https://gitee.com/wolf18
3. hsk99 链接：https://github.com/hsk99


## 服务器

1. php8.1
2. redis6.0
3. mysql8.0
4. mangodb5.0
5. env  https://cloud.tencent.com/developer/article/2060142
6. 内核优化 https://cloud.tencent.com/developer/article/2060138
7. 多项目nginx配置  https://my.oschina.net/owenzhang24/blog/5585429
8. 本地window开发运行php start.php start即可，无需配置nginx，另外解压vendor.zip文件即可
9. composer update注意，阿里云包（alibabacloud，alipaysdk，aliyuncs等等）可能对PHP81不兼容，那就复制vendor.zip文件的对应旧包


## 本项目采用php8.1版本，cli模式运行

请使用 php start.php (restart | start | stop) 命令进行控制 守护模式 -d

php start.php start       #启动服务 测试环境用

php start.php start -d    #启动服务 守护模式 正式环境用

php start.php reload      #重载代码


## nginx
```
server
{
    listen 80;
	listen 443 ssl;
    server_name api.OwenWeb.com;
    index index.php index.html index.htm default.php default.htm default.html;
    root /www/wwwroot/owenweb-api/public;

    #SSL-START SSL相关配置，请勿删除或修改下一行带注释的404规则
    #error_page 404/404.html;
    ssl_certificate    /www/server/panel/vhost/cert/api.OwenWeb.com/fullchain.pem;
    ssl_certificate_key    /www/server/panel/vhost/cert/api.OwenWeb.com/privkey.pem;
    ssl_protocols TLSv1.1 TLSv1.2 TLSv1.3;
    ssl_ciphers EECDH+CHACHA20:EECDH+CHACHA20-draft:EECDH+AES128:RSA+AES128:EECDH+AES256:RSA+AES256:EECDH+3DES:RSA+3DES:!MD5;
    ssl_prefer_server_ciphers on;
    ssl_session_cache shared:SSL:10m;
    ssl_session_timeout 10m;
    add_header Strict-Transport-Security "max-age=31536000";
    error_page 497  https://$host$request_uri;

    #SSL-END

    #ERROR-PAGE-START  错误页配置，可以注释、删除或修改
    #error_page 404 /404.html;
    #error_page 502 /502.html;
    #ERROR-PAGE-END

    #PHP-INFO-START  PHP引用配置，可以注释或修改
    include enable-php-00.conf;
    #PHP-INFO-END

    #REWRITE-START URL重写规则引用,修改后将导致面板设置的伪静态规则失效
    include /www/server/panel/vhost/rewrite/api.OwenWeb.com.conf;
    #REWRITE-END

    #禁止访问的文件或目录
    location ~ ^/(\.user.ini|\.htaccess|\.git|\.svn|\.project|LICENSE|README.md)
    {
        return 404;
    }

    #一键申请SSL证书验证目录相关设置
    location ~ \.well-known{
        allow all;
    }

    location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$
    {
        expires      30d;
        error_log /dev/null;
        access_log /dev/null;
    }

    location ~ .*\.(js|css)?$
    {
        expires      12h;
        error_log /dev/null;
        access_log /dev/null;
    }
    
    location / {
        if ($request_method = 'OPTIONS') {  
            add_header 'Access-Control-Allow-Origin' $http_origin;  
            add_header 'Access-Control-Allow-Credentials' 'true';  
            add_header 'Access-Control-Allow-Methods' 'GET, POST, PUT, DELETE, PATCH, OPTIONS';  
            add_header 'Access-Control-Allow-Headers' 'DNT,X-Mx-ReqToken,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Authorization,Authorization-admin,token,AppVersion';  
            return 200;
        }
        
        if ($request_method = 'POST') {  
            add_header 'Access-Control-Allow-Origin' $http_origin;   
            add_header 'Access-Control-Allow-Credentials' 'true';  
            add_header 'Access-Control-Allow-Methods' 'GET, POST, PUT, DELETE, PATCH, OPTIONS';  
            add_header 'Access-Control-Allow-Headers' 'DNT,X-Mx-ReqToken,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Authorization,Authorization-admin,token,AppVersion';  
        }      
        proxy_pass http://127.0.0.1:3571;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header Host $http_host;
    }
    
    access_log  /www/wwwlogs/api.OwenWeb.com.log;
    error_log  /www/wwwlogs/api.OwenWeb.com.error.log;
}
```

# Manual

https://www.workerman.net/doc/webman

# Benchmarks

https://www.techempower.com/benchmarks/#section=test&runid=9716e3cd-9e53-433c-b6c5-d2c48c9593c1&hw=ph&test=db&l=zg24n3-1r&a=2
![image](https://user-images.githubusercontent.com/6073368/96447814-120fc980-1245-11eb-938d-6ea408716c72.png)

## Contact

E-mail:owen@owenzhang.com

## LICENSE

MIT
