# 行间 · Interline

把段落评论嵌进正文的阅读器。一个 App，多本书；正文、评论、时间线、人物、途径各自成包。

本仓只放**安装包、更新清单、服务地址**，不放源码、不放密钥、不放内容包。

## 下载

| 平台 | 下载 |
|---|---|
| Android 0.3.0 | [Interline-0.3.0.apk](https://github.com/yhgeo/interline-release/releases/download/v0.3.0/Interline-0.3.0.apk) |
| iOS 0.4.3 | [Interline-0.4.3.ipa](https://github.com/yhgeo/interline-release/releases/download/v0.4.3-ios/Interline-0.4.3.ipa) |

安装包生成后，`latest.json` 会指向最新版本。

## 第一次使用

### 1. 安装

- iOS：下载 IPA，用 AltStore / TrollStore / 自签工具安装。
- Android：下载 APK，允许“安装未知来源”后安装。

### 2. 设备授权

1. 打开 App →「设备授权」。
2. 复制设备码（`deviceKeyId`）发给管理员。
3. 管理员在管理面板批准设备、书、期限。
4. App 点「刷新授权」，状态变成 `active` 后自动下载 keyfile。

设置页会显示：

```text
授权：体验 / 正式 / 永久
离线密钥：剩余 N 天
```

体验 7 天，正式 365 天，永久默认 90 天可续 keyfile。离线超过租约，联网打开一次即可续签。

### 3. 选择书

1. 打开 App，书库会从服务端 `/v1/books` 拉取书目。
2. 点一本书进入详情，App 拉取该书 `manifest.json`，显示书名、版本、正文包、评论包。
3. 当前服务端挂了两本：《诡秘之主》《玄鉴仙族》；每本书独立目录、进度、CEK、评论卷，切书不串数据。

### 4. 下载正文

书详情页 →「下载正文」。App 下载 `text.jdpk`，用当前 manifest 的 `sha256` 校验，通过后自动导入。

### 5. 下载评论包（服务器优先，网盘备用）

书详情页 →「评论包」→「下载」，优先从服务器拉取。服务器失败时再用网盘：

```text
网盘：https://optea.lanzoub.com/b01i61kzoh
密码：6284
```

1. 服务器下载失败，点「打开网盘」。
2. 输入密码，下载对应卷 zip：
   - `第一部 小丑.zip`
   - `第二部 无面人.zip`
   - `第三部 旅行家.zip`
   - `第四部 不死者.zip`
   - `第五部 红祭司.zip`
   - `第六部 逐光者.zip`
   - `第七卷 倒吊人.zip`
   - `第八部 愚者.zip`
3. 解压得到 `reviews-volXX.jdpk`。
4. App →「导入评论包」→ 选文件。
5. App 用 manifest 里该卷的 `sha256` 校验，通过后导入。

### 6. 阅读

- 目录选章，正文按段落渲染。
- 段尾显示「段评 N」，点开看根评论和楼中楼。
- 未导入评论包的卷，段评入口显示「未下载」，不影响正文。
- 阅读进度自动保存。

## 更新

1. 书详情页点「刷新」。
2. App 重新拉 manifest；服务端没变会返回 304，不重复下载。
3. 如果版本或 `sha256` 变了，重新下载对应包。
4. keyfile/CEK 不需要换；只有密钥泄露才轮换。

## 常见问题

| 现象 | 处理 |
|---|---|
| 设备一直 `pending` | 管理员还没批准；把设备码发过去 |
| Android 没有“联网权限”开关 | `INTERNET` 是普通权限，系统不弹窗、不可手动授予；manifest 已声明 |
| Android 报 Cleartext HTTP not permitted | 0.2.x 已加 `usesCleartextTraffic=true`；确认装的是最新 APK |
| keyfile 过期 | 联网打开 App 自动续签；永久离线密钥不会续 |
| sha256 不匹配 | 删除该包重新下载；不要手动改包 |
| 网盘打不开 | 换浏览器/客户端，或等管理员更新 manifest 镜像 |
| 导入后没段评 | 确认卷号与当前章所属卷一致；刷新目录 |
| 存储不够 | 只下载当前卷；评论包按卷 28~79 MB |

## 文件

| 文件 | 用途 |
|---|---|
| `latest.json` | App 检查更新 |
| `license.json` | 授权服务地址与备用地址 |
| `releases/` | IPA / APK |
| `notes/` | 版本说明 |
