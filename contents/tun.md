## TUN 模式

对于不遵循系统代理的软件，TUN 模式可以接管其流量并交由 CFW 处理，在 Windows 中，TUN 模式性能比 TAP 模式好

### Windows

启动 TUN 模式需要进行如下操作：

1. 进入网站[Wintun](https://www.wintun.net/)，点解界面中`Download Wintun xxx`下载压缩包，根据系统版本将对应目录中`wintun.dll`复制至`Home Directory`目录中
2. 以管理员权限启动 CFW
3. 在使用的配置文件中加入如下内容：

```yaml
dns:
  enable: true
  enhanced-mode: redir-host
  nameserver:
    - 1.1.1.1 # 真实请求DNS，可多设置几个
# interface-name: en0 # 出口网卡名称，或者使用下方的自动检测
tun:
  enable: true
  stack: gvisor
  dns-hijack:
    - 198.18.0.2:53
  macOS-auto-route: true
  macOS-auto-detect-interface: false # 自动检测出口网卡
```

### macOS

启动 TUN 模式需要进行如下操作：

1. 进入`General`中，点击`Root Enable`下方`Authorize`小字，弹出授权输入密码（当前版本下，每次更新 CFW，都需要重新授权）
2. 在使用的配置文件中加入如下内容：

```yaml
dns:
  enable: true
  enhanced-mode: redir-host
  nameserver:
    - 1.1.1.1 # 真实请求DNS，可多设置几个
# interface-name: en0 # 出口网卡名称，或者使用下方的自动检测
tun:
  enable: true
  stack: system # 或 gvisor
  dns-hijack: # DNS劫持设置为系统DNS
    - 114.114.114.114 # 可任意设置，但为了保证CFW关闭后能不影响联网，建议设置真实能访问的DNS服务器
  macOS-auto-route: true
  macOS-auto-detect-interface: false # 自动检测出口网卡
```

{% hint style='info' %}
dns-hijack 不可以劫持局域网地址的 DNS，如 192.168.0.0/16，请务必手动设置系统 DNS
{% endhint %}

### 配置文件参考

https://github.com/Dreamacro/clash/wiki/Premium-Core-Features

### 技巧

{% hint style='info' %}
你可以使用[mixin](/contents/mixin.md)将上面内容合并至所有配置文件中，并使用 Mixin 开关 TUN 模式
{% endhint %}
