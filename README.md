# vue seo phantomjs方案

### 参考 https://github.com/lengziyu/vue-seo-phantomjs
## 安装 phantomjs

下载

```
wget https://bitbucket.org/ariya/phantomjs/downloads/phantomjs-2.1.1-linux-x86_64.tar.bz2
```

解压

```
tar -jxvf phantomjs-2.1.1-linux-x86_64.tar.bz2
```

配置环境变量

```
sudo vi /etc/profile
```

文件末尾增加如下内容(替换phantomjs下载解压的路径)

```
export PATH=$PATH:{phantomjs的路径}/phantomjs-2.1.1-linux-x86_64/bin
```

执行下面命令，环境变量生效

```
source /etc/profile
```

在执行下面命令，输出2.1.1，表示安装成功

```
phantomjs -v
```

## 部署

```
# 克隆项目到本地
$ git clone https://github.com/lengziyu/vue-seo-phantomjs.git

# 安装express
$ npm install
```

测试是否可以：

```
$ phantomjs spider.js 'https://www.baidu.com/'
```

打印出一堆html代码就说明成功了。

## 线上部署

请先安装PM2、phantomjs、nodejs，并配置全局环境变量。

运行

```
pm2 start server.js
```
日志

```
pm2 log
```

nginx配置：

```
upstream spider_server {
  server localhost:8081;
}

server {
    listen       80;
    server_name  example.com;
    
    location / {
      proxy_set_header  Host            $host:$proxy_port;
      proxy_set_header  X-Real-IP       $remote_addr;
      proxy_set_header  X-Forwarded-For $proxy_add_x_forwarded_for;

      if ($http_user_agent ~* "Baiduspider|twitterbot|facebookexternalhit|rogerbot|linkedinbot|embedly|quora link preview|showyoubot|outbrain|pinterest|slackbot|vkShare|W3C_Validator|bingbot|Sosospider|Sogou Pic Spider|Googlebot|360Spider") {
        proxy_pass  http://spider_server;
      }
    }
}
```
