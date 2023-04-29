# 简介

本项目参考[@Loyalsoldier/clash-rules](https://github.com/Loyalsoldier/clash-rules)，将自定义Clash分流策略转换为适用于 Clash Premium 内核的规则集（RULE-SET）。

## 说明

分流规则模板可以使用[@ACL4SSR/ACL4SSR](https://github.com/ACL4SSR/ACL4SSR/tree/master)，也可以使用[@Loyalsoldier/clash-rules](https://github.com/Loyalsoldier/clash-rules)，但`ACL4SSR`只能使用[GithubApi](###使用github-api)。

`convert.yml`工作流程为：
1. 更新`.list`文件时触发
2. `github[bot]`会将`.list`文件转换为`rule-providers`字段及对应`rules`字段的配置
3. 将转换结果上传到项目的[release](https://github.com/jankiny/ClashRules/tree/release)分支
4. 通过jsDelivr加速资源


## How to use

完整的使用模板参考[template](https://github.com/jankiny/ClashRules/tree/master/template)目录。

在Clash的配置中通过`rule-providers`获取Clash分流的规则（可以使用[GithubApi](###使用github-api)获取，也可以使用[jsdelivr cdn](###使用jsdelivr-cdn)获取，使用jsdelivr需要过一段时间才生效），之后只需要刷新规则，即可实现分类规则热更新。
### 使用github api

```bash
# example
rule-providers:
  American:
      behavior: classical 
      type: http
      url: "https://raw.githubusercontent.com/jankiny/ClashRules/release/American.list.yaml"
      interval: 86400
      path: ./ClashRules/American.list.yaml
rules:
 - RULE-SET,American,🚀 节点选择 
```

### 使用jsdelivr cdn
```bash
# example
rule-providers:
  American:
    behavior: classical 
    type: http
    url: "https://cdn.jsdelivr.net/gh/jankiny/ClashRules@release/American.list.yaml"
    interval: 86400
    path: ./ClashRules/American.list.yaml
rules:
 - RULE-SET,American,🚀 节点选择 
```
