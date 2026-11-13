# qbittorrentee-lzcapp

使用 `dockers-x/Docker-qBittorrent-Enhanced-Edition` 重写后的 `czyt/qbittorrentee:<四段版本>-r1`，不再使用旧缓存镜像。先在镜像仓库完成构建和推送，再构建本应用；新标签未发布时不要安装。

版本号由 GitHub Action 根据镜像自动写入，不手动维护或额外增加修复版本号。当前本地测试包为 `5.2.410`，对应 Docker 版本 `5.2.4.10` 和重写版镜像标签 `5.2.4.10-r1`。

## 登录

安装参数支持自定义用户名和密码，默认 `admin / adminadmin`，无需查终端日志。凭据在每次启动时写入配置，请通过懒猫部署参数改密。应用配置只存 PBKDF2 哈希，部署参数和环境变量仍需保密。

设置向导提供“启用自动登录”开关，默认开启，通过 `simple-inject-password` 使用同一组部署凭据填写并提交登录页。启用时不声明公开路径，必须先通过懒猫身份认证，避免向匿名访问者暴露凭据；关闭后保留原先的手动登录入口。不添加文件选择器拦截。自动填充和实际多实例挂载仍需在懒猫设备上验证。

## 存储与权限

- 包名保持 `cloud.lazycat.app.qbittorrentee`。
- 单实例保留 `/lzcapp/var/downloads`；多实例改为 `/lzcapp/documents/{{ .S.DeployUID }}/<download_dir_name>`。目录名不能含路径分隔符，也不能是 `.` 或 `..`。
- `/config` 始终使用 `/lzcapp/var/config`，保留 `document.private` 权限，删除不再使用的旧文稿权限和媒体访问开关。
- 使用 `RUN_AS_ROOT=true` 兼容模式，不使用 `run_as`，不设置 PUID/PGID，关闭两项自动 chown。root 不能绕过宿主机文稿授权。
- 旧版缺少用户 UID 层，可能在容器启动前报挂载路径 `operation not permitted`，无法靠镜像内 chmod/chown 修复。
- 不自动迁移旧文件，升级前备份并按用户确认下载数据位置。

## 自动更新

每天 23:00 UTC 检查带 `-r1` 后缀的镜像，四段 Tag（如 `5.2.4.10-r1`）映射为 LazyCat SemVer（`5.2.410`）。发现更新后沿用已有发布流程。复用工作流固定为 `ca-x/lazycat-github-action@v1.3.8`，其中的 Node Actions 已使用 Node.js 24。官方和私有商店开关未改变；本地打包不执行发布或提交审核。

GitHub Secrets：官方商店使用 `LAZYCAT_TOKEN`（或兼容凭据）；喵喵商店使用 `APPSTORE_URL`、`APPSTORE_TOKEN`，可选 `APP_ID` 和 `PRIVATE_STORE_GROUP_CODES`。
