# Arcane for LazyCat

Modern Docker Management, Designed for Everyone

上游：https://github.com/getarcaneapp/arcane

## 使用

要求懒猫微服 1.5.0 或更新版本，目标架构 amd64。首次使用 arcane / arcane-admin 登录，然后按提示修改密码。时区保留原 Compose 的 UTC。保留手动登录，不注入文件选择器。

原 Compose 的 Docker socket 通过构建配置的 Compose 扩展挂载，数据持久化到 `/lzcapp/var/data`（容器 `/app/data`）。保留 `cgroup: host`。Socket 提供宿主 Docker 管理能力，请仅授权可信用户。目标微服需存在 `/var/run/docker.sock`，实际 socket 权限和管理操作需安装后确认。不要通过此工具随意改动微服管理的系统容器。

加密密钥由懒猫 `stable_secret` 生成并截取为 32 字符，不在仓库中保存明文密钥，同一微服上的同一应用重启和升级后保持稳定。迁移到其他微服时必须同时迁移原密钥与数据。

## 构建与发布

```sh
mkdir -p dist
lzc-cli project release -o dist/application.lpk
lzc-cli lpk info dist/application.lpk
```

只发布喵喵商店，不发布官方商店。使用 `ghcr.1ms.run` 镜像，并在自动更新时验证目标架构摘要与 GHCR 一致。初始版本为 `2.11.0-next.30`，每天跟踪 `vX.Y.Z-next.N` 预发布版本。

工作流引用组织级 `APPSTORE_URL`、`APPSTORE_TOKEN` 与可选的 `PRIVATE_STORE_GROUP_CODES`。发布文件为 `community.lazycat.app.arcane-v<version>.lpk`，喵喵商店引用 GitHub Release 下载地址和 SHA256。

本地打包和工作流校验不能替代微服实机验证。图标由用户提供，应用代码及许可证见上游仓库。
