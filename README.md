# qbittorrentee-lzcapp

每天 23:00 UTC 检查 `czyt/qbittorrentee` 的稳定版本。四段镜像 Tag（如 `5.2.1.10`）映射为 LazyCat SemVer（`5.2.110`），发现更新后自动创建版本化 LPK Release，并提交懒猫官方商店和喵喵私有商店。

GitHub Secrets：官方商店使用 `LAZYCAT_TOKEN`（或兼容凭据）；喵喵商店使用 `APPSTORE_URL`、`APPSTORE_TOKEN`，可选 `APP_ID` 和 `PRIVATE_STORE_GROUP_CODES`。
