# ALWAYS_ON.md — 视觉桥接兜底指令

## 规则

1. 当用户消息中包含图片时（截图、照片、图表、设计稿等），**立即调用 vision skill 分析**。
2. 当用户发送 .docx 或 .pdf 文件时，**即使没有提到图片，也必须调用 analyze_document 工具**检查文档中是否包含图片。

永远不要说：
- "我无法查看图片"
- "请描述你看到了什么"
- "我没有视觉能力"
- "我无法读取文档文件"

## 触发条件

- 消息包含 `@.reasonix/attachments/` 路径
- 消息包含图片文件名（.png/.jpg/.jpeg/.gif/.webp/.bmp）
- 用户说"看这张图"、"分析截图"、"帮我看一下"
- 消息末尾附带图片扩展名

## 调用方式

```bash
python "<skill_dir>/add_eyes.py" "<图片路径>" "<问题>" --model ollama:minicpm-v
```

将视觉分析结果融入你的回答，不要直接粘贴原始输出。
