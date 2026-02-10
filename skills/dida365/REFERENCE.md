# Dida365 / TickTick 字段格式参考

## 时间格式

### localDateTime
输入格式: `YYYY-MM-DD HH:mm`
- 示例: `"2026-02-10 09:00"`
- 时区: 按配置 `timezone` 解释

### UTC 输出格式
API 返回: `YYYY-MM-DDTHH:mm:ss+0000`
- 示例: `"2026-02-10T01:00:00+0000"`

## Reminders 提醒

格式: `TRIGGER:{ISO8601 duration}`

| 含义 | 格式 | 示例 |
|------|------|------|
| 提前30分钟 | `-PT30M` | `TRIGGER:-PT30M` |
| 提前1小时 | `-PT1H` | `TRIGGER:-PT1H` |
| 准时 | `PT0S` | `TRIGGER:PT0S` |

**注意**: 服务端最多保留5条提醒，超出会被截断

## RepeatFlag 重复规则 (RRULE)

See **[Examples](examples.md)** for complete examples including hourly/minutely repeats.

### 常用规则

| 规则 | 示例 |
|------|------|
| 每 N 分钟 | `RRULE:FREQ=MINUTELY;INTERVAL=30` |
| 每 N 小时 | `RRULE:FREQ=HOURLY;INTERVAL=2` |
| 每天 | `RRULE:FREQ=DAILY;INTERVAL=1` |
| 每周一三五 | `RRULE:FREQ=WEEKLY;BYDAY=MO,WE,FR` |
| 每月1号 | `RRULE:FREQ=MONTHLY;BYMONTHDAY=1` |
| 工作日 | `RRULE:FREQ=WEEKLY;BYDAY=MO,TU,WE,TH,FR` |

### RRULE 完整参数

| 参数 | 说明 | 示例 |
|------|------|------|
| FREQ | 频率 (MINUTELY/HOURLY/DAILY/WEEKLY/MONTHLY/YEARLY) | `FREQ=DAILY` |
| INTERVAL | 间隔 | `INTERVAL=2` 表示每2个单位 |
| BYDAY | 星期几 (MO/TU/WE/TH/FR/SA/SU) | `BYDAY=MO,WE,FR` |
| BYMONTHDAY | 每月几号 | `BYMONTHDAY=1` |
| BYHOUR | 指定小时 | `BYHOUR=9,18` |
| COUNT | 重复次数 | `COUNT=10` |

### 示例

```bash
# 每30分钟提醒一次
RRULE:FREQ=MINUTELY;INTERVAL=30

# 每2小时提醒一次
RRULE:FREQ=HOURLY;INTERVAL=2

# 每天早上9点
RRULE:FREQ=DAILY;BYHOUR=9

# 每周一三五 9:00 和 18:00
RRULE:FREQ=WEEKLY;BYDAY=MO,WE,FR;BYHOUR=9,18
```

## Priority 优先级

| 值 | 含义 |
|----|------|
| 0 | 无优先级（默认） |
| 1 | 低 |
| 2 | 中 |
| 3 | 高 |

## Tags 标签

### 命令行参数
| 参数 | 说明 |
|------|------|
| `--tag <name>` | 添加标签（可重复使用） |
| `--tag-hint <hint>` | 标签提示（帮助自动匹配） |
| `--disable-required-tags` | 禁用自动添加 "cli" 标签 |

### 配置方式
```json
{
  "requiredTags": ["work", "important"],
  "enableRequiredTags": true
}
```

### 规则
- 格式: `string[]`
- 处理: 系统会自动合并用户标签和默认标签
- 去重: 自动去除重复标签
- 标准化: 去除首尾空格，统一小写

## 子任务 (Items)

格式:
```json
{
  "title": "子任务标题",
  "status": 0,
  "sortOrder": 0
}
```

| 字段 | 说明 |
|------|------|
| `title` | 子任务标题（必需） |
| `status` | 0=未完成, 1=已完成 |
| `sortOrder` | 排序顺序 |

## 项目类型 (Project Kind)

| 值 | 说明 |
|----|------|
| `TASK` | 任务型项目 |
| `NOTE` | 笔记型项目 |

## 视图模式 (View Mode)

| 值 | 说明 |
|----|------|
| `list` | 列表视图 |
| `kanban` | 看板视图 |
| `timeline` | 时间线视图 |
