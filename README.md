# ClashRules
自定义clash分流策略

```bash

cat Personal.list | grep -E "(DOMAIN-SUFFIX|DOMAIN-KEYWORD|DOMAIN|SRC-IP-CID|IP-CIDR|DST-PORT|SRC-PORT)" | awk -F',' '{print " - " $1","$2}' > test.yaml
```