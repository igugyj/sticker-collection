# 贴纸库内容 模块

## 职责

`lib/` 下每个子目录一个分类，直接放图片即为全部内容。本仓库的主体数据，无代码逻辑（逻辑在本模块，脚本见同目录其他卡）。

## 关键文件

| 文件路径 | 作用 |
|---|---|
| `lib/<category>/` | 分类目录，44 个（文件总数以 README stats 块为准，由 update_stats.py 生成） |
| `README.md` stats 块 | 每个分类的文件计数（由 update_stats.py 生成，勿手改） |
| `.gitignore` | 当前仅忽略 `.venv` / `info.txt` / `index.html`，无分类级忽略 |

## 数据流

```
lib/<cat>/图片 → update_stats.py 扫描统计 → README stats 块
lib/<cat>/图片 → generate_gallery.py 扫描 → index.html 画廊
```

计数忽略规则（update_stats.py 实测）：`.preview` 前缀文件 + `.html/.psd/.txt/.md/.url` 扩展名不计入。

## 依赖关系

- 被 `scripts/update_stats.py`、`scripts/generate_gallery.py` 扫描
- 被 Android EweSticker 等应用直接引用为贴纸源

## 变更记录索引

- 2025-06-01：docs/.ai 初始化（本卡建立）
- 2026-11-14：`emo/` → `lib/` 改名同步；移除已失效的 bilibili gitignore 条目与过时计数
