# 名称

ngx_req_status - Request status in nginx

完整配置
```lua
    http {
        req_status_zone server_name $server_name 256k;
        req_status_zone server_addr $server_addr 256k;

        req_status server_name server_addr;

        server {
            location /req-status {
                req_status_show on;

                allow 10.0.0.0/8;
                allow 127.0.0.1;
                deny all;
            }
        }
    }
```
## 配置说明

  #### req_status_zone

    syntax:  req_status_zone zone_name $variable memory_max_size
    context: http

    定义统计类型, 内容及内存使用限制.

    如:
    req_status_zone server_name $server_name 256k;
    req_status_zone server_addr $server_addr 256k;
    req_status server_name server_addr;

    variable 可以是组合, 比如 "$server_name,$server_addr"

#### req_status

    syntax:  req_status zone_name1 [zone_name2]
    context: http, server, location

    为 location 启用定义的 req_status_zone 统计, 外层的定义会被
    内层继承, 除非内层在 req_status 中单独或者在某个 zone_name
    前使用了 @

  #### req_status_show

    syntax:  req_status_show
    context: location

    统计信息页面. 支持以下参数:
    c: 统计信息重置
    l: 数字使用原始格式显示

#### 配置示例

http {
  req_status_zone server_name $server_name 256k; 
  req_status_zone server_addr $server_addr 256k; 
  req_status server_name server_addr; 
}

#### 配置访问地址
```lua
    location /req-status {
        req_status_show on;

        allow 10.0.0.0/8;
        allow 127.0.0.1;
        deny all;
    }
```
## 效果

然后你可以访问如下地址

    
    curl http://127.0.0.1/req-status

输出结果是纯文本信息，如：

    zone_name       key     max_active      max_bw  traffic requests        active  bandwidth
    imgstore_appid  43    27      6M      63G     374063  0        0
    imgstore_appid  53    329     87M     2058G   7870529 50      25M
    server_addr     10.128.1.17     2        8968   24M     1849    0        0
    server_addr     127.0.0.1       1       6M      5G      912     1        0
    server_addr     180.96.x.1   3358    934M    27550G  141277391       891     356M
    server_addr     180.96.x.2   78      45M     220G    400704  0        0
    server_addr     180.96.x.3   242     58M     646G    2990547 42      7M
    server_name     d.123.sogou.com 478     115M    2850G   30218726        115     39M
    server_name     dl.pinyin.sogou.com     913     312M    8930G   35345453        225     97M
    server_name     download.ie.sogou.com   964     275M    7462G   7979817 297     135M

## 安装

```shell
    wget "http://nginx.org/download/nginx-1.3.5.tar.gz"
    tar -xzvf nginx-1.3.5.tar.gz
    cd nginx-1.3.5/

    patch -p1 < /path/to/ngx_req_status/write_filter-VERSION.patch

    ./configure --prefix=/usr/local/nginx \
                --add-module=/path/to/ngx_req_status

    make -j2
    make install
```
## 补丁
=======

根据nginx版本选择补丁文件：

### write_filter-1.7.11.patch

* **1.9.0-1.9.1**
* **1.8.0**
* **1.7.11-1.7.12**

### write_filter.patch

* **1.7.0-1.7.10**
* **1.6.x**
* **1.5.x**
* **1.4.x**
* **1.3.x**
* **1.2.x**
* **1.1.x**
* **1.0.x**

## 变化

## 作者

* Lanshun Zhou *&lt; zls0424@gmail.com&gt; *

## 版权和许可证

我从[limit_req module]借了很多代码(http://nginx.org/en/docs/http/ngx_http_limit_req_module.html)nginx的。这部分代码的版权归igor sysoev所有。

####  BSD license

This module is licensed under the terms of the BSD license.

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions
are met:

* Redistributions of source code must retain the above copyright notice, this list of conditions and the following disclaimer.
* Redistributions in binary form must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
"AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR
A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT
HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, 
SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED
TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR
PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF
LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING
NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
