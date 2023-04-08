# ClashRules

自定义clash分流策略

## How to use

使用github api
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

使用jsdelivr cdn
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
