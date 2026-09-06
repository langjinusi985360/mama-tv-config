# mama-tv-config — 「妈妈影视」接口配置托管

本仓库是「妈妈影视」App（基于开源 FongMi/TV 定制）的**默认接口配置托管仓库**。

App 内置的默认地址指向：

```
https://cdn.jsdelivr.net/gh/langjinusi985360/mama-tv-config@main/tvbox.json
```

## 为什么要托管一份副本？

- 主源失效时，**只需更新本仓库的 `tvbox.json`**，妈妈端 App 打开即自动生效，无需重新安装 APK。
- `tvbox.json` 当前内容取自 kstore 接口（2026-09-06 验证可用，98 个站点 + 2 个直播）。

## 换源操作（维护手册）

1. 当确认当前源失效（App 打开无内容/报错），从 `backup/` 里选一个验证可用的备份源。
2. 把备份源 JSON 里的**相对路径改成绝对路径**（`./xxx` → `源站域名/xxx`），
   覆盖本仓库 `tvbox.json`（可通过 GitHub 网页编辑或 API 更新）。
3. jsDelivr 有缓存，改动后用 purge 接口刷新：
   `https://purge.jsdelivr.net/gh/langjinusi985360/mama-tv-config@main/tvbox.json`
4. 妈妈端重新打开 App 即可。

## 备份源（2026-09-06 验证）

| 文件 | 原始地址 | 状态 |
|---|---|---|
| backup/fmys.abs.json | http://fmys.top/fmys.json | ✅ 200, 82 站点 |
| backup/jundie.abs.json | http://home.jundie.top:81/top98.json | ✅ 200, 24 站点+直播 |

其他候选（未做绝对化改写）：`https://9280.kstore.vip/newwex.json`（当前主源来源）
