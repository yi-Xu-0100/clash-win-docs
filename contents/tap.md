## TAP 模式

对于不遵循系统代理的软件，TAP 模式可以接管其流量并交由 CFW 处理

对于 0.13.8 及以后版本，更推荐使用[TUN 模式](./tun.md)

### 安装 TAP 网卡

点击`General`页面中`TAP Device`选项的`Manage`按钮，在弹出对话框中选择`Install`将会安装 TAP 网卡，此网卡用于接管系统流量，安装完成可在系统网络连接中看到名为`cfw-tap`的网卡

### 启动 TAP 模式

使用的 Profile 中包含 listen 设置：

```yaml
dns:
  enable: true
  enhanced-mode: redir-host # 或 fake-ip
  listen: 0.0.0.0:53
  nameserver:
    - 223.5.5.5
```

### 工作原理

此版本可以通过设置 Interface Name (自动识别) 属性避免回环，并且支持了 UDP 及 IP 类请求，请在`Settings`页面`Interface Name`选项中选择出站网卡（通常为本机物理网卡）

### 注意事项

当`enhanced-mode`设置为`fake-ip`时，会出现系统检测到网卡无法联网，微软系 APP 无法登陆使用等问题，可以通过添加`fake-ip-filter`解决：

```yaml
dns:
  enable: true
  enhanced-mode: fake-ip
  listen: 0.0.0.0:53
  nameserver:
    - 223.5.5.5
  fake-ip-filter:
    - "dns.msftncsi.com"
    - "www.msftncsi.com"
    - "www.msftconnecttest.com"
```

{% hint style='info' %}
TAP 模式更推荐使用 redir-host 模式
{% endhint %}
