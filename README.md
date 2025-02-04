<p align="center">
  <a href="https://nonebot.dev/"><img src="https://raw.githubusercontent.com/nonebot/adapter-qq/master/assets/logo.png" width="200" height="200" alt="nonebot-adapter-qq"></a>
</p>

<div align="center">

# NoneBot-Adapter-QQ

_✨ QQ 协议适配 ✨_

</div>

## 配置

修改 NoneBot 配置文件 `.env` 或者 `.env.*`。

### Driver

参考 [driver](https://nonebot.dev/docs/appendices/config#driver) 配置项，添加 `HTTPClient` 和 `WebSocketClient` 支持。

如：

```dotenv
DRIVER=~httpx+~websockets
DRIVER=~aiohttp
```

### QQ_IS_SANDBOX

是否为沙盒模式，默认为 `False`。

```dotenv
QQ_IS_SANDBOX=true
```

### QQ_BOTS

配置机器人帐号 `id` `token` `secret`，intent 需要根据机器人类型以及需要的事件进行配置。

#### Intent

以下为所有 Intent 配置项以及默认值：

```json
{
  "guilds": true,
  "guild_members": true,
  "guild_messages": false,
  "guild_message_reactions": true,
  "direct_message": false,
  "open_forum_event": false,
  "audio_live_member": false,
  "c2c_group_at_messages": false,
  "interaction": false,
  "message_audit": true,
  "forum_event": false,
  "audio_action": false,
  "at_messages": true
}
```

#### 示例

私域频道机器人示例

```dotenv
QQ_BOTS='
[
  {
    "id": "xxx",
    "token": "xxx",
    "secret": "xxx",
    "intent": {
      "guild_messages": true,
      "at_messages": false
    }
  }
]
'
```

公域群机器人示例

```dotenv
QQ_BOTS='
[
  {
    "id": "xxx",
    "token": "xxx",
    "secret": "xxx",
    "intent": {
      "c2c_group_at_messages": true
    }
  }
]
'
```

### 关于IP白名单

目前新上线机器人将强制使用ip白名单，如需使用代理ip可配置 `proxy` , 驱动器选择 `aiohttp`

公域群机器人使用代理示例

```dotenv
QQ_BOTS='
[
  {
    "id": "xxx",
    "token": "xxx",
    "secret": "xxx",
    "intent": {
      "c2c_group_at_messages": true
    },
    "proxy": "xxx",
  }
]
'
```

#### 2024.3 新增消息撤回

`bot.delete_user_messages` `bot.delete_group_messages` `bot.delete_dms_messages` `bot.delete_channel_messages`


#### 2024.5 新增回调回复

```python
@on_type(InteractionCreateEvent, block=False).handle()
async def _(bot: Bot, event: InteractionCreateEvent):
    # 响应回调事件
    out = await bot.put_interaction(interaction_id=event.id, code=0)
    logger.debug(f"响应事件：{out}")
    if isinstance(bot, QQBot) and event.group_openid:
        out = await bot.send_to_group(
            group_openid=event.group_openid,
            message="这是个回调消息",
            event_id=event.__id__,
            )
        logger.debug(f"回复事件：{out}")
```

#### 安装此Fork

`pip install git+https://github.com/Ainierheokami/adapter-qq`

或

`pip install git+https://github.com/Ainierheokami/adapter-qq.git@add-proxy`
