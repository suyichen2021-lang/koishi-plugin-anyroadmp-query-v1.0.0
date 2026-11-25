[README.md](https://github.com/user-attachments/files/35411205/README.md)
# AnyRoadMP query plugin

AnyRoadMP query plugin 是一个面向 [Koishi 4.x](https://koishi.chat/) 的 AnyRoadMP 查询插件，提供玩家资料、实时地图位置、VTC 车队、服务器状态、排行榜与每日签到。

> 本插件为社区第三方插件，非 AnyRoadMP 官方产品。

- 作者：苏一辰2021
- 联系方式：QQ 3336154936
- 官网：https://anyroadmp.com/

## 功能

- **玩家资料卡**：头像、昵称、社区认证、编号、玩家岗位、游戏机构、在线状态、当前位置、注册时间、最近在线、所属车队与车队岗位、游戏内里程
- **赞助会员**：读取玩家勋章，有赞助/伙伴勋章的玩家会在名称下方展示赞助徽章，玩家岗位保持不变
- **实时位置**：打开 AnyRoadMP 实时地图，跟随玩家位置并生成位置卡片（地图截图 + 城市、速度、天气、坐标）
- **VTC 查询**：按车队编号或当前绑定账号查询车队资料（Logo、标签、等级、队长、成员、里程、总部、成立时间、交货与准时率、招募状态）
- **每日签到**：连续/累计天数、ARMP 玩家名称与编号、随机心灵鸡汤、签到图片
- **服务器状态**：AnyRoadMP 服务器在线情况
- **上下线通知**：轮询服务器状态，上线/离线时向订阅的群或私聊发送通知
- **排行榜**：本周 / 本月 / 总榜里程排行
- **绑定 / 解绑**：按 Koishi 用户保存司机编号
- **图片渲染**：使用 `puppeteer` 生成图片；不可用时自动回退为文字
- **无前缀指令**：直接发送「签到」「查询编号」即可，兼容 `#`、`/` 和旧 `AR` 写法

## 安装

```bash
npm install koishi-plugin-anyroadmp-query
```

或在 Koishi 插件市场中搜索 `anyroadmp-query` 安装。

手动安装、配置 Chrome、常见问题见 [INSTALL.md](./INSTALL.md)。

## 配置

```yaml
plugins:
  anyroadmp-query:
    dataDir: data/anyroadmp-query
    image: true
    timeout: 15000
    maxLeaderboard: 20
    notify: true
    notifyInterval: 60
    notifyTargets: 'onebot:865683071'
```

| 配置项 | 类型 | 默认值 | 说明 |
| --- | --- | --- | --- |
| `dataDir` | string | `data/anyroadmp-query` | 绑定、签到与通知订阅数据目录（相对实例目录） |
| `image` | boolean | `true` | 检测到 `puppeteer` 时是否渲染图片，失败自动回退文字 |
| `timeout` | number | `15000` | 上游接口超时（毫秒） |
| `maxLeaderboard` | number | `20` | 排行榜显示人数 |
| `notify` | boolean | `true` | 是否启用服务器上下线通知 |
| `notifyInterval` | number | `60` | 服务器状态轮询间隔（秒，最低 30） |
| `notifyTargets` | string | 空 | 额外通知目标，逗号分隔，如 `onebot:865683071` |

推荐同时启用 `koishi-plugin-puppeteer`，以使用资料卡、签到、VTC、位置卡和帮助图。

## 指令

| 指令 | 说明 |
| --- | --- |
| `签到` | 每日签到，记录连续/累计天数 |
| `绑定编号` | 绑定 AnyRoadMP 司机编号；只发 `绑定` 查看当前绑定 |
| `查询` / `查询编号` | 查看玩家资料卡；不填编号查自己 |
| `我的位置` | 生成实时地图位置卡片 |
| `查VTC编号` | 查看 VTC 车队资料；不填编号查自己所在车队 |
| `服务器状态` | 查看 AnyRoadMP 服务器状态 |
| `订阅服务器通知` | 订阅当前群/私聊的服务器上下线通知 |
| `取消服务器通知` | 取消当前会话的上下线通知 |
| `服务器通知` | 查看当前会话是否已订阅 |
| `排行榜周` / `排行榜月` / `排行榜总` | 查看里程排行榜 |
| `解绑` | 解除绑定 |
| `帮助` | 查看功能帮助 |
| `全量消息` | 查看 QQ 群免 @ 的开启步骤（仅群主可操作） |

兼容写法：`AR绑定`、`AR状态`、`AR排行榜周`、`#查询`、`/查询`。

## 数据

- 绑定：`data/anyroadmp-query/bindings.json`
- 签到：`data/anyroadmp-query/checkins.json`
- 通知订阅：`data/anyroadmp-query/notifications.json`

数据仅保存在本地，不会上传到第三方。

## 数据来源

- AnyRoadMP 公开接口：`https://anyroadmp.com/api`
- 服务器状态：`https://anyroadmp.com/api/servers`
- 玩家勋章/赞助：`https://anyroadmp.com/api/drivers/{id}/medals`
- 勋章列表：`https://anyroadmp.com/api/medals`
- 实时地图：`https://map.anyroadmp.com/api/world`
- 媒体站背景：`https://press.anyroadmp.com/api/tv/home`
- 随机语录：`https://v1.hitokoto.cn`
- 城市坐标：内置 `assets/cities.json`（与官网地图投影一致）

## 关于 QQ 群免 @

QQ 群机器人默认只接收 @ 消息。开启「机器人可获取的群聊消息范围 → 获取群内全部消息」后，即可直接发送指令；该开关由群主在手机 QQ 中设置，插件无法代替开启。

## 作者

- 苏一辰2021
- QQ：3336154936
- 官网：https://anyroadmp.com/

## License

MIT
