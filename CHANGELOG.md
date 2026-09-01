# Changelog

## v1.0.0

- 六步专家工作流：资料收集 → 大纲策划 → 补充确认 → 策划稿 → 设计稿 → 自动导入 PPTX
- 双路径触发：只给主题（Path A）/ 给了资料（Path B，飞书/本地文件）
- SVG 中间态：策划稿（灰度 SVG）+ 设计稿（Bento Grid 彩色 SVG），两层分离
- PPT-safe SVG 约束：禁用 filter / gradient / defs / CSS style / opacity，纯色 + 模拟阴影
- BOM 编码修正：PowerPoint 导入含中文 SVG 必须 BOM (EF BB BF)，用 WriteAllText 代替 write 工具
- 金字塔大纲 + 逐页审核（全部认可 or 逐页编辑）
- 5 种风格配色（科技/商务/学术/创意/极简）
- PowerShell COM 自动导入 PPTX
- 中英双语 README