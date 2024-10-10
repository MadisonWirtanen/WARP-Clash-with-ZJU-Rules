# WARP-Clash-with-ZJU-Rules

**更新:** 更新DNS设置

一个结合[WARP-Clash-API](https://github.com/vvbbnn00/WARP-Clash-API), [edgetunnel](https://github.com/cmliu/edgetunnel)并使用[ZJU-Rules](https://github.com/MadisonWirtanen/WARP-Clash-with-ZJU-Rules/blob/main/ZJU.yaml)的免费Clash订阅配置模板

[WARP-Clash-API](https://github.com/vvbbnn00/WARP-Clash-API) 可以将WARP服务转为Clash订阅, 无限流量且响应速度极快, 但无法访问OpenAI、New Bing、Copilot等服务; [edgetunnel](https://github.com/cmliu/edgetunnel) 可以将CloudFlare Workers服务转为Vless节点, 可以访问OpenAI、New Bing、Copilot等服务, 但每月流量有限且访问安全等级较高的网站会出现隐私错误. 将二者结合即可实现免费的完全体Clash订阅, 演示配置文件见 [clash.yaml](https://github.com/MadisonWirtanen/WARP-Clash-with-ZJU-Rules/blob/main/clash.yaml), URL导入可使用 [clash.yaml(镜像)](https://mirror.ghproxy.com/https://raw.githubusercontent.com/MadisonWirtanen/WARP-Clash-with-ZJU-Rules/main/clash.yaml). 

一些说明: 

- 配置文件中同时使用了[aiboboxx/clashfree](https://github.com/aiboboxx/clashfree)、[ermaozi/get_subscribe](https://github.com/ermaozi/get_subscribe)提供的节点信息作为WARP节点不可用时的补充, 当WARP节点无法使用时可尝试刷新`代理 - Provider`
- 演示配置文件中的Vless节点为演示服务, 可用性不保证, 可自行按[edgetunnel](https://github.com/cmliu/edgetunnel)文档部署服务替换演示节点
- Vless节点特性需要Clash.Meta内核, 故Clash客户端建议使用[clash-verge-rev（Windows）](https://github.com/clash-verge-rev/clash-verge-rev)、[ClashMetaForAndroid（Android）](https://github.com/MetaCubeX/ClashMetaForAndroid)
- 想要实现URL导入配置文件的可以fork并更改配置后从GitHub仓库的文件链接或[jsDelivr CDN](https://www.jsdelivr.com/github)导入, 或者想要隐私性更强可参考[这篇教程](https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/Clash/%E5%9F%BA%E7%A1%80%E7%AF%87/%E7%94%9F%E6%88%90%E5%B8%A6%E6%9C%89%E8%87%AA%E5%AE%9A%E4%B9%89%E7%AD%96%E7%95%A5%E7%BB%84%E5%92%8C%E8%A7%84%E5%88%99%E7%9A%84%20Clash%20%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E7%9B%B4%E9%93%BE-geodata%20%E6%96%B9%E6%A1%88.md#%E4%BA%8C-%E6%B7%BB%E5%8A%A0%E6%A8%A1%E6%9D%BF)修改配置及在Gist上生成secret gist
- 参考文档: [Clash.Meta Profile文档](https://wiki.metacubex.one/config/), [Clash Rules仓库](https://github.com/Loyalsoldier/clash-rules)