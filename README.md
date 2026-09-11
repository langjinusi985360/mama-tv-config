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

## 2026-09-11 直连源修订

原默认源把推荐内容优先导向夸克/115 网盘线路，未登录网盘 Cookie 时会出现“115 Cookie 未配置或已失效”“还没有配置夸克 Cookie”。为妈妈端默认体验，`tvbox.json` 已改为：

1. 前排加入 6 个不依赖网盘登录的采集直连源，默认首页为 `暴风资源`。
2. 依次保留 `360资源`、`ikun资源`、`无尽资源`、`魔都资源`、`采集丨影视` 作为备用。
3. 原 98 个扩展站点完整保留，但整体后移；夸克/115/Emby/AList 等网盘或自建服务不再作为默认入口。

### 来源与复核方法

- 主参考仓库：[`tushen6/Tomorrow`](https://github.com/tushen6/Tomorrow)（2026-09-11 复核为 2,269 stars，2026-09-09 仍有推送），采用其 `caiji.json` 中的 CMS 直连接口。
- 辅助参考：[`qist/tvbox`](https://github.com/qist/tvbox)、[`noimank/tvbox`](https://github.com/noimank/tvbox) 和 [`cluntop/tvbox`](https://github.com/cluntop/tvbox)，用于交叉确认采集接口的活跃度和结构。
- 2026-09-11 实测：使用 App 相同的 `wd` 搜索和 `ac=detail` 详情参数，以“兰香如故”“剑来第二季”为样本；保留的直连源均能返回详情，并解析到可请求的 `.m3u8` 地址。
- `1080资源` 与 `神马云` 虽然搜索和详情正常，但其视频 CDN 在当前网络直接请求返回 403，因此已从默认前排移除，避免妈妈点开后失败。
- 网盘源仍可手动选择；若要使用，需要先在“配置中心/我的网盘”完成对应网盘登录。该操作不应作为妈妈端默认流程。
