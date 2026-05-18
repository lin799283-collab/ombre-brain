# Ombre Brain — MCP Memory Server

娜宝的记忆系统。基于 [p0luz/ombre-brain](https://github.com/p0luz/ombre-brain)，打了兼容补丁，支持 Operit（Android）一键导入。

## 快速开始（Operit 仓库导入）

1. 在 Operit → 插件 → 添加 → 仓库导入，粘贴此仓库地址
2. Operit 会自动安装依赖（`pip install -r requirements.txt`）
3. 在插件配置里填入环境变量（见下方）

## 环境变量

复制 `.env.example` 为 `.env`，填入真实值：

| 变量 | 说明 | 示例 |
|------|------|------|
| `OMBRE_API_KEY` | DeepSeek/SiliconFlow API Key（脱水压缩必需） | `sk-xxx` |
| `OMBRE_TRANSPORT` | 传输方式 | `streamable-http` |
| `OMBRE_PORT` | 监听端口 | `8000` |
| `OMBRE_BUCKETS_DIR` | 记忆桶存储路径 | `./buckets` |

**最小配置（Operit 环境变量）：**
```
OMBRE_API_KEY=sk-你的key
OMBRE_TRANSPORT=streamable-http
OMBRE_PORT=8000
```

## 手动运行

```bash
pip install -r requirements.txt
OMBRE_API_KEY=sk-xxx OMBRE_TRANSPORT=streamable-http python server.py
```

## 已应用的补丁

| 补丁 | 原因 |
|------|------|
| Content-Type 验证绕过 | mcp 1.27.1 新增严格验证，部分客户端（kelivo 等）省略该头部 → 400 报错 |
| `stateless_http=True` | 每次 POST 创建新会话，避免会话复用导致的连接问题 |
| GET /mcp SSE keepalive | 防止 Cloudflare/代理空闲超时断连 |
| `_ContentTypePatch` ASGI wrapper | 双重保险：对缺少 Content-Type 的 POST 自动注入 |

## MCP 工具

- `breath` — 浮现未解决记忆 / 按关键词检索
- `hold` — 存储单条记忆
- `grow` — 日记归档，自动拆分多桶
- `trace` — 修改元数据 / 删除
- `pulse` — 系统状态 + 桶列表
- `dream` — 返回最近桶供自省

## 连接地址

运行后 MCP endpoint：`http://localhost:8000/mcp`（Operit 内网），或配置外网隧道后填外网地址。
