# lessons.md - 禁止行为与避坑指南

> 此文档记录开发过程中遇到的错误和教训，确保未来不再犯同样的错误。
> 每次遇到错误时，必须更新此文档。

---

## 分类索引

| 分类 | 说明 |
|------|------|
| [通用错误](#通用错误) | 跨项目通用的错误教训 |
| [语言/框架特定](#语言框架特定) | 特定技术栈的避坑指南 |
| [工具/环境](#工具环境) | 开发工具和环境相关问题 |

---

## 通用错误

*（暂无记录，随着开发积累逐步填充）*

---

## 语言/框架特定

### SQLite 并发访问

```python
# ❌ 错误：多线程/异步环境下可能导致锁定超时
conn = sqlite3.connect('db.sqlite3')

# ✅ 正确：设置超时参数
conn = sqlite3.connect('db.sqlite3', timeout=30)

# ✅ 更好：使用 WAL 模式允许并发读写
conn.execute("PRAGMA journal_mode=WAL")
```
**场景**：asyncio 线程、多进程、Streamlit 并发请求

---

### Python 3.12 datetime 兼容性

```python
# ❌ 错误：Python 3.12 中直接传 datetime 对象有弃用警告
cursor.execute('... VALUES (?, ?)', (id, datetime.now()))

# ✅ 正确：转为 ISO 格式字符串
cursor.execute('... VALUES (?, ?)', (id, datetime.now().isoformat()))
```
**原因**：Python 3.12 的 sqlite3 模块弃用了默认的 datetime 适配器

---

## 工具/环境

*（暂无记录，随着开发积累逐步填充）*

---

## 更新日志

| 日期 | 更新内容 |
|------|----------|
| 2026-03-08 | 初始化 lessons.md，从 CLAUDE.md 迁移避坑指南 |
