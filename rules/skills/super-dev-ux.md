# super-dev-ux — 感官级 UI/UX 验证

## 定位
提供**感官级体验**的 UI/UX 质量保障系统，覆盖视觉、交互、无障碍、响应式全维度验证，交付"让人愉悦"的工业艺术品。

## 何时激活
- 创建或修改 UI 组件/页面
- 前端代码变更
- 设计评审
- UX 走查
- 用户说"美化"、"优化体验"、"加动画"

## 感官级验证维度

### 🎨 视觉维度
```
- 色彩一致性与对比度
- 排版节奏与可读性
- 间距系统一致性
- 动效流畅度与时机
- 暗黑模式适配
- 视觉层次清晰度
```

### 🖐️ 交互维度
```
- 操作反馈及时性（<100ms）
- 过渡动画自然度（200-500ms）
- 加载状态与骨架屏
- 空状态与错误状态设计
- 手势与键盘操作支持
- 触控目标尺寸（≥44px）
```

### ♿ 无障碍维度
```
- 语义化 HTML
- ARIA 标签完整性
- 键盘导航可行性
- 屏幕阅读器兼容
- 焦点管理
- 色彩对比度（WCAG AA/AAA）
```

### 📱 响应式维度
```
- 移动端（320px+）
- 平板（768px+）
- 桌面（1024px+）
- 宽屏（1440px+）
- 横屏/竖屏适配
- 安全区域适配
```

## UX 检查清单

### 通用检查项
```
□ 加载状态是否友好（skeleton/spinner）？
□ 错误状态是否有清晰提示和恢复路径？
□ 空状态是否有引导性内容？
□ 操作是否有即时反馈？
□ 可点击元素是否有 hover/active 状态？
□ 表单是否有实时校验和错误提示？
□ 是否支持 Command/Ctrl+K 快捷操作？
```

### 组件级检查
```
Button:
□ 有明确的视觉层级（Primary/Secondary/Ghost）
□ 有 loading 状态
□ 有 disabled 状态
□ 触控面积 ≥ 44×44px

Form:
□ 输入框有清晰的 label
□ 校验错误即时显示
□ 提交按钮防重复点击
□ 必填项有明确标识

Modal/Dialog:
□ 打开时有焦点捕获
□ ESC 键可关闭
□ 背景有遮罩层
□ 内容不超出视口

Table/List:
□ 加载中有骨架屏
□ 空列表有引导
□ 长列表有虚拟滚动
□ 列可排序/筛选
```

### 页面级检查
```
□ 首屏加载时间 < 2s（LCP）
□ 交互延迟 < 100ms（INP）
□ 无布局偏移（CLS < 0.1）
□ 路由切换有过渡动画
□ 页面标题随路由更新
□ 404/500 页面设计完整
```

## CSS/样式规范

### 设计系统变量
```css
:root {
  /* 色彩 */
  --color-primary: #...;
  --color-primary-hover: #...;
  --color-text: #...;
  --color-text-secondary: #...;
  --color-bg: #...;
  --color-surface: #...;
  --color-border: #...;
  --color-error: #...;
  --color-success: #...;

  /* 间距 */
  --space-xs: 4px;
  --space-sm: 8px;
  --space-md: 16px;
  --space-lg: 24px;
  --space-xl: 32px;
  --space-2xl: 48px;

  /* 排版 */
  --font-sans: 'Inter', system-ui, sans-serif;
  --font-mono: 'JetBrains Mono', monospace;
  --text-xs: 0.75rem;
  --text-sm: 0.875rem;
  --text-base: 1rem;
  --text-lg: 1.125rem;
  --text-xl: 1.25rem;
  --text-2xl: 1.5rem;

  /* 圆角 */
  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 12px;
  --radius-full: 9999px;

  /* 阴影 */
  --shadow-sm: 0 1px 2px rgba(0,0,0,0.05);
  --shadow-md: 0 4px 6px rgba(0,0,0,0.07);
  --shadow-lg: 0 10px 25px rgba(0,0,0,0.1);

  /* 动效 */
  --ease-out: cubic-bezier(0.16, 1, 0.3, 1);
  --duration-fast: 150ms;
  --duration-normal: 300ms;
  --duration-slow: 500ms;
}
```

## Tailwind CSS 最佳实践

```
布局：flex, grid, gap-*, container, mx-auto
间距：p-*, m-*, space-y-*, space-x-*
颜色：bg-*, text-*, border-*, ring-*
圆角：rounded-sm/md/lg/xl/2xl/full
阴影：shadow-sm/md/lg/xl/2xl
动效：transition-all, duration-*, ease-*
响应式：sm:, md:, lg:, xl:, 2xl:
暗黑：dark:
状态：hover:, focus:, active:, disabled:
```

## 动效设计

### 何时使用动效
```
✓ 页面/元素出现（fade + slide）
✓ 页面/元素消失（fade + scale）
✓ 状态切换（smooth transition）
✓ 列表增删（FLIP 动画）
✓ 用户操作反馈（ripple/pulse）

✗ 纯装饰性动画（分散注意力）
✗ 超过 500ms 的转场
✗ 不尊重 prefers-reduced-motion
```

### 动效 Token
```css
/* prefers-reduced-motion 适配 */
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}
```

## 与上下游交互

```
上游：requirements（体验需求） + architect（技术选型）
下游：tdd（组件测试验证）
关联：security（XSS/CSRF防护不影响UX）
```

## 反模式警示

| 反模式 | 表现 | 纠正 |
|--------|------|------|
| 盲目跟风 | 不加甄别地用流行设计 | 回归用户需求和场景 |
| 过度动效 | 动画影响性能和可用性 | 动效服务于功能，不喧宾夺主 |
| 忽略无障碍 | 只考虑视觉完美 | 从设计阶段就纳入 a11y |
| 响应式补丁 | 桌面做完再打移动端补丁 | Mobile First |
