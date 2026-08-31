# PPT Guider

顶级演示文稿智能体预设（DSH Agent Preset），模拟专业 PPT 设计公司的完整工作流。

## 特色

### 1. 六步专家工作流

```
资料收集 → 大纲策划 → 补充确认 → 策划稿 → 设计稿 → 自动导入 PPTX
```

每一步都有确认关卡，用户可随时回退。

### 2. 双路径触发

- **只给主题** → 自动联网调研 + 逐页搜索补充资料
- **给了资料**（飞书链接 / 本地文件）→ 先读资料，再联网补充，轻量确认

### 3. SVG 中间态（核心创新）

| 阶段 | 输出 | 作用 |
|------|------|------|
| Step 4 策划稿 | 灰度 SVG 线框图 | 结构蓝图，先定版式 |
| Step 5 设计稿 | 彩色 Bento Grid SVG | 最终视觉效果 |

**两层分离**确保结构先行、设计在后，避免 AI 生成 PPT 常见的"设计偏离策划 40%+"问题。

### 4. PPT-safe SVG 约束

设计稿可直接拖入 PowerPoint 2016+，`Convert to Shape` 后保持样式一致。

- ❌ 禁止 `<filter>`、`<defs>`、渐变、CSS `<style>`
- ✅ 纯色填充 + 深色矩形模拟阴影 + 所有样式内联

### 5. 自动导入 PPTX

Step 6 通过 PowerShell COM 将全部 SVG 按序插入 PPTX 文件。用户只需手动 `右键 → Convert to Shape` 即可编辑。

### 6. 金字塔大纲 + 逐页审核

Step 2 用金字塔原理生成结构化大纲，用户可选择"全部认可"或逐页审核编辑。

## 风格配色

| 风格 | 背景 | 主色 | 辅助色 | 强调色 |
|------|------|------|--------|--------|
| 🔵 科技感 | #1a1a2e | #16213e | #0f3460 | #e94560 |
| 🟢 商务 | #f8f9fa | #1a3a5c | #2c5f8a | #d4a853 |
| 🟡 学术 | #ffffff | #333333 | #2563eb | #1e40af |
| 🟣 创意 | #faf5ff | #7c3aed | #db2777 | #f59e0b |
| ⚪ 极简 | #ffffff | #111111 | #666666 | #000000 |

## 已知限制

### SVG → PPT Convert to Shape 不完全

PowerPoint 的 `Convert to Shape` 无法 1:1 还原所有 SVG 特性。
我们做了硬限制（禁用渐变/阴影等），但视觉效果仍有妥协。

**欢迎 Fork 贡献更好的 SVG→PPT 转写方案！** 可行的方向：
- 直接生成原生 PPT 形状（跳过 SVG 中间层）
- 改进 SVG 到 PPT 的映射规则
- 利用 PPT 内置渐变/阴影补全转换后的样式

## 未竟尝试

### 便利贴（Sticky Note）侧边栏 Tab

曾尝试在 Step 2 大纲确认后加入可拖拽排序的便利贴视图，
通过 `ctx.betterSidebar.registerTab()` 挂靠到 dsh-better-sidebar 侧边栏。
基本实现可行但交互体验待打磨，代码在 `pastk-1` 动态插件中保留。

参考实现：[ego-browser](https://github.com/Fisfzy/ego-browser) 的侧边栏 Tab 挂靠模式。

## 安装

```bash
git clone https://github.com/<user>/dsh-ppt-guider.git ~/.dsh/.agent-presets/ppt-guider/
```

重启 DSH，在预设列表中选择 "PPT Guider"。

## 使用

1. 选择 "PPT Guider" 预设
2. 输入主题或提供资料（飞书链接 / 本地文件）
3. 跟随 Agent 的六步工作流完成 PPT

## 协议

MIT