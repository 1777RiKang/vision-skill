# Changelog

All notable changes to this project will be documented in this file.

## [2.1.0] - 2026-06-18

### Added
- MCP Server (`mcp_server.py`) — 模型自动发现视觉工具，主动调用
  - `see_image` 工具：分析图片返回文字描述
  - `detect_backends` 工具：检测可用视觉后端（支持 JSON/TEXT 格式）
  - 零依赖 MCP 实现（不需要 `pip install mcp`）
  - `add-eyes-mcp` CLI 入口
- `--context` 参数：MCP `see_image` 工具支持上下文增强（利用 smart_question）
- README 新增 MCP 章节（配置示例、工具说明、MCP vs CLI 对比）

### Changed
- ALWAYS_ON.md 精简为 700 bytes 兜底指令（有 MCP 的平台不需要）
- 版本号统一为 2.1.0（pyproject.toml / mcp_server.py / add_eyes.py）

## [2.0.0] - 2026-06-17

### Added
- **MCP 服务器架构**：双保险策略（MCP 为主，系统提示词为辅）
- **pyproject.toml**：支持 `pip install`，`add-eyes` CLI 命令
- **smart_question 重写**：增强而非替换，用户问题保持最前面
  - 多类别组合（error + code + UI 同时命中）
  - 自动提取错误信息和技术栈名称
  - 中英文自适应
- **smart_focus 智能聚焦**：根据上下文自动判断聚焦区域
  - 直接关键词匹配（左上/底部/中间）
  - 上下文隐含推断（导航栏→顶部，报错→底部）
  - 自动聚焦时打印 `[auto-focus]` 提示
  - 支持 `--focus none` 禁用自动聚焦
- **区域聚焦分析**：`--region` / `--focus` 参数
  - 关键词聚焦：左上/右下/中间/顶部/底部
  - 坐标裁剪：`--region "100,200,300,250"` 或百分比 `"0,0,0.5,1"`
  - 裁剪后保留原格式（JPEG/WebP/PNG），避免膨胀
- **自动检测后端**：`--detect-backends` 参数
- **自动路由**：`--auto-route` 参数
- **Always-on 模式**：`ALWAYS_ON.md` 系统提示词注入
- **13 个视觉后端**：MiMo / GPT-4o / Claude / Gemini / Ollama
- **本地模型支持**：Ollama (MiniCPM-V / LLaVA / Moondream)
- **OCR 降级**：easyocr → pytesseract → Pillow 三级降级
- **自动重试**：指数退避 + 429/5xx 重试
- **HTTP 安全警告**：非 HTTPS 连接时打印警告
- **API Key 打码**：verbose 模式和 detect-backends 输出中隐藏敏感信息
- **多平台支持**：Reasonix / Claude Code / Cursor / GitHub Copilot

### Changed
- 重命名 `mimo_vision.py` → `add_eyes.py`
- 重命名项目 `mimo-vision-skill` → `add-eyes-skills`
- "能力融合" → "视觉桥接"（宣传诚实度）
- "零依赖" → "核心零依赖（高级功能需可选依赖）"
- chcp 65001 → `sys.stdout.reconfigure()`（更 Pythonic）

### Fixed
- Ollama 完整模型名 base_url 错误（`_find_preset` 前缀匹配）
- auto_detect_backends() 环境变量名推导错误
- stdin + OCR 崩溃（ocr_fallback 支持 b64 参数）
- auto_route() 仅支持 Reasonix（增加通用路径匹配）
- 网络错误不触发 OCR 降级
- Gemini API Key 在 URL 中泄露
- 重试时复用已失败的 Request 对象
- 5xx HTTP 错误不享受重试
- auto_route() 问句/上下文颠倒
- smart_question 关键词重叠（"问题"同时在 error 和通用场景）
- auto_route 视觉关键词误报率高
- _find_preset startswith 误匹配（gpt-4o-mini 匹配 gpt-4o）
- 裁剪后强制转 PNG 导致文件膨胀
- 文档残留（旧项目名、旧宣传语）
- 错误响应泄露 API 原始数据

## [1.0.0] - 2026-06-16

### Added
- 初始版本：纯文本模型的视觉桥接
- 支持 MiMo / GPT-4o / Claude / Gemini 云端 API
- 基本图片分析功能
- Reasonix / Claude Code / Cursor / GitHub Copilot 多平台支持
