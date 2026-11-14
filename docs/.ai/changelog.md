# 更新日志

本文件记录 sticker-collection 的所有重要变更。**新条目加在顶部**。

---

## [2026-11-14] 文档同步重构（AGENTS.md 重建 + docs/.ai 修正）

- 根目录 `AGENTS.md` 重建（原文件标注 NEED REBUILD）：`emo/` → `lib/`、补仓库/画廊地址、修正 `.gitignore` 现状
- `docs/.ai/` 全量同步改名：`emo/` → `lib/`、项目名 EMO → sticker-collection、本地路径更新为 `F:\Stickers\EMO`
- 清除失效事实：`.gitignore` 的 `**/bilibili超かぐや姫！`（已移除）、`.nomedia.example`（现为空文件）、`docs/ai/emojiMaker.prompt.txt`（已删除）、分类数 41 → 44、Total 1089 → 以 README stats 块为准
- `modules/emoCut.md` 按源码核实 tile 命名与宽高比实现，移除过时 TODO；`modules/gallery.md` 同步新模板标题与 GitHub 链接

## [2026-11-14]

重构仓库名为sticker-collection，重命名`emo`为`lib`

## [2025-06-01]

### 画廊导航改版

- `scripts/template.html` 布局重构：左右两栏 → 上下两块
  - 顶部新增经典导航栏（`header.navbar`）——左标题、右 GitHub 图标链接（fa-brands fa-github）
  - 下方 `.layout` 保持左右两栏：左 `nav` 分类列表、右 `main` 画廊
  - 链接 `https://github.com/igugyj/EMO`，纯图标无文字，`title`/`aria-label` 提示，新窗口打开
  - 移动端媒体查询同步调整（`.layout` 转 column，nav 横置）
  - 重新运行 `python scripts/generate_gallery.py` 生成新 `index.html`
  - JS 的 `nav a` 选择器不受影响（分类列表仍在 `nav` 内）

### 初始记录

- 建立 docs/.ai 结构（Init 协议）
  - 扫描来源：README.md、AGENTS.md、requirements.txt、.gitignore、.nomedia.example、emoCut.py（grep 实测）、scripts/update_stats.py、scripts/generate_gallery.py、scripts/template.html（头部）、.github/workflows/update_stats.yml、.github/deploy-gallery.yml、emo/ 目录树
  - 待验证项：
    - emoCut.py 完整切割行为（重叠/宽高比细节未逐行读全，见 modules/emoCut.md）
    - `docs/ai/emojiMaker.prompt.txt` 内容未读（与文档协议无关，独立文件）
    - index.html 当前是否已生成、内容是否过时（generate_gallery.py 可重跑）
