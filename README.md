# DSH Operation Log（操作日志 / 交接记录协议）

> 中文 | [English](README.en.md)

> 多个 AI 会话共用一个工作区时，怎么让后来者 30 秒知道“现在在做什么、上一个人改了什么、怎么回滚”，
> 同时**不把历史全塞进上下文**。
>
> 非官方 · 模板可直接复制 · 文档 CC BY 4.0

## 核心设计：三层文件

```text
<workspace>/
├─ 当前状态.md          ← 一页看板（只写“现在”）
├─ 交接记录.md          ← 最近 4~10 条（新的在最上面）
└─ 交接归档/
   ├─ 2026-10上旬.md    ← 更早历史（原样保存）
   └─ 2026-11.md
```

| 文件 | 读的时机 | 写什么 |
|---|---|---|
| `当前状态.md` | 每次开工（30 秒） | 进行中项目、下一步、待确认风险 |
| `交接记录.md` | 开工 + 收工 | 目标/改动/验证/回滚/下一步 |
| `交接归档/` | 查旧账时 | 原样保留的历史条目 |

## 仓库内容

| 内容 | 说明 |
|---|---|
| [docs/operation-log.md](docs/operation-log.md) | 协议：条目模板、轮换归档、五条硬规则、落地步骤、与记忆插件的区别 |
| [examples/templates/handover-log.md](examples/templates/handover-log.md) | 交接记录条目模板 |
| [examples/templates/current-status.md](examples/templates/current-status.md) | 当前状态一页看板模板 |
| [examples/templates/AGENTS-snippet.md](examples/templates/AGENTS-snippet.md) | 加到 `AGENTS.md` 的协作规则片段 |

## 五条硬规则（真实踩坑换来的）

1. 最新条目放**最上面**；
2. **禁止“整读再整写”**（大文件 read 会截断，曾一次丢 9 条）；
3. 同一文件**同一时刻只让一个会话写**；
4. 细节外置：长日志/表格放项目文件夹，交接只留结论 + 路径；
5. 不在交接记录里写密钥/个人数据。

## 快速开始

1. 复制模板到你的工作区；
2. 把 `AGENTS-snippet.md` 加到 `AGENTS.md`；
3. 每次收工在 `交接记录.md` 顶部加一条（≤15 行）；
4. 满 10 条后归档到 `交接归档/YYYY-MM.md`，并更新索引。

## 与“记忆插件”的区别

- 操作日志：**协作交接**（谁改了什么、怎么回滚）；
- 自动记忆插件：**让 AI 自动记住**偏好/事实；
- 记录路由器：**按需查档案**（见姊妹仓库 `dsh-records-router`）。

三者可以共存。

## 许可证

文档 CC BY 4.0；示例代码 MIT（[LICENSE](LICENSE)）。非官方；详见 [LEGAL.md](LEGAL.md)。

---

*English summary: an operation-log/handover protocol for multiple AI sessions sharing one workspace — a one-page status board, a rolling handover log, and monthly archives, with templates and five hard-won rules. Docs CC BY 4.0.*
