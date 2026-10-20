# 简介

## 推荐流程：mihomo 规则与节点分离

新的 [公共模板](template/mihomo.yaml) 保留 Janki 自用 `NoAuto.ini` 的分类和手动选择方式。规则通过 `rule-providers` 更新；机场、自建节点各自通过 `proxy-providers` 更新。共享模板没有私人订阅地址，同事填写自己的来源即可使用。

- [接入、更新与分享步骤](docs/mihomo-workflow.md)
- [旧 x-ui 备份、重装与 3x-ui 交接](docs/vps-migration.md)
- [重装后无机场时的离线恢复手册](docs/vps-offline-reinstall.md)
- [VPS 节点优化、对照测试与回退](docs/vps-optimization.md)
- [当前主备节点与独立额度](docs/native-node-quotas.md)
- [新加坡备用节点、统一管理与过期移除](docs/singapore-backup.md)
- [日本 Windows 备用节点、独立限额与恢复](docs/windows-backup.md)
- [历史：整机共享月度流量保护](docs/traffic-budget.md)
- [历史：单节点选择与原生月度额度](docs/single-node.md)
- [本地检查与实际内核验收](docs/validation.md)

完成 VPS 部署并保存私人管理信息后，可运行 `python3 scripts/open_xui_panel.py`，自动建立 SSH 隧道、显示账号密码并打开面板。默认读取 `private/new-vps-result.json`，SSH 端口为 `2022`（可用 `--ssh-port` 覆盖），支持 `--status` 查看连接、`--stop` 关闭本脚本管理的隧道。

首次接入在本机生成一次私人配置；后续规则内容和节点内容分别刷新。修改分组或规则优先级时才升级主模板。个人配置启用完整 `Personal.list` 并固定直连，同事默认不启用。

也可将生成结果通过 3x-ui 的固定 HTTPS 订阅发布，之后在客户端直接添加订阅链接。[发布和导入步骤](docs/mihomo-workflow.md#通过订阅导入完整配置) 保留规则与节点独立刷新，并提供原设置备份和回退。

```bash
uv sync --locked
mkdir -m 700 -p private
cp examples/personal-settings.yaml private/settings.yaml
# 在本机编辑 private/settings.yaml 和 private/self-hosted.txt 后执行：
uv run --locked python scripts/build_config.py
```

同事使用 `examples/shared-settings.yaml`。个人订阅与节点文件放在被 Git 忽略的 `private/` 中。`examples/` 的占位地址不能直接使用。Windows 代理继续供家庭服务使用时，个人设置保留 `allow_lan: true`；NAS 常驻代理在后续阶段部署。

新配置直接读取 `master` 的 `.list`，不依赖下面的旧版生成和 CDN 流程。新增公共模板与脚本须发布到仓库后，远程下载入口才能使用；本地生成工具可以直接从当前工作目录运行。

## 旧版 Clash Premium 配置

本项目参考[@Loyalsoldier/clash-rules](https://github.com/Loyalsoldier/clash-rules)，将自定义Clash分流策略转换为适用于 Clash Premium 内核的规则集（RULE-SET）。

## 说明

分流规则模板可以使用[@ACL4SSR/ACL4SSR](https://github.com/ACL4SSR/ACL4SSR/tree/master)，也可以使用[@Loyalsoldier/clash-rules](https://github.com/Loyalsoldier/clash-rules)。在国内使用[GithubApi](###使用github-api)会出现错误，因此将[@ACL4SSR/ACL4SSR](https://github.com/ACL4SSR/ACL4SSR/tree/master/Clash/Providers)中的内容同步到本项目并通过`jsdelivr cnd`加速，建议使用[jsdelivr cdn](###使用jsdelivr-cdn)。

`convert.yml`工作流程为：
1. 更新`.list`文件时触发
2. `github[bot]`会将`.list`文件转换为`rule-providers`字段及对应`rules`字段的配置
3. 将转换结果上传到项目的[release](https://github.com/jankiny/ClashRules/tree/release)分支
4. 通过jsDelivr加速资源

`sync-acl4ssr.yml`工作流程为：
1. 每天0点或更新master分支时触发
2. 将[@ACL4SSR/ACL4SSR/Clash/Providers](https://github.com/ACL4SSR/ACL4SSR/tree/master/Clash/Providers)中的文件同步到本项目[acl4ssr](https://github.com/jankiny/ClashRules/tree/acl4ssr)分支
3. 通过jsDelivr加速资源


## How to use

<!-- [template](https://github.com/jankiny/ClashRules/tree/master/template)中有两个版本的目标，将文件内容复制到[节点工具](https://v2rayse.com/clash-template)中。 -->

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
