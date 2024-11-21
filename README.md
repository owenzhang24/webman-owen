# workerman开发脚手架webman-owen
- Gitee链接:
- https://gitee.com/owenzhang24/webman-owen
- https://www.workerman.net/


# 服务器

1. php8.1
2. redis6.0
3. mysql8.0
4. mangodb5.0
5. env  https://cloud.tencent.com/developer/article/2060142
6. 内核优化 https://cloud.tencent.com/developer/article/2060138
7. 多项目nginx配置  https://my.oschina.net/owenzhang24/blog/5585429
8. 本地window开发运行php start.php start即可，无需配置nginx，composer install即可


# 项目运行步骤
1. 安装数据库，根目录下的owenweb.sql
2. 修改根目录下的.env1改为.env，修改数据库配置
3. 运行 composer install
4. 运行 php start.php start -d
5. sudo chmod 755 runtime
5. webman/admin 后台账号:admin，密码:123456，访问地址http://127.0.0.1:8241/app/admin/
6. 自定义后台管理系统前后端分离，用app/admin来接后台接口，
7. 集成的后台管理系统用webman/admin源代码位于 plugin/admin/app/controller是官方的后台集成好的
8. 接口文档文件，下载apifox，导入根目录下的webman-owen.apifox.json
9. 本地Windows开发，php环境下载phpstudy（https://old.xp.cn/download.html）
10. 根目录php81 for phpstudy.zip解压复制到对应php目录(我的是D:\phpstudy_pro\Extensions\php)，该php81版本已安装对应的redis等php扩展,解压后即可使用,



# 项目内容

- app端时间随机数签名,后台jwt-token验证,接口限流
- 登录(苹果,微信,支付宝,QQ,手机短信,手机一键登录,微博),
- 支付(苹果,微信,支付宝),
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
- webman/admin 后台系统
- gatewayworker IM聊天 简易多人聊天表情包和图片文件（前后端）
- RabbitMQ 使用roiwk/rabbitmq 参考 https://github.com/roiwk/rabbitmq
- 后期添加-ElasticSearch
- ElasticSearch 参考：https://my.oschina.net/owenzhang24/blog/4981998
- https://my.oschina.net/owenzhang24/blog/5268463
- RabbitMQ 参考：https://my.oschina.net/owenzhang24/blog/5401729
- https://my.oschina.net/owenzhang24/blog/5051652


# 其他功能说明
1. webman/admin 后台账号:admin，密码:123456 源代码位于 plugin/admin是官方的后台集成好的，访问地址http://127.0.0.1:8241/app/admin/，运行owenweb.sql，就可以用了，已经安装好了，如果要自己写的后台模块则删除 plugin/admin，用app/admin是前后端分离的后台接口，即可，官方webman/admin接口文档地址https://www.workerman.net/doc/webman-admin/
2. gatewayworker 简易多人聊天表情包和图片文件，服务端：app/gatewayworker，前端v2：plugin/chatweb（效果图在该目录下的README.md） ，启用该功能config/plugin/webman/gateway-worker/process.php注释去除
3. 在linux下运行php start.php start即可，app/gatewayworker代码已经同步到plugin/webman/gateway，和，config/plugin/webman/gateway-worker，运用webman/gatewayworker
4. 在window下可以使用根目录下gatewayworker_start_for_win.bat来运行，然后代码写在app/gatewayworker，调试成功后，代码在同步到3
5. 可以忽略4（删除gatewayworker_start_for_win.bat，和，app/gatewayworker），直接在3进行聊天gatewayworker操作


# 感谢

- workerman 链接：https://www.workerman.net/
- wolfcode 链接：https://gitee.com/wolf18
- hsk99 链接：https://github.com/hsk99
- qice IM通讯gatewayworker 链接：https://gitee.com/qice/QchatServer
- qice IM通讯VUE 链接：https://gitee.com/qice/qchat


# 联系方式

- 有任何问题可以联系我邮箱，提交Issues不及时看，邮箱时刻在线
- E-mail:owen@owenzhang.com


# 本项目采用php8.1版本，cli模式运行

请使用 php start.php (restart | start | stop) 命令进行控制 守护模式 -d

php start.php stop        #停止服务 测试环境用

php start.php start       #启动服务 测试环境用

php start.php start -d    #启动服务 守护模式 正式环境用

php start.php reload -d     #重载代码


# Buy me a cup of coffee :)

觉得对你有帮助，就给我打赏吧，谢谢！

[微信赞赏码链接，点击跳转:](https://www.owenzhang.com/wechat_reward.png)

![微信赞赏码.png](微信赞赏码.png)

# nginx
//外网访问项目地址:api.owenweb.com，转发到本地的端口127.0.0.1:8241
```
upstream webman {
    server 127.0.0.1:8241;
    keepalive 10240;
}

# HTTP 配置，负责自动跳转到 HTTPS
server {
    listen 80;
    server_name test.owenweb.com;

    # 自动跳转到 HTTPS
    return 301 https://$host$request_uri;
}

# HTTPS 配置
server
{
	listen 443 ssl;
    server_name test.owenweb.com;
    access_log off;
    root /home/www/owenweb;

    # SSL 配置
    ssl_certificate /etc/ssl/owenweb/fullchain.crt;  # 包含服务器证书和中间证书
    ssl_certificate_key /etc/ssl/owenweb/private.pem;  # 私钥

    # SSL 配置安全选项
    ssl_protocols TLSv1.2 TLSv1.3;  # 启用 TLS 1.2 和 TLS 1.3
    ssl_ciphers 'TLS_AES_128_GCM_SHA256:TLS_AES_256_GCM_SHA384:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384';
    ssl_prefer_server_ciphers off;
    
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
    
    location ^~ / {
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

        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header Host $http_host;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_http_version 1.1;
        proxy_set_header Connection "";
        proxy_buffering on; # 启用缓冲提高性能

        if (!-f $request_filename){
            proxy_pass http://webman;
        }        
    }
    
   location /wss
   {
       proxy_pass http://127.0.0.1:2348;
       proxy_set_header X-Real-IP $remote_addr;
       proxy_set_header Host $host;
       proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
       proxy_http_version 1.1;
       proxy_set_header Upgrade $http_upgrade;
       proxy_set_header Connection "upgrade";
       rewrite /wss/(.*) /$1 break;
       proxy_redirect off;
   } 
   
}

```

# ip+端口访问
//外网访问项目地址:110.14.109.123:8331，转发到本地的端口127.0.0.1:8241
```
server
{
    listen 8331;
    server_name 110.14.109.123:8331;
    root /www/wwwroot/webman-owen;
    access_log off;
    
    location ^~ / {
        proxy_pass http://127.0.0.1:8241;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header Host $http_host;
    }
}

#或者
server
{
	listen 80;
	listen [::]:80;
    server_name paytest.owenweb.com;
    root /home/www/pay;
    access_log off;
    
    location ^~ / {
        proxy_pass http://127.0.0.1:8241;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header Host $http_host;
    }
}
```

