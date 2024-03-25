# WARP-Clash-with-ZJU-Rules

一个结合[WARP-Clash-API](https://github.com/vvbbnn00/WARP-Clash-API)、[edgetunnel](https://github.com/cmliu/edgetunnel)并使用ZJU-Rules的免费Clash订阅配置模板

[WARP-Clash-API](https://github.com/vvbbnn00/WARP-Clash-API) 可以将WARP服务转为Clash订阅，无限流量且响应速度极快，但无法访问OpenAI、New Bing、Copilot等服务。[edgetunnel](https://github.com/cmliu/edgetunnel) 可以将CloudFlare Workers服务转为Vless节点，可以访问OpenAI、New Bing、Copilot等服务，但每月流量有限且访问安全等级较高的网站会出现隐私错误。故二者结合即可实现免费的完全体Clash订阅，以下是配置模板（内容与clash.yaml相同）。

注：

- 配置文件中同时使用了[aiboboxx/clashfree](https://github.com/aiboboxx/clashfree)、[ermaozi/get_subscribe](https://github.com/ermaozi/get_subscribe)提供的节点信息作为补充
- 只需将Vless节点中空余处对应按[edgetunnel](https://github.com/cmliu/edgetunnel)文档部署服务中显示的信息填入即可
- 特性需要Clash.Meta内核，故Clash客户端建议使用[clash-verge-rev（Windows）](https://github.com/clash-verge-rev/clash-verge-rev)、[ClashMetaForAndroid（Android）](https://github.com/MetaCubeX/ClashMetaForAndroid)
- 移动端因为性能原因以下模板可能无法正常使用，可考虑使用clashlite.yaml文件模板，iOS端软件可能不适配配置文件
- 想要实现URL导入配置文件的可以fork并更改配置后从GitHub仓库的文件链接或[jsDelivr CDN](https://www.jsdelivr.com/github)导入，或者想要隐私性更强可参考[这篇教程](https://github.com/DustinWin/clash_singbox-tutorials/blob/main/教程合集/Clash/基础篇/生成带有自定义策略组和规则的 Clash 配置文件直链-geodata 方案.md)修改配置及在Gist上生成secret gist

```yaml
# 代理集合（获取机场订阅链接内的所有节点）
proxy-providers:
  🛫 WARP:
    type: http
    # 源地址 https://github.com/vvbbnn00/WARP-Clash-API
    url: "https://subs.zeabur.app/clash"
    path: ./proxies/warp.yaml
    interval: 600
    # 初步筛选需要的节点，可有效减轻路由器压力，支持正则表达式，不筛选可删除此配置项
    filter: "(?i)CF|WARP|港|hk|hongkong|hong kong|美|us|unitedstates|united states|NL"
    health-check:
      enable: true
      # 未选择到当前代理集合时，不会进行测试，有多个代理集合时可使用
      lazy: true
      url: "https://www.gstatic.com/generate_204"
      interval: 600

  🛫 我的机场 1:
    type: http
    # 源地址 https://github.com/aiboboxx/clashfree
    url: "https://cdn.jsdelivr.net/gh/aiboboxx/clashfree@main/clash.yml"
    path: ./proxies/airport1.yaml
    interval: 43200
    filter: "^(?!.*(?i)(CloudFlare)).*$"
    health-check:
      enable: true
      lazy: true
      url: "https://www.gstatic.com/generate_204"
      interval: 600

  🛫 我的机场 2:
    type: http
    # 源地址 https://github.com/ermaozi/get_subscribe
    url: "https://cdn.jsdelivr.net/gh/ermaozi/get_subscribe@main/subscribe/clash.yml"
    path: ./proxies/airport2.yaml
    interval: 43200
    # filter: "(?i)港|hk|hongkong|hong kong|台|tw|taiwan|日本|jp|japan|新|sg|singapore|美|us|unitedstates|united states"
    health-check:
      enable: true
      lazy: true
      url: "https://www.gstatic.com/generate_204"
      interval: 600
  # 更多免费订阅收集仓库: https://github.com/WilliamStar007/ClashX-V2Ray-TopFreeProxy

# 单个出站代理节点（以 vless 为例）
proxies:
  - name: 🆓 Vless节点
    type: vless
    server: 
    port: 443
    uuid: 
    network: ws
    tls: true
    udp: false
    sni: 
    client-fingerprint: chrome
    ws-opts:
      path: "/?ed=2048"
      headers:
        host: 

# 策略组
proxy-groups:
  # 手动选择国家或地区节点；根据“国家或地区策略组”名称对 `proxies` 值进行增删改，须一一对应
  - {name: 🚀 节点选择, type: select, proxies: [🌏 WARP, 🇺🇸 美国节点, 🇺🇳 其它节点, 🆓 Vless节点, 🎯 全球直连]}
  # 选择`🎯 全球直连`为测试本地网络（运营商网络速度和 IPv6 支持情况），可选择其它节点用于测试机场节点速度和 IPv6 支持情况
  - {name: 📈 网络测试, type: select, proxies: [🎯 全球直连, 🌏 WARP, 🇺🇸 美国节点, 🇺🇳 其它节点, 🆓 Vless节点]}
  # 若机场的 UDP 质量不是很好，导致某游戏无法登录或进入房间，可以添加 `disable-udp: true` 配置项解决
  - {name: 🏫 ZJU, type: select, proxies: [🎯 全球直连]}
  - {name: 🤖 AI, type: select, proxies: [🆓 Vless节点]}
  - {name: Ⓜ️ 微软服务, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}
  - {name: 📢 谷歌服务, type: select, proxies: [🚀 节点选择, 🎯 全球直连]}
  - {name: 🍎 苹果服务, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}
  - {name: 🇨🇳 国内 IP, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}
  - {name: 🔗 直连域名, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}
  - {name: 🪜 代理域名, type: select, proxies: [🚀 节点选择, 🎯 全球直连]}
  - {name: 🎮 游戏平台, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}
  - {name: 📲 电报消息, type: select, proxies: [🚀 节点选择]}
  - {name: 🐟 漏网之鱼, type: select, proxies: [🚀 节点选择, 🎯 全球直连]}
  # 若使用 ShellCrash，由于无法判断本机进程（默认 `find-process-mode: off`），需删除此条 `🖥️ 直连软件`
  - {name: 🖥️ 直连软件, type: select, proxies: [🎯 全球直连]}
  - {name: 🔒 私有网络, type: select, proxies: [🎯 全球直连]}
  - {name: 🛑 广告拦截, type: select, proxies: [REJECT]}
  - {name: 🎯 全球直连, type: select, proxies: [DIRECT]}

  # ----------------国家或地区策略组---------------------
  # 自动选择节点，即按照 url 测试结果使用延迟最低的节点；测试后容差大于 100ms 才会切换到延迟低的那个节点；未选择到当前策略组时不会进行延迟测试；筛选出“香港”节点，支持正则表达式
  - {name: 🌏 WARP, type: url-test, tolerance: 100, lazy: true, use: [🛫 WARP], filter: "CF|WARP"}
  # 节点负载均衡，即将请求均匀分配到多个节点上，优点是更稳定，速度可能有提升；将相同顶级域名的请求分配给策略组内的同一个代理节点；推荐在节点复用比较多的情况下使用
#  - {name: 🇺🇸 美国节点, type: load-balance, strategy: consistent-hashing, lazy: true, use: [🛫 我的机场 1, 🛫 我的机场 2], filter: "(?i)美|us|unitedstates|united states"}
  - {name: 🇺🇸 美国节点, type: url-test, tolerance: 100, lazy: true, use: [🛫 我的机场 1, 🛫 我的机场 2], filter: "(?i)美|us|unitedstates|united states"}
  # 若有多个机场，可以使用 `include-all-providers: true` 代替 `use: [🛫 我的机场 1, 🛫 我的机场 2, ...]`
  - {name: 🇺🇳 其它节点, type: url-test, tolerance: 100, lazy: true, include-all-providers: true, filter: "^(?!.*(?i)(CloudFlare|CF|WARP|美|us)).*$"}

# 规则集（yaml 文件每天自动更新）
rule-providers:
  zju:
    type: http
    behavior: classical
    format: yaml
    path: ./rules/ZJU.yaml
    url: "https://gist.githubusercontent.com/MadisonWirtanen/dea50be03b79db737505e1c80c86da6c/raw/ZJU.yaml"
    interval: 86400

  ai:
    type: http
    behavior: classical
    format: yaml
    path: ./rules/ai.yaml
    url: "https://gist.githubusercontent.com/MadisonWirtanen/e965aa2048ca2e19d34d01ed21b7435d/raw/ai.yaml"
    interval: 86400

  ads:
    type: http
    behavior: domain
    format: text
    path: ./rules/ads.list
    url: "https://cdn.jsdelivr.net/gh/DustinWin/ruleset_geodata@clash-ruleset/ads.list"
    interval: 86400

  # 若使用 ShellCrash，由于无法判断本机进程（默认 `find-process-mode: off`），需删除此条 `applications`
  applications:
    type: http
    behavior: classical
    format: text
    path: ./rules/applications.list
    url: "https://cdn.jsdelivr.net/gh/DustinWin/ruleset_geodata@clash-ruleset/applications.list"
    interval: 86400

  private:
    type: http
    behavior: domain
    format: text
    path: ./rules/private.list
    url: "https://cdn.jsdelivr.net/gh/DustinWin/ruleset_geodata@clash-ruleset/private.list"
    interval: 86400

  microsoft-cn:
    type: http
    behavior: domain
    format: text
    path: ./rules/microsoft-cn.list
    url: "https://cdn.jsdelivr.net/gh/DustinWin/ruleset_geodata@clash-ruleset/microsoft-cn.list"
    interval: 86400

  apple-cn:
    type: http
    behavior: domain
    format: text
    path: ./rules/apple-cn.list
    url: "https://cdn.jsdelivr.net/gh/DustinWin/ruleset_geodata@clash-ruleset/apple-cn.list"
    interval: 86400

  google-cn:
    type: http
    behavior: domain
    format: text
    path: ./rules/google-cn.list
    url: "https://cdn.jsdelivr.net/gh/DustinWin/ruleset_geodata@clash-ruleset/google-cn.list"
    interval: 86400

  games-cn:
    type: http
    behavior: domain
    format: text
    path: ./rules/games-cn.list
    url: "https://cdn.jsdelivr.net/gh/DustinWin/ruleset_geodata@clash-ruleset/games-cn.list"
    interval: 86400

  networktest:
    type: http
    behavior: classical
    format: text
    path: ./rules/networktest.list
    url: "https://cdn.jsdelivr.net/gh/DustinWin/ruleset_geodata@clash-ruleset/networktest.list"
    interval: 86400

  proxy:
    type: http
    behavior: domain
    format: text
    path: ./rules/proxy.list
    url: "https://cdn.jsdelivr.net/gh/DustinWin/ruleset_geodata@clash-ruleset/proxy.list"
    interval: 86400

  cn:
    type: http
    behavior: domain
    format: text
    path: ./rules/cn.list
    url: "https://cdn.jsdelivr.net/gh/DustinWin/ruleset_geodata@clash-ruleset/cn.list"
    interval: 86400

  telegramip:
    type: http
    behavior: ipcidr
    format: text
    path: ./rules/telegramip.list
    url: "https://cdn.jsdelivr.net/gh/DustinWin/ruleset_geodata@clash-ruleset/telegramip.list"
    interval: 86400

  privateip:
    type: http
    behavior: ipcidr
    format: text
    path: ./rules/privateip.list
    url: "https://cdn.jsdelivr.net/gh/DustinWin/ruleset_geodata@clash-ruleset/privateip.list"
    interval: 86400

  cnip:
    type: http
    behavior: ipcidr
    format: text
    path: ./rules/cnip.list
    url: "https://cdn.jsdelivr.net/gh/DustinWin/ruleset_geodata@clash-ruleset/cnip.list"
    interval: 86400

# 规则
rules:
  - RULE-SET,ads,🛑 广告拦截
  - RULE-SET,zju,🏫 ZJU
  - RULE-SET,ai,🤖 AI
  # 若使用 ShellCrash，由于无法判断本机进程（默认 `find-process-mode: off`），需删除此条 `RULE-SET`
  - RULE-SET,applications,🖥️ 直连软件
  # 为过滤 P2P 流量（BT 下载），可添加一条 `DST-PORT` 规则（ShellCrash 会默认开启“只代理常用端口”，可忽略此项）
  - DST-PORT,6881-6889,🎯 全球直连
  - RULE-SET,private,🔒 私有网络
  - RULE-SET,microsoft-cn,Ⓜ️ 微软服务
  - RULE-SET,apple-cn,🍎 苹果服务
  - RULE-SET,google-cn,📢 谷歌服务
  - RULE-SET,games-cn,🎮 游戏平台
  - RULE-SET,networktest,📈 网络测试
  - RULE-SET,proxy,🪜 代理域名
  - RULE-SET,cn,🔗 直连域名
  - RULE-SET,telegramip,📲 电报消息
  - RULE-SET,privateip,🔒 私有网络,no-resolve
  - RULE-SET,cnip,🇨🇳 国内 IP
  - MATCH,🐟 漏网之鱼
```
