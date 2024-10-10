# WARP-Clash-with-ZJU-Rules

**更新:** 更新 DNS 设置

[WARP-Clash-API](https://github.com/vvbbnn00/WARP-Clash-API) 可以将WARP服务转为订阅, 无限流量但无法访问许多 AI 服务; [edgetunnel](https://github.com/cmliu/edgetunnel) 可以将CloudFlare Workers服务转为节点, 可以访问 AI 服务但用量有限, 将二者结合的演示配置文件见 [clash.yaml](https://github.com/MadisonWirtanen/WARP-Clash-with-ZJU-Rules/blob/main/clash.yaml), URL导入可使用 [clash.yaml(镜像)](https://mirror.ghproxy.com/https://raw.githubusercontent.com/MadisonWirtanen/WARP-Clash-with-ZJU-Rules/main/clash.yaml). 

一些说明: 

- Vless特性需要Meta内核, 故客户端建议使用[clash-verge-rev（Windows）](https://github.com/clash-verge-rev/clash-verge-rev)、[ClashMetaForAndroid（Android）](https://github.com/MetaCubeX/ClashMetaForAndroid)
- 想要实现URL导入配置文件的可以fork并更改配置后从GitHub仓库的文件链接或[jsDelivr CDN](https://www.jsdelivr.com/github)导入, 或者想要隐私性更强可参考[这篇教程](https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/Clash/%E5%9F%BA%E7%A1%80%E7%AF%87/%E7%94%9F%E6%88%90%E5%B8%A6%E6%9C%89%E8%87%AA%E5%AE%9A%E4%B9%89%E7%AD%96%E7%95%A5%E7%BB%84%E5%92%8C%E8%A7%84%E5%88%99%E7%9A%84%20Clash%20%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E7%9B%B4%E9%93%BE-geodata%20%E6%96%B9%E6%A1%88.md#%E4%BA%8C-%E6%B7%BB%E5%8A%A0%E6%A8%A1%E6%9D%BF)修改配置及在Gist上生成secret gist
- 参考文档: [Clash.Meta Profile文档](https://wiki.metacubex.one/config/), [Clash Rules仓库](https://github.com/Loyalsoldier/clash-rules)
