### 使用`nginx `减少网站带宽

1. 压缩一些静态文件 (HTML, CSS, and JavaScript Files)

   ```
   gzip on;
   gzip_types application/xml application/json text/css text/javascript application/javascript;
   gzip_vary on;
   gzip_comp_level 6;
   gzip_min_length 500;
   ```

   修改前网站请求的资源大小，时间

   ![](https://img.mupaie.com/20200507160748.png)

   修改配置后

   ![](https://img.mupaie.com/20200507160930.png)

   结论：很明显资源被压缩了，加载时间也变短了

2. 配置头部缓存

   ```shell
   # 设置缓存路径
   proxy_cache_path /tmp/cache levels=1:2 keys_zone=cache_one:100m inactive=1d max_size=10g;
   
   location ~ .*\.(gif|jpg|png|css|js)(.*) { # 添加需要过滤的文件后缀，设置缓存
       proxy_pass http://127.0.0.1:5000; 
       proxy_redirect off; 
       proxy_set_header Host $host;
       proxy_cache cache_one;
       proxy_cache_valid 200 302 24h; # 200，302 状态码 缓存24小时
       proxy_cache_valid 301 30d; # 301 状态码 缓存30天
       proxy_cache_valid any 5m; # 其他 状态码 缓存5分钟
       expires 90d;
       add_header wall "hey!guys!give me a star."; 
   }
   ```

3. 启用 http2 协议支持

   `nginx` 版本要求 1.9.5 以上

   ```
   listen 443 ssl http2;
   ```

   

