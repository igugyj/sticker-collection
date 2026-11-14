# emoCut 切图工具 模块

## 职责

交互式把一张大图切成 rows×cols 小图，支持重叠像素与保持小图宽高比。命令行工具，无 GUI。

## 关键文件

| 文件路径 | 作用 |
|---|---|
| `emoCut.py` | 工具本体（411 行，依赖 pillow） |
| `requirements.txt` | `pillow==12.0.0`（唯一依赖来源） |

## 使用流程（实测 grep 归纳）

1. `python emoCut.py`
2. 提示输入图片路径（自动去两端引号；可选列目录文件供选择）
3. 确认图片 → 输入 rows、cols（校验 >0）
4. 可选重叠像素 `overlap`（默认 0，非负校验）、是否保持宽高比 `keep_aspect_ratio`（默认否）
5. 可选自定义输出目录（直接回车用默认）
6. 确认切割 → 输出 + 可选打开输出目录

核心函数：`split_image(image_path, rows, cols, output_dir=None, overlap=0, keep_aspect_ratio=False)`

## 数据流

```
大图 + rows×cols [+overlap] [+keep_aspect_ratio]
  → 默认目录 <图片名>_split_{rows}x{cols}/（与图片同目录）
  → 小图 tile_{行:03d}_{列:03d}_{宽}x{高}.png（1 起始行/列 + 实际像素尺寸）
  → info.txt（记录 rows/cols/overlap/keep_aspect_ratio/小图尺寸等参数）
```

实现细节（源码实测，55-111 行）：

- **切分尺寸**：`tile_width = width // cols`、`tile_height = height // rows`
- **keep_aspect_ratio**：开启时 `tile_height = int(tile_width / (width/height))`，即按原图宽高比反推小图高（否则直接整除平分）
- **overlap**：每块向四周扩展 overlap 像素并 clamp 到边界——`left = max(0, j*tile_width - overlap)`、`right = min(width, (j+1)*tile_width + overlap)`（行方向同理）；`cols==1` 或 `rows==1` 时该方向取整幅（无重叠效果）
- **tile 命名**：`tile_{i+1:03d}_{j+1:03d}_{right-left}x{lower-upper}.png`（含 overlap 后的实际裁切尺寸），统一存 PNG
- **尺寸校验**：`tile_width <= 0 or tile_height <= 0` 时提示图片太小并返回 False

## 依赖关系

- 依赖：pillow（PIL.Image）
- 被依赖：无（独立工具）

## 变更记录索引

- 2025-06-01：docs/.ai 初始化（本卡建立）
- 2026-11-14：按源码核实行数（411）、tile 命名与宽高比/重叠实现，移除过时 TODO
