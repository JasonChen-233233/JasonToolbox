# 杰哥电商设计工具箱 — 服务端配置仓库

此仓库用于存储杰哥电商设计工具箱客户端所需的配置文件，通过 jsdelivr CDN 加速访问。

## 仓库结构

```
config/
├── version.json            # 版本信息（最新版本号、最低版本、更新说明）
├── authorized-codes.json   # 授权码列表（开发者生成后写入）
├── announcement.json       # 公告内容
└── ad.json                 # 广告配置
```

## 客户端访问方式

客户端通过 jsdelivr CDN 访问配置文件（国内可直连）：

- 主源：`https://cdn.jsdelivr.net/gh/{用户名}/{仓库名}@main/config/{文件名}.json`
- 备用源：`https://raw.githubusercontent.com/{用户名}/{仓库名}/main/config/{文件名}.json`

## 各文件说明

### version.json — 版本信息

| 字段 | 说明 |
|------|------|
| latest_version | 最新版本号（如 1.0.1） |
| min_version | 最低支持版本（低于此版本强制更新） |
| force_update | 是否强制更新 |
| download_url | 更新包下载地址（GitHub Release） |
| release_notes | 更新说明 |

### authorized-codes.json — 授权码列表

由 `gen_auth_code.py` 自动写入，格式：

```json
[
  {
    "device_id": "88106B93C1F0687B",
    "expire_date": "2026-12-31",
    "activate_code": "2026-12-31|88106B93C1F0687B|A3F2B1C8",
    "activated_at": "2026-07-19"
  }
]
```

### announcement.json — 公告内容

由 `announcement_editor.py` 编辑保存。

### ad.json — 广告配置

由开发者手动编辑。

## 使用流程

1. 开发者在本地使用 `gen_auth_code.py` 生成授权码 → 自动写入 `authorized-codes.json`
2. 开发者在本地使用 `announcement_editor.py` 编辑公告 → 自动写入 `announcement.json`
3. 开发者将变更推送到 GitHub：`git push origin main`
4. 客户端自动从 jsdelivr CDN 拉取最新配置（10-15 分钟缓存延迟）

## 注意事项

- jsdelivr CDN 有 10-15 分钟缓存，推送后不会立即生效
- 如需立即生效，可访问 `https://purge.jsdelivr.net/gh/{用户名}/{仓库名}@main/config/{文件名}.json` 刷新缓存
- `authorized-codes.json` 包含授权信息，建议仓库设为 Private（jsdelivr 支持私有仓库但需配置 token）
