---
# This is the title of the article
title: 配置文件
# This is the icon of the page
icon: json
# This control sidebar order
order: 1
# A page can have multiple categories
category:
  - Config
# A page can have multiple tags
tag:
  - Config
  - Settings
# this page is sticky in article list
sticky: true
# this page will appear in starred articles
star: true
---

### 初始配置

::: tip
`config.json`内配置文件修改后都需要重启 AList 才会生效

- Windows/Mac：和 AList 同级文件夹內的 `data/config.json`
- Linux：一键脚本路径,、/opt/alist/`data/config.json`，手动安装 /xx 路径/`data/config.json`
- Docker：进入 Docker 容器内`data/config.json`
- openwrt：如果使用的是`luci-app-alist`,请在网页修改,其他自行找到 AList 执行文件同级目录`data/config.json`
- 其他：找到 AList 同级文件夹內的`data/config.json`

:::

```json
{
  "force": false,
  "site_url": "",
  "cdn": "",
  "jwt_secret": "random_generated",
  "token_expires_in": 48,
  "database": {
    "type": "sqlite3",
    "host": "",
    "port": 0,
    "user": "",
    "password": "",
    "name": "",
    "db_file": "data\\data.db",
    "table_prefix": "x_",
    "ssl_mode": ""
  },
  "scheme": {
    "address": "0.0.0.0",
    "http_port": 5244,
    "https_port": -1,
    "force_https": false,
    "cert_file": "",
    "key_file": "",
    "unix_file": ""
  },
  "temp_dir": "data\\temp",
  "bleve_dir": "data\\bleve",
  "log": {
    "enable": true,
    "name": "data\\log\\log.log",
    "max_size": 10,
    "max_backups": 5,
    "max_age": 28,
    "compress": false
  },
  "delayed_start": 0,
  "max_connections": 0,
  "tls_insecure_skip_verify": true
}
```

## 字段说明

### **force**

程序会优先从环境变量中读取配置，设置 `force` 为 `true` 会使程序忽略环境变量强制读取配置文件。

### **site_url**

你的网站 URL，比如`https://pan.nn.ci`，这个地址会在程序中的某些地方使用，如果不设置这个字段，一些功能可能无法正常工作，比如

- 本地存储的缩略图
- 开启 web 代理后的预览
- 开启 web 代理后的下载地址
- 反向代理至二级目录
- ...

URL 链接结尾请勿携带 `/` ，参照如下示例，否则也将无法使用上述功能或出现异常

```json
# 正确写法：
"site_url": "https://al.nn.ci",
# 错误写法：
"site_url": "https://al.nn.ci/",
```

### **cdn**

CDN 地址，如果要使用 CDN，可以设置该字段，`$version` 会被替换为 `alist-web` 的实际版本
这是动态的。 现有的 dist 资源托管在 npm 和 GitHub 上，它们的位置是：

- https://www.npmjs.com/package/alist-web
- https://github.com/alist-org/web-dist

所以你可以使用任何 npm 或 GitHub CDN 作为路径，例如：

- https://registry.npmmirror.com/alist-web/$version/files/dist/
- https://cdn.jsdelivr.net/npm/alist-web@$version/dist/
- https://unpkg.com/alist-web@$version/dist/
- https://cdn.jsdelivr.net/gh/alist-org/web-dist@$version/dist/
- https://cdn1.tianli0.top/npm/alist-web@$version/dist/
- https://cdn1.tianli0.top/gh/alist-org/web-dist@$version/dist/
- https://npm.elemecdn.com/alist-web@$version/dist/
- https://jsd.onmicrosoft.cn/npm/alist-web@$version/dist/
- https://jsd.onmicrosoft.cn/gh/alist-org/web-dist@$version/dist/

您也可以将其设置为空以使用本地 dist。

### **jwt_secret**

用于签署 JWT 令牌的密钥，第一次启动时随机生成。

### **token_expires_in**

用户登录过期时间，单位：小时

### **database**

数据库配置，默认是 `sqlite3`，也可以使用 `mysql` 或者 `postgres`。

- 如果不使用`MySQL`或者`postgres`，配置文件数据库选项不用修改

```json
  "database": {
    "type": "sqlite3",  //数据库类型
    "host": "",         //数据库地址
    "port": 0,          //数据库端口号
    "user": "",         //数据库账号
    "password": "",     //数据库密码
    "name": "",         //数据库库名
    "db_file": "data\\data.db",     //数据库位置,sqlite3使用的
    "table_prefix": "x_",           //数据库表名前缀
    "ssl_mode": ""      //来控制SSL握手时的加密选项,参数自行搜索，或者查看下方来自ChatGPT的回答
  },
```

:::: details 展开查看`ssl_mode`参数选项

如果不知道如何填，默认空白即可，不用修改，不填不能用的话自行研究，无法提供太多有效的帮助

---

在 MySQL 中，`ssl_mode`参数是用于指定 SSL 连接的验证模式。以下是几种常见的选项：

- `DISABLED`: 禁用 SSL 连接。
- `PREFERRED`: 如果服务器启用了 SSL，则使用 SSL 连接；否则使用普通连接。
- `REQUIRED`: 必须使用 SSL 连接，如果服务器不支持 SSL 连接，则连接失败。
- `VERIFY_CA`: 必须使用 SSL 连接，并验证服务器证书的可信性。
- `VERIFY_IDENTITY`: 必须使用 SSL 连接，并验证服务器证书的可信性和名称是否与连接的主机名匹配。

MySQL 5.x 和 8.x 也不一样。如果使用服务商提供的免费/收费数据库，服务商会有文档说明。自己部署的数据库那自己肯定知道。

---

在 PostgreSQL 中，`ssl_mode`参数用于指定客户端如何使用 SSL 连接。以下是几种常见的选项：

- `disable`: 禁用 SSL 连接。
- `allow`: 允许使用 SSL 连接，但不需要。
- `prefer`: 如果服务器启用了 SSL，则使用 SSL 连接；否则使用普通连接。
- `require`: 必须使用 SSL 连接，如果服务器不支持 SSL 连接，则连接失败。
- `verify-ca`: 必须使用 SSL 连接，并验证服务器证书的可信性。
- `verify-full`: 必须使用 SSL 连接，并验证服务器证书的可信性和名称是否与连接的主机名匹配。

---

::: right

:warning::warning:**以上参数来自 ChatGPT，未验证真实性/实用性/准确性:warning:**:warning:

:::

::::

### **scheme**

协议配置，如果要使用 HTTPS，可以设置该字段。

- 填写示例：记得把证书文件丢到 data 目录里面才会识别到喔~

```json
  "scheme": {
    "address": "0.0.0.0",   // 要监听的http/https地址，默认为 0.0.0.0
    "http_port": 5244,      // 监听的http端口,默认的“5244”,如果你想禁用http,将其设置为'-1'
    "https_port": -1,       // https端口监听,默认的'-1',如果你想启用https,将其设置为非'-1'
    "force_https": false,   // 是否强制使用HTTPS协议,如果设置为true,则用户只能通过HTTPS访问该网站
    "cert_file": "data\\cert.crt",  // 证书文件路径
    "key_file": "data\\key.key",    // 证书密钥文件路径
    "unix_file": ""         // Unix监听套接字文件路径,默认的空的,如果你想使用Unix socket,将其设置为非空
  },
```

### **temp_dir**

程序临时目录，默认 `data/temp`

::: danger
temp_dir 为 alist 独占的临时文件夹，为避免程序中断产生垃圾文件会在每次启动时清空，故请不要手动在此文件夹内放置任何内容，也不要在使用 docker 时将此文件夹及其子文件夹映射至正在使用的文件夹。
:::

### **bleve_dir**

你使用 **`bleve`** 索引时,数据存放的位置

### **log**

日志配置，如果要查看详细日志（或禁用它），可以设置该字段。

```json
  "log": {
    "enable": true,					//开启日志记录功能，默认为开启状态 true
    "name": "data\\log\\log.log",	//日志文件的路径和名称
    "max_size": 10,					//单个日志文件的最大大小，单位为MB。达到指定大小后会自动切分文件
    "max_backups": 5,				//保留的日志备份数量，超过数量会自动删除旧的备份
    "max_age": 28,					//日志文件保存的最大天数，超过天数的日志文件会被删除
    "compress": false				//是否启用日志文件压缩功能。压缩后可以减小文件大小，但查看时需要解压缩，默认为关闭状态 false
  },
```

### **delayed_start**

**单位：秒** (v3.19.0 新增功能)

是否延时启动，一般此功能常用于 AList 开机自启动选项

因为有时候网络连接的慢，导致 AList 启动过快后需要网络连接的驱动无法连接导致无法正常打开

### **max_connections**

同时最多的连接数(并发)，默认为 0 即不限制.

- 对于一般的设备比如 n1 推荐 10 或者 20
  - 使用场景（例如打开图片模式会并发不是很好的设备就会崩溃）

### **tls_insecure_skip_verify**

是否检查 SSL 证书，开启后如使用的网站的证书出现问题（如未包含中级证书、证书过期、证书伪造等），将不能使用该服务，关闭该选项请尽量在安全的网络环境下运行程序
