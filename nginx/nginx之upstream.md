# upstream使用

## 示例

- `upstream` 添加在 `http` 节点下

```nginx
upstream linuxidc {
	server 10.0.0.2:9000;
	server 10.0.0.3:9000;
}
```

- 在 `server` 节点下的 `location` 节点中定义 `proxy_pass`

```nginx
location / {
	root html;
	index index.html index.htm;
	proxy_pass http://linuxidc;
}
```

## `upstream`负载均衡算法

- 轮询（默认），每个请求按时间顺序逐一分配到不同的后端服务器，如果后端某台服务器宕机，故障系统被自动剔除，使用户访问不受影响。Weight 指定轮询权值，Weight值越大，分配到的访问机率越高，主要用于后端每个服务器性能不均的情况下。

- `ip_hash` 每个请求按访问IP的hash结果分配，这样来自同一个IP的访客固定访问一个后端服务器，有效解决了动态网页存在的session共享问题。

**注：当负载调度算法为ip_hash时，后端服务器在负载均衡调度中的状态不能是weight和backup**

```nginx
upstream linuxidc {
	ip_hash;
	server 10.0.0.2:9000 weight=5;
	server 10.0.0.3:9000 weight=10;
}
```

- `fair`(第三方) 这是比上面两个更加智能的负载均衡算法。此种算法可以依据页面大小和加载时间长短智能地进行负载均衡，也就是根据后端服务器的响应时间来分配请求，响应时间短的优先分配。 `Nginx` 本身是不支持 `fair` 的，如果需要使用这种调度算法，必须下载 `Nginx` 的 `upstream_fair` 模块。

```nginx
upstream linuxidc {
	server 10.0.0.2:9000 weight=5;
	server 10.0.0.3:9000 weight=10;
	fair;
}
```

- `url_hash`(第三方) 此方法按访问 `url` 的 `hash` 结果来分配请求，使每个 `url` 定向到同一个后端服务器，可以进一步提高后端缓存服务器的效率。 `Nginx` 本身是不支持 `url_hash` 的，如果需要使用这种调度算法，必须安装 `Nginx` 的 `hash` 软件包；使用时server语句将不能写入`weight`等其他参数；

```nginx
upstream linuxidc {
	server 10.0.0.2:9000;
	server 10.0.0.3:9000;
	hash $request_uri; 
	hash_method crc32;
}
```

## upstream 支持的状态参数

在HTTP Upstream模块中，可以通过server指令指定后端服务器的IP地址和端口，同时还可以设定每个后端服务器在负载均衡调度中的状态。常用的状态有:

- `down` 表示单前的server暂时不参与负载.
- `backup：` 其它所有的非backup机器down或者忙的时候，请求backup机器。所以这台机器压力会最轻。
- `max_fails` ：允许请求失败的次数，默认为`1`.当超过最大次数时，返回`proxy_next_upstream` 模块定义的错误.
- `fail_timeout` : 经过`max_fails`次失败后，暂停的时间。

```nginx
upstream bakend{ #定义负载均衡设备的Ip及设备状态 
      ip_hash; 
      server 10.0.0.11:9090 down; 
      server 10.0.0.11:8080 weight=2; 
      server 10.0.0.11:6060; 
      server 10.0.0.11:7070 backup; 
}
```
