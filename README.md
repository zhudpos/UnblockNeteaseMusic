<img src="https://user-images.githubusercontent.com/26399680/47980314-0e3f1700-e102-11e8-8857-e3436ecc8beb.png" alt="logo" width="140" height="140" align="right">

# UnblockNeteaseMusic

解锁网易云音乐客户端变灰歌曲

## 特性

- 使用 QQ / 虾米 / 百度 / 酷狗 / 酷我 / 咪咕 / JOOX 音源替换变灰歌曲链接 (默认仅启用一、五、六)
- 为请求增加 `X-Real-IP` 参数解锁海外限制，支持指定网易云服务器 IP，支持设置上游 HTTP / HTTPS 代理
- 完整的流量代理功能 (HTTP / HTTPS)，可直接作为系统代理 (同时支持 PAC)

## 在vps进行自签名ca证书

```
# 生成 CA 私钥
openssl genrsa -out ca.key 2048

# 生成 CA 证书 ("YOURNAME" 处填上你自己的名字)
openssl req -x509 -new -nodes -key ca.key -sha256 -days 1825 -out ca.crt -subj "/C=CN/CN=UnblockNeteaseMusic Root CA/O=YOURNAME"

# 生成服务器私钥
openssl genrsa -out server.key 2048

# 生成证书签发请求
openssl req -new -sha256 -key server.key -out server.csr -subj "/C=CN/L=Hangzhou/O=NetEase (Hangzhou) Network Co., Ltd/OU=IT Dept./CN=*.music.163.com"

# 使用 CA 签发服务器证书
openssl x509 -req -extfile <(printf "extendedKeyUsage=serverAuth\nsubjectAltName=DNS:music.163.com,DNS:*.music.163.com") -sha256 -days 365 -in server.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out server.crt

# 证书生成后，放置在/root/docker/unmusic/crt下，路径与docker run时保持一致。

```
## 在mac进行ca证书安装

```
https://blog.csdn.net/weixin_34417183/article/details/88928123
```

## 运行

使用 Docker

```
$ docker run --restart=always --name unmusic -v /root/docker/unmusic/crt/server.crt:/usr/src/app/server.crt -v /root/docker/unmusic/crt/server.key:/usr/src/app/server.key -d -p 80:8080 -p 443:8081 nondanee/unblockneteasemusic --port 8080:8081
```

## 使用

**警告：本项目不提供线上 demo，请不要轻易信任使用他人提供的公开代理服务，以免发生安全问题**

### 方法 1. 修改 hosts

向 hosts 文件添加两条规则

```
<Server IP> music.163.com
<Server IP> interface.music.163.com
```

### 方法 2. 设置代理

PAC 自动代理脚本地址 `http://<Server Name:PORT>/proxy.pac`

全局代理地址填写服务器地址和端口号即可

| 平台    | 基础设置 |
| :------ | :------------------------------- |
| Windows | 设置 > 工具 > 自定义代理 (客户端内) |
| UWP     | Windows 设置 > 网络和 Internet > 代理 |
| Linux   | 系统设置 > 网络 > 网络代理 |
| macOS   | 系统偏好设置 > 网络 > 高级 > 代理 |
| Android | WLAN > 修改网络 > 高级选项 > 代理 |
| iOS     | 无线局域网 > HTTP 代理 > 配置代理 |


## 致谢

感谢大佬们为逆向 eapi 所做的努力

使用的其它平台音源 API 出自

[trazyn/ieaseMusic](https://github.com/trazyn/ieaseMusic)

[listen1/listen1_chrome_extension](https://github.com/listen1/listen1_chrome_extension)

向所有同类项目致敬

[EraserKing/CloudMusicGear](https://github.com/EraserKing/CloudMusicGear)

[EraserKing/Unblock163MusicClient](https://github.com/EraserKing/Unblock163MusicClient)

[ITJesse/UnblockNeteaseMusic](https://github.com/ITJesse/UnblockNeteaseMusic/)

[bin456789/Unblock163MusicClient-Xposed](https://github.com/bin456789/Unblock163MusicClient-Xposed)

[YiuChoi/Unlock163Music](https://github.com/YiuChoi/Unlock163Music)

[yi-ji/NeteaseMusicAbroad](https://github.com/yi-ji/NeteaseMusicAbroad)

[stomakun/NeteaseReverseLadder](https://github.com/stomakun/NeteaseReverseLadder/)

[fengjueming/unblock-NetEaseMusic](https://github.com/fengjueming/unblock-NetEaseMusic)

[acgotaku/NetEaseMusicWorld](https://github.com/acgotaku/NetEaseMusicWorld)

[mengskysama/163-Cloud-Music-Unlock](https://github.com/mengskysama/163-Cloud-Music-Unlock)

[azureplus/163-music-unlock](https://github.com/azureplus/163-music-unlock)

[typcn/163music-mac-client-unlock](https://github.com/typcn/163music-mac-client-unlock)

## 许可

The MIT License
