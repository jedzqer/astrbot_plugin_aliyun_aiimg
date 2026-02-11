# AstrBot Aliyun Qwen 图像生成插件

> **当前版本**: v2.0

本插件为 AstrBot 接入阿里云通义千问（Qwen）的图像生成能力，支持通过自然语言或指令调用，支持多 Key 轮询。

## 功能特性

- 支持通过 LLM 自然语言调用生成图片
- 支持通过指令 `/qwenimg` 生成图片
- 支持多种图片比例和分辨率
- 支持千问（Qwen-Image）和万相（Wan）两大系列模型
- 支持多 API Key 轮询调用
- 支持自定义负面提示词
- 支持 Prompt 智能改写功能
- 自动清理旧图片，节省存储空间

## 配置

在 AstrBot 的管理面板中配置以下参数：

| 参数 | 说明 | 默认值 |
|------|------|--------|
| `base_url` | 阿里云百炼 API 地址 | `https://dashscope.aliyuncs.com/api/v1` |
| `api_key` | 阿里云百炼 API Key（支持多 Key 轮询） | `[]` |
| `model` | 使用的模型名称 | `qwen-image-plus` |
| `size` | 默认图片大小 | `1024*1024` |
| `negative_prompt` | 负面提示词 | `" "` |
| `prompt_extend` | 开启 Prompt 智能改写 | `true` |
| `watermark` | 添加水印 | `false` |

### 配置说明

- **base_url**: 北京地域使用默认值，新加坡地域使用 `https://dashscope-intl.aliyuncs.com/api/v1`
- **api_key**: 支持配置多个 Key 以实现轮询调用，可有效分摊 API 额度消耗
- **model**: 
  - 千问系列：`qwen-image-max`、`qwen-image-plus`、`qwen-image`（擅长复杂文字渲染）
  - 万相系列：`wan2.6-t2i`、`wan2.5-t2i-preview`、`wan2.2-t2i-flash`、`wanx2.0-t2i-turbo`（擅长写实摄影）
- **prompt_extend**: 开启后会自动优化提示词，提升出图效果，但会增加 3-5 秒耗时
- **negative_prompt**: 描述不希望在画面中出现的内容，留空或单个空格表示不使用

## 阿里云百炼 API Key 获取方法

1. 访问 [阿里云百炼控制台](https://bailian.console.aliyun.com/)
2. 点击右上角头像 → API-KEY 管理
3. 创建新的 API Key 并复制
4. 详细文档：https://help.aliyun.com/zh/model-studio/get-api-key

## 支持的图像尺寸

### 千问（Qwen-Image）系列

仅支持以下 5 种固定的分辨率：

| 比例 | 可用尺寸 |
|------|----------|
| 16:9 | 1664×928 |
| 9:16 | 928×1664 |
| 1:1 | 1328×1328 |
| 4:3 | 1472×1104 |
| 3:4 | 1104×1472 |

### 万相（Wan）系列

支持在 `[512, 1440]` 像素范围内任意组合宽高，总像素不超过 1440×1440。常用分辨率：

| 比例 | 推荐尺寸 |
|------|----------|
| 1:1 | 1024×1024 |
| 16:9 | 1440×810 |
| 9:16 | 810×1440 |
| 4:3 | 1440×1080 |
| 3:4 | 1080×1440 |

## 使用方法

### 指令调用

```
/qwenimg <提示词> [比例]
```

示例：
- `/qwenimg 一个可爱的女孩` (使用默认比例)
- `/qwenimg 一个可爱的女孩 16:9`
- `/qwenimg 赛博朋克风格的城市 9:16`

### 自然语言调用

直接与 bot 对话，例如：
- "帮我画一张小猫的图片"
- "生成一个二次元风格的少女"

## 模型选型建议

- **复杂文字渲染**（如海报、对联）：首选 `qwen-image-max`、`qwen-image-plus`
- **写实场景和摄影风格**：推荐万相模型，如 `wan2.6-t2i`、`wan2.5-t2i-preview`
- **需要自定义输出图像分辨率**：推荐万相模型，如 `wan2.2-t2i-flash`
- **成本敏感，可接受基础质量**：可选择 `wanx2.0-t2i-turbo`

## 注意事项

- 请确保您的阿里云账号有足够的免费额度或已开通按量付费
- 生成的图片会临时保存在 `data/plugins/astrbot_plugin_aliyun_qwen_aiimg/images` 目录下
- 插件会自动清理旧图片，保留最近 50 张，无需手动管理
- `/qwenimg` 命令和 LLM 调用均有 10 秒防抖机制，避免重复请求
- API 返回的图片 URL 有 24 小时有效期，插件会自动下载到本地

## 免费额度

阿里云百炼提供免费额度，具体请参见：https://help.aliyun.com/zh/model-studio/new-free-quota

**🚀 魔改说明**
- 完全迁移到阿里云通义千问（Qwen）图像生成服务
- 支持千问（Qwen-Image）和万相（Wan）两大系列模型
- 使用 DashScope SDK 进行 API 调用
- 新增 `prompt_extend` 配置项，支持 Prompt 智能改写
- 新增 `watermark` 配置项，可选择是否添加水印
- 支持更多图片尺寸和比例

## 相关文档

- [阿里云百炼文生图指南](./Aliyun_Qwen文生图指南.md)
- [阿里云百炼图像编辑指南](./Aliyun_Qwen图像编辑指南.md)
- [官方文档](https://help.aliyun.com/zh/model-studio/text-to-image)

## 常见问题

**Q: 如何获取 API Key？**

A: 访问阿里云百炼控制台，在 API-KEY 管理页面创建新的 API Key。详见：https://help.aliyun.com/zh/model-studio/get-api-key

**Q: 提示 "API Key 无效或已过期" 怎么办？**

A: 请检查配置的 API Key 是否正确，注意北京地域和新加坡地域的 API Key 不同。

**Q: 提示 "输入内容触发了内容安全审核" 怎么办？**

A: 请修改提示词，移除可能违规的内容后重试。

**Q: prompt_extend 参数应该开启还是关闭？**

A: 当输入的 prompt 比较简洁时，建议保持开启（默认）。当 prompt 已经非常详细时，可以关闭以减少响应延迟。

**Q: 如何提升图像中文字的生成效果？**

A: 如果需要在图像中生成清晰、准确的文字，请使用 `qwen-image-max` 或 `qwen-image-plus` 模型。

## 许可证

本项目基于原 [Gitee AI](https://github.com/muyouzhi6/astrbot_plugin_gitee_aiimg) 插件修改而来，感谢原作者的贡献！✨
