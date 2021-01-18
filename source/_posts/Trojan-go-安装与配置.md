---
title: Trojan-go 安装与配置
date: 2021-01-18 21:06:45
tags: 
	- Trojan-go
---

Trojan-go 版本：0.8.2

github 地址 : https://github.com/p4gefau1t/trojan-go

doc 地址 : 

## server 端 (CentOS 8)

### 安装

```bash
wget https://github.com/p4gefau1t/trojan-go/releases/download/v0.8.2/trojan-go-linux-amd64.zip

unzip -o trojan-go-linux-amd64.zip -d /usr/local/bin/trojan-go
```

### 配置

server.json

```json
{
    "run_type": "server",
    "local_addr": "0.0.0.0",
    "local_port": 443,
    "remote_addr": "127.0.0.1",
    "remote_port": 80,
    "password": [
        "password"
    ],
    "ssl": {
        "cert": "certificate.crt", 	// 域名https证书
        "key": "private.key",		// 域名 私钥
        "sni": "域名"
    },
    "router": {
        "enabled": true,
        "block": [
            "geoip:private"
        ],
        "geoip": "/usr/share/trojan-go/geoip.dat",
        "geosite": "/usr/share/trojan-go/geosite.dat"
    },
    "websocket": {
        "enabled": true,
        "path": "/trojan-go",
        "host": "域名"
    }
}
```

### 后台运行

```bash
nohup ./trojan-go -config server.json > server.log 2>&1 &
```

## Client(Windows)

下载地址 https://github.com/p4gefau1t/trojan-go/releases/download/v0.8.2/trojan-go-windows-amd64.zip

### 启动脚本

start.bat

```bash
@ECHO OFF
%1 start mshta vbscript:createobject("wscript.shell").run("""%~0"" ::",0)(window.close)&&exit
start /b trojan-go.exe -config client.json
```

### 关闭脚本

stop.bat

```bash
@ECHO OFF

taskkill /im trojan-go.exe /f

ping -n 2 127.1 >nul
```

### 配置文件

client.json

```
{
    "run_type": "client",
    "local_addr": "127.0.0.1",
    "local_port": 1080,
    "remote_addr": "域名",
    "remote_port": 443,
    "password": [
        "AC60XtgP"
    ],
    "ssl": {
        "sni": "域名"
    },
    "mux": {
        "enabled": true,
		"concurrency": 8,
		"idle_timeout": 60
    },
    "router": {
        "enabled": true,
        "bypass": [
            "geoip:cn",
            "geoip:private",
            "geosite:cn",
            "geosite:geolocation-cn"
        ],
        "block": [
            "geosite:category-ads"
        ],
        "proxy": [
            "geosite:geolocation-!cn"
        ],
        "default_policy": "proxy",
        "geoip": "/usr/share/trojan-go/geoip.dat",
        "geosite": "/usr/share/trojan-go/geosite.dat"
    },
    "websocket": {
        "enabled": true,
        "path": "/trojan-go",
        "host": "域名"
    }
}
```

### chrome 插件 proxy-switchyomega

### 使用 cloudflare 做 CDN 加速