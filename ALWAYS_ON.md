# ALWAYS_ON.md — 视觉桥接永久指令

## 核心规则

**你必须使用 vision skill 分析所有图片和视频。永远不要说"我看不见图片"或"请描述你看到了什么"。**

## 触发条件

当以下任一情况发生时，**立即调用 vision skill**：

1. 用户粘贴/拖放图片
2. 消息中包含图片路径（如 `@.reasonix/attachments/xxx.png`）
3. 消息中包含图片文件名（如 `screenshot.png`、`image.jpg`）
4. 用户说"看这张图"、"分析截图"、"帮我看看"等
5. 消息末尾附带图片扩展名（`.png/.jpg/.jpeg/.gif/.webp/.bmp`）

## 执行流程

### 步骤 1：检测图片

收到用户消息后，立即检测是否包含图片：

```bash
python "<skill_dir>/add_eyes.py" --auto-route "<用户消息>"
```

如果返回 `needs_vision: true`，继续步骤 2。

### 步骤 2：调用视觉分析

```bash
python "<skill_dir>/add_eyes.py" "<图片路径>" "<问题>" --model ollama:minicpm-v --context "<对话上下文>"
```

### 步骤 3：融入回答

将视觉分析结果**融入你的回答**，不要直接粘贴原始输出。

## 禁止行为

- ❌ 不要说"我看不见图片"
- ❌ 不要说"请描述你看到了什么"
- ❌ 不要说"我无法查看图片"
- ❌ 不要忽略图片附件
- ❌ 不要只回复文字而不分析图片

## 示例

### ✅ 正确做法

```
用户：帮我看看这个截图
你：[调用 vision skill] → "这是一个登录页面，有用户名和密码输入框..."
你：这是登录页面的截图，主要包含用户名/密码输入框和登录按钮。
```

### ❌ 错误做法

```
用户：帮我看看这个截图
你：抱歉，我无法查看图片。请描述一下图片内容。
```

## 支持的视觉后端

| 后端 | 类型 | 需要 Key |
|------|------|---------|
| `ollama:minicpm-v` | 本地 | 无需 Key |
| `ollama:llava` | 本地 | 无需 Key |
| `ollama:moondream` | 本地 | 无需 Key |
| `mimo-v2.5` | 云端 | `MIMO_API_KEY` |
| `gpt-4o` | 云端 | `OPENAI_API_KEY` |
| `claude-3-5-sonnet` | 云端 | `ANTHROPIC_API_KEY` |

## 故障排除

### vision skill 调用失败

1. 检查 Ollama 是否运行：`ollama list`
2. 检查 API Key 是否设置：`echo $env:MIMO_API_KEY`
3. 尝试其他后端：`--model gpt-4o`
4. 使用 OCR 降级：`--ocr`

### 图片路径找不到

1. 检查 `.reasonix/attachments/` 目录
2. 使用绝对路径
3. 让用户提供图片路径

## 更新日志

- v1.0.0：初始版本，支持 Always-on 模式
