# Design System Design Notes

> 当前默认主题：IQ 默认主题。品牌色为 iQIYI 标准绿色；`brand-rgb` 为 `28, 199, 73`。本文件是全局设计规范的唯一入口。

## 定位

Design System 服务于企业应用平台的组件规范、交互示例和视觉一致性沉淀。当前站点采用静态 HTML 实现，适合作为设计验收、前端还原和组件扩展的共同参考。

## 设计目标

- 清晰：组件说明应帮助使用者快速判断何时使用、如何组合、有哪些边界状态。
- 稳定：基础 Token、组件尺寸和状态表达保持一致，降低跨页面理解成本。
- 高效：页面结构服务于后台工作台场景，信息密度适中，便于扫描和对照。
- 贴近业务：示例围绕 Agent 创建、任务配置、店铺运营、素材上传、时间排期等实际场景展开。

## 视觉语言

### 字体

- 全局字体统一使用 `Inter`。
- 字体栈必须以 `Inter`、`Inter var` 开头，中文字体作为后备，例如 `"PingFang SC"`、`"Microsoft YaHei"`。
- 全局字体栈不得显式使用 `SF Pro`，也不得通过 `system-ui`、`-apple-system`、`BlinkMacSystemFont` 等苹果系统字体别名回退到 SF Pro。
- `font-family` 必须保持 `Inter` 在首位。
- Typography 只负责 Font Family、Font Size、Font Weight、Line Height 等排版属性；文字颜色统一由 Text Semantic 管理。
- Font Style 按 `Display → Title → Body → Caption` 组织，共九种固定样式。
- 每种 Font Style 固定组合 Font Size、Weight 与 Line Height，不拆分调用或临时调整。

| Style | Font Size | Weight | Line Height | Usage |
| --- | ---: | ---: | ---: | --- |
| `Display` | `32px` | `600` | `40px` | Large data and prominent display |
| `Title-1` | `24px` | `600` | `32px` | Page title |
| `Title-2` | `20px` | `600` | `28px` | Section title |
| `Title-3` | `16px` | `600` | `24px` | Card and subsection title |
| `Body-Large` | `16px` | `400` | `24px` | Large body text |
| `Body` | `14px` | `400` | `22px` | Default body text |
| `Body-Medium` | `14px` | `500` | `22px` | Emphasized body text |
| `Caption` | `12px` | `400` | `20px` | Secondary information |
| `Caption-Medium` | `12px` | `500` | `20px` | Emphasized secondary information |

#### Font Style 使用规则

- 页面和组件必须先判断内容角色，再选择对应 Font Style，不按 HTML 标签或视觉大小机械选型。
- `Display` 仅用于大型数据和最突出的展示内容，不替代常规页面标题。
- 页面标题、章节标题、卡片及子章节标题依次使用 `Title-1`、`Title-2`、`Title-3`。
- 默认正文使用 `Body`；需要更大阅读尺寸时使用 `Body-Large`；正文强调使用 `Body-Medium`。
- 次要信息使用 `Caption`，需要强调的次要信息使用 `Caption-Medium`。
- Font Style 仅负责字号、字重和行高；文字颜色统一调用 `color-text-*` Semantic Color。
- 不因单个页面的特殊需求新增字号、字重、行高或 Font Style。
- 组件如需覆盖 Font Style，必须有明确且可复用的组件级需求。

AI Coding 选择顺序：

```text
判断文字内容角色
↓
选择 Display / Title / Body / Caption 分组
↓
选择对应 Font Style
↓
组件最终渲染
```

禁止写法：这个文字看起来比较重要，所以临时使用 18px。

推荐写法：这是 Section Title，所以使用 `Title-2`。

### 图标

产品内所有 Icon 必须遵循统一 Icon System。Icon 是语义型 UI 元素，不作为装饰图形使用。

核心原则：

- 优先复用已有 Icon；不存在准确或等价资源时才允许创建新 Icon。
- 相同语义必须使用相同 Icon，语义清晰度优先于视觉新颖度。
- 禁止 AI 自由创造新的 Icon Style。
- Icon 的选择、检索、复用、绘制、尺寸、Stroke、Geometry、Color、SVG 输出和校验必须遵循 [IQB Icon Skill](skills/icon/SKILL.md)。
- Component Contract 只负责 Icon 的位置、显示尺寸、间距与状态；Icon 原始绘制仍使用统一的 `24 × 24` viewBox。

AI Coding 在任何页面、组件或 Pattern 中识别到 Icon 需求时，必须调用 `skills/icon/SKILL.md`，不得跳过检索直接生成 SVG。

### 圆角

默认圆角主题使用 `radius-style-default`，基础圆角收敛为 Small、Default、Large、XLarge 和 Full 五档。`Default` 是默认圆角；胶囊与圆形元素使用 `Full` 保持形态稳定。

| Token | 值 | 语义 | 典型组件 |
| --- | --- | --- | --- |
| `radius-small` | `4px` | 小圆角，用于紧凑控件和轻量标识。 | Checkbox、Tag、代码片段 |
| `radius-default` | `6px` | 默认圆角，用于高频操作与导航元素。 | Button、Side nav item |
| `radius-large` | `8px` | 大圆角，用于输入区域和内容容器。 | Input、Card、Collapse |
| `radius-xlarge` | `12px` | 超大圆角，用于浮层、大型容器和媒体承载区域。 | Dropdown、Popover、Modal、媒体预览 |
| `radius-full` | `9999px` | 完整圆角，元素应呈现胶囊或圆形。 | Avatar、Icon Button、状态点 |

使用规则：

- 组件优先引用圆角 token，不在局部样式里临时写散落数值。
- 默认圆角主题统一由 `radius-style-default` 写入上述基础 Token；组件根据语义选择尺寸，不统一使用单一数值。
- `radius-full` 只用于需要胶囊或圆形轮廓的元素，不用于普通卡片。
- 拼接边、分段控件内部边界等必须保持直角的结构位置可直接使用 `0`；`0` 不属于基础圆角档位。
- 切换或应用默认圆角主题只改变 Token 映射，不得改变组件尺寸、布局结构和交互范围。

### 色彩

颜色体系采用三层调用关系：

```text
Primitive Color → Semantic Color → Component Token
```

Primitive Color 是基础色板，Semantic Color 是用途命名，Component Token 是组件私有映射。页面和组件开发必须优先根据用途选择 Semantic Token，不直接根据视觉相似度调用 `brand-5`、`neutral-6` 等 Primitive Token。

#### Primitive Color

- Brand、Success、Danger、Warning、Info 使用 `0–9` 共 10 档色阶，`5` 为默认基准色；Neutral 使用 `0–13` 共 14 档色阶，深色端专门承载多级表面。
- Neutral 采用无色相中性灰，RGB 三通道保持一致，避免偏绿或偏蓝。
- Primitive Color 仅作为基础色板和 Semantic Token 的取值来源。
- Brand 相关色阶通过 `brand-0` 到 `brand-9` 暴露；本文件中的取值固定为当前默认主题。
- Status Color 表达固定状态含义，不随 Brand Theme 改变。
- 遮罩基于 Neutral Primitive Color 与透明度生成，不直接写死 RGBA 色值。

| 色板 | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Brand | `#F0FDF3` | `#DBFDE3` | `#B9F9C9` | `#82F39F` | `#45E36E` | `#1CC749` | `#00AE2E` | `#118430` | `#13682A` | `#125526` |
| Success | `#F0FFF1` | `#C3FACA` | `#93EDA2` | `#67E07F` | `#3FD462` | `#1CC749` | `#0EA13A` | `#057A2C` | `#00541F` | `#002E12` |
| Danger | `#FFF1F0` | `#FFCCC7` | `#FFA39E` | `#FF7875` | `#FF4D4F` | `#F5222D` | `#CF1322` | `#A8071A` | `#820014` | `#5C0011` |
| Warning | `#FFF7E6` | `#FFE7BA` | `#FFD591` | `#FFC069` | `#FFA940` | `#FA8C16` | `#D46B08` | `#AD4E00` | `#873800` | `#612500` |
| Info | `#E6F4FF` | `#BAE0FF` | `#91CAFF` | `#69B1FF` | `#4096FF` | `#1677FF` | `#0958D9` | `#003EB3` | `#002C8C` | `#001D66` |

| Neutral Token | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| HEX | `#FFFFFF` | `#F5F5F5` | `#EAEAEA` | `#DDDDDD` | `#A3A3A3` | `#737373` | `#666666` | `#3D3D3D` | `#292929` | `#1F1F1F` | `#171717` | `#111111` | `#080808` | `#000000` |

#### Categorical Color

Categorical Color 用于分类、属性、业务类型和数据系列，不表达状态，也不跟随 Brand Theme 改变色相。Tag、Chart、Badge、Avatar 和数据可视化应共享这一层，禁止用 Status 或 Text Token 冒充物理色。

| Token | Light | Dark |
| --- | --- | --- |
| `color-category-blue` | `#2563EB` | `#4096FF` |
| `color-category-cyan` | `#0E7490` | `#22D3EE` |
| `color-category-teal` | `#0F766E` | `#2DD4BF` |
| `color-category-green` | `#15803D` | `#4ADE80` |
| `color-category-lime` | `#4D7C0F` | `#A3E635` |
| `color-category-yellow` | `#A16207` | `#FACC15` |
| `color-category-orange` | `#C2410C` | `#FB923C` |
| `color-category-red` | `#DC2626` | `#F87171` |
| `color-category-magenta` | `#A21CAF` | `#E879F9` |
| `color-category-purple` | `#7C3AED` | `#A78BFA` |

使用规则：

- Categorical Color 只表达稳定分类身份；`green ≠ success`、`red ≠ error`、`orange ≠ warning`、`blue ≠ info`。
- Brand 场景继续使用 `color-action-primary`；系统状态继续使用 `color-status-*`。
- Light 与 Dark 可以调整亮度和对比度，但必须保持同一物理色相身份。

#### Semantic Color

核心 Semantic Token 按 Text、Background、Border、Action、Status、Overlay 组织。新增 Semantic Token 必须引用已有 Primitive Token，不在 Semantic 层创建新的 HEX 或 RGBA。

##### Text

| Semantic Token | Light | Dark | 使用场景 |
| --- | --- | --- | --- |
| `color-text-top` | `neutral-12` | `neutral-0` | 最高层级标题、核心数据与最强文字强调 |
| `color-text-primary` | `neutral-10` | `neutral-1` | 标题、关键数据、主要文本 |
| `color-text-default` | `neutral-9` | `neutral-2` | 正文、菜单项、输入内容 |
| `color-text-secondary` | `neutral-7` | `neutral-4` | 说明、辅助信息、Helper Text |
| `color-text-tertiary` | `neutral-6` | `neutral-5` | Placeholder、尾注、低优先级信息 |
| `color-text-disabled` | `neutral-4` | `neutral-6` | 禁用文本与图标 |
| `color-text-inverse` | `neutral-0` | `neutral-12` | 反色文字、深色按钮文字 |
| `color-text-link` | `info-5` | `info-4` | 链接和可点击文本 |
| `color-text-link-hover` | `info-6` | `info-3` | 链接 Hover |
| `color-text-link-disabled` | `neutral-4` | `neutral-6` | 禁用链接 |

文字用途禁止直接使用 `neutral-9`、`neutral-7` 等 Primitive Token 表达，应优先使用 `color-text-*` Semantic Token。

示例：

```text
neutral-9 → color-text-primary → Page Title / Form Label
neutral-7 → color-text-secondary → Description / Helper Text
```

##### Background

| Semantic Token | Light | Dark | 使用场景 |
| --- | --- | --- | --- |
| `color-bg-page` | `neutral-0` | `neutral-12` | 页面画布 |
| `color-bg-container` | `neutral-0` | `neutral-11` | Card / Form / 主要内容容器 |
| `color-bg-elevated` | `neutral-0` | `neutral-10` | Popover / Dropdown / 浮层 |
| `color-bg-overlay-hover` | `neutral-2` | `neutral-8` | Dropdown / Select 等浮层菜单项 Hover |
| `color-bg-subtle` | `neutral-1` | `neutral-9` | 弱化区域和次级背景 |
| `color-bg-hover` | `neutral-1` | `neutral-9` | 中性组件 Hover |
| `color-bg-selected` | `brand-0` | `neutral-8` | Selected / Active 区域 |
| `color-bg-disabled` | `neutral-2` | `neutral-8` | Disabled 控件背景 |
| `color-bg-inverse` | `neutral-9` | `neutral-0` | Tooltip / 反色区域 |

##### Border

| Semantic Token | Light | Dark | 使用场景 |
| --- | --- | --- | --- |
| `color-border-default` | `neutral-3` | `neutral-7` | Input / Select / Card 常规边框 |
| `color-border-subtle` | `neutral-2` | `neutral-8` | Divider / 弱边界，统一为 1px |
| `color-border-strong` | `neutral-4` | `neutral-6` | 强调边界 |
| `color-border-focus` | `brand-5` | `brand-5` | Focus Ring 与聚焦边框 |
| `color-border-disabled` | `neutral-2` | `neutral-8` | Disabled 控件边框 |

示例：

```text
neutral-3 → color-border-default → input-border / select-border / card-border
```

##### Action

Action 类颜色必须使用当前主题的 Brand Color，不直接绑定其他固定颜色。

| Semantic Token | Light | Dark | 使用场景 |
| --- | --- | --- | --- |
| `color-action-primary` | `brand-5` | `brand-5` | Primary Button / Checkbox / Radio / Switch / Slider |
| `color-action-primary-hover` | `brand-6` | `brand-6` | 主要操作 Hover |
| `color-action-primary-bg` | `brand-0` | `brand-8` | 轻量品牌背景和选中态 |
| `color-action-primary-bg-hover` | `brand-1` | `brand-7` | 轻量品牌区域 Hover |
| `color-action-primary-disabled` | `neutral-1` | `neutral-9` | 主要操作 Disabled |
| `color-action-neutral-bg-hover` | `color-text-primary 10%` | `color-text-primary 10%` | 无边框灰色纯文字按钮、ICON 按钮 Hover |

示例：

```text
brand-5 → color-action-primary → button-primary-background
```

##### Status

Status Color 表达固定状态含义，不随 Brand Theme 改变。

| Semantic Token | Light | Dark | 使用场景 |
| --- | --- | --- | --- |
| `color-status-success` | `success-5` | `success-5` | 成功、完成、通过 |
| `color-status-success-border` | `success-5` | `success-5` | 成功状态组件描边 |
| `color-status-success-hover` | `success-6` | `success-6` | 成功状态 Hover |
| `color-status-success-bg` | `success-0` | `success-8` | 成功弱背景 |
| `color-status-success-bg-hover` | `success-1` | `success-7` | 成功弱背景 Hover |
| `color-status-error` | `danger-5` | `danger-5` | 错误、失败、危险操作 |
| `color-status-error-border` | `danger-5` | `danger-5` | 错误状态组件描边 |
| `color-status-error-hover` | `danger-6` | `danger-6` | 错误状态 Hover |
| `color-status-error-bg` | `danger-0` | `danger-8` | 错误弱背景 |
| `color-status-error-bg-hover` | `danger-1` | `danger-7` | 错误弱背景 Hover |
| `color-status-warning` | `warning-5` | `warning-5` | 警告、待处理、中风险提示 |
| `color-status-warning-border` | `warning-5` | `warning-5` | 警告状态组件描边 |
| `color-status-warning-hover` | `warning-6` | `warning-6` | 警告状态 Hover |
| `color-status-warning-bg` | `warning-0` | `warning-8` | 警告弱背景 |
| `color-status-warning-bg-hover` | `warning-1` | `warning-7` | 警告弱背景 Hover |
| `color-status-info` | `info-5` | `info-5` | 信息反馈、进行中、Notification |
| `color-status-info-border` | `info-5` | `info-5` | 信息状态组件描边 |
| `color-status-info-hover` | `info-6` | `info-6` | 信息状态 Hover |
| `color-status-info-bg` | `info-0` | `info-8` | 信息弱背景 |
| `color-status-info-bg-hover` | `info-1` | `info-7` | 信息弱背景 Hover |

示例：

```text
danger-5 → color-status-error → form-error / alert-error / message-error
```

##### Overlay

Overlay 基于 Neutral Primitive Color 与透明度生成，按黑白与强度统一语义命名；亮暗模式使用同一映射。实现时使用 `color-mix(in srgb, var(--neutral-*), transparent)`，不直接写死 RGBA。

| Semantic Token | 取值 | 使用场景 |
| --- | --- | --- |
| `color-overlay-black-strong` | `neutral-10 / 75%` | 显著压低底层内容、集中视觉焦点 |
| `color-overlay-black-default` | `neutral-10 / 50%` | 建立清晰遮挡关系、弱化底层信息 |
| `color-overlay-black-subtle` | `neutral-10 / 35%` | 轻度压暗并保留较多底层信息 |
| `color-overlay-white-strong` | `neutral-0 / 75%` | 在深色或复杂表面形成强提亮覆盖 |
| `color-overlay-white-default` | `neutral-0 / 50%` | 柔化深色表面并建立明亮层次 |
| `color-overlay-white-subtle` | `neutral-0 / 35%` | 轻度提亮或增加通透感 |

#### 使用原则

- 页面和组件优先使用 Semantic Token，不直接调用 Primitive Color。
- 禁止根据 HEX 或 Primitive Token 猜颜色；必须先判断 UI 语义，再查找对应 Semantic Token，最后由 Semantic Token 映射到 Primitive Color。
- Component Token 应优先引用 Semantic Token。
- Brand / Action Semantic Token 使用本文件定义的当前默认主题映射。
- Semantic Token 必须同时定义 Light 与 Dark 映射；模式切换只改变映射，不改变组件调用方式。
- Warning / Error / Info 表达固定状态含义，不随 Brand Theme 改变；成功操作使用 Brand / Action Token，跟随 Brand Theme。
- 不要因为单个组件需求随意增加 Semantic Token；只有出现跨组件、重复使用的颜色语义时才新增。
- 如果现有 Semantic Token 可以表达用途，不创建新的 Token。

### 布局

- 顶部栏高度：`60px`
- 侧边导航宽度：`240px`
- 主内容区采用左侧导航加右侧画布的工作台布局。
- 文档内容以组件说明、示例块、属性表和 Token 表为主要结构。

### 浮层层级

下拉菜单、级联菜单、自动补全候选、日期时间面板、Tooltip、Popover 等浮层必须作为触发控件的 `+1` 层级呈现，不参与原底层 card、表单行或页面区块的高度计算：

- 浮层容器应使用 `position: absolute` 或等价的 portal/overlay 机制，并设置明确的 `z-index`，相对触发控件悬浮展示。
- 触发控件所在 card 的默认高度不得因为浮层展开而增加；展开和收起时，底层布局、相邻 card 与页面滚动位置应保持稳定。
- 文档示例如果需要展示默认打开状态，只能为演示区域预留固定高度或使用独立预览画布，不得依赖浮层内容把 card 撑高。
- 下拉类组件默认应收起；除非组件规范明确说明“默认展开演示”，否则页面加载时不应自动选中或展开。
- 下拉类浮层展开后，点击触发控件和浮层内容以外的页面区域必须自动收起；组件内部点击不应误触发收起。
- 浮层超出容器时优先通过更高层级、裁切规避或 portal 处理，不通过增加父级 padding、margin、min-height 来临时修补。

### 全局侧导航

侧导航是所有 HTML 页面共享的全局结构，新增或调整组件页时必须保持一致：

- 所有导航项和分组文案统一使用英文在前、中文在后，例如 `Styles 全局样式`、`Components 组件总览`、`Data Entry 数据录入`、`AutoComplete 自动补全`。
- 所有页面侧导航宽度、字号、行高和间距保持一致：桌面端侧栏宽度 `240px`，导航项最小高度 `40px`，导航文字字号 `14px`，分组文字字号 `12px`。
- 侧导航中的当前页必须通过 `aria-current="page"` 标记，便于视觉高亮、辅助技术识别和脚本定位。
- 页面加载后，当前选中的导航 tab 应自动滚动到侧导航可视区域中部。桌面端按垂直方向居中；移动端或横向侧导航按横向方向居中。
- 页面布局必须固定在视口高度内，让侧导航自身成为滚动容器；不得依赖 `body` 滚动承载侧导航，否则靠后的 tab 无法稳定自动居中。
- 侧导航列表底部需要保留约半屏高度的滚动缓冲，确保 `Cascader 联级选择` 及其下方 tab 也能滚动到可视区域中部。
- 侧导航滚动条必须与主模板保持一致：使用细滚动条、透明轨道、中性色滑块和稳定 gutter，独立组件页不得使用浏览器默认粗滚动条。
- 首页 `index.html` 的 `Styles` 与 `Components` 使用 `data-page` 切换面板，其他独立组件页使用链接跳转，但展示文案、尺寸和选中态行为必须一致。

### 投影

投影用于表达弱层级、交互悬停、临时浮层和焦点可达性。组件应先判断投影承担的语义，再引用对应 token，不按视觉强弱临时挑选阴影值。

| Token | Light / Dark 值 | 语义 | 典型组件 |
| --- | --- | --- | --- |
| `shadow-surface` | `neutral-10 6% / 48%` | 弱层级投影，用于让静态轻量容器从背景中轻微浮起。 | 低强调卡片、统计卡片、轻量预览容器 |
| `shadow-hover` | `neutral-10 8% / 56%` | 交互悬停投影，用于表达可点击模块被指向或轻微抬升。 | Card hover、列表项 hover、可点击模块 |
| `shadow-overlay` | `0 8px 24px / color-bg-inverse 5%` | 浮层投影，用于临时覆盖在页面之上的面板和菜单。 | Select Dropdown、DatePicker Panel、Popover、Menu |

使用规则：

- 静态容器优先使用 `shadow-surface`，不要用高强度阴影制造层级。
- 交互 hover 只在可点击或可选中模块上使用 `shadow-hover`。
- 下拉、菜单、日期时间面板等临时浮层统一使用 `shadow-overlay`。
- 焦点态必须配合明确边框或状态色，不只依赖阴影变化。

### 聚焦

- Input 与 Input Number 的 hover / focus 边框使用 `color-action-primary`；focus shadow 使用 `shadow-focus`。
- Error 控件默认态仅使用 `color-status-error` 描边；只有控件实际获得焦点时才叠加错误态 focus shadow，禁止用静态类让默认态持续显示外发光。
- 表单内的 Checkbox、Input Number、Time Picker 必须复用对应基础组件的尺寸、状态与 Component Token，不另建视觉相近但行为不同的局部变体。
- 可清除的时间 Trigger 仅在有值且 hover / focus-within 时展示清除操作；清除后回到占位态并派发空值变更。

交互控件需要提供清晰的 hover、focus、active、disabled 和 error 表达，焦点态应稳定可见，避免只依赖颜色变化。

### 滚动条

- 细滚动条宽度：`4px`
- 滚动条轨道：`transparent`
- 滚动条滑块：`var(--color-border-default)`
- 滚动条圆角：`999px`

下拉菜单、浮层列表和组件内部滚动区域默认使用细滚动条。滚动条轨道不应出现底色，避免在轻量浮层中形成额外边界；仅显示中性色滑块，并在需要滚动时提供足够可见性。

## Component Contract 调用规则

所有组件级 AI Coding 契约统一存放在 `contract/` 目录，`design.md` 只维护全局设计原则与调用规则。

调用顺序：

1. 先读取 `design.md`，确定全局 Semantic Token、布局、交互和可访问性规则。
2. 再读取 `contract/README.md`，从索引定位目标组件 Contract。
3. 读取 `contract/<component-name>.md` 的接口、变体、状态矩阵和代码示例。
4. 检查 Contract 指向的实现 Source of Truth；样式实现不得与 Contract 分叉。
5. 生成或修改代码后，按 Contract 的检查清单验证所有状态。

规则优先级：用户当前明确要求 > 目标组件 Contract > `design.md` 全局规则 > 页面示例。若 Contract 与 Source of Truth 不一致，应停止猜测并同步更新二者。

AI Coding 不得跳过目标组件 Contract，不得从截图推断已在 Contract 中定义的颜色、尺寸、间距或状态。新增组件 Contract 必须使用小写 kebab-case 文件名，并登记到 `contract/README.md`。

当前 Contract：

- Button：`contract/button.md`

## 组件组织

当前组件按四类组织：

- 通用：Button
- 布局：Divider、Space、Grid、Layout、Splitter
- 数据录入：Input、InputNumber、Radio、Checkbox、Switch、Select、DatePicker、TimePicker、Upload、Form、AutoComplete、Cascader、Mentions、VerficationCode、Rate、Slider
- 数据展示：Card、Table

完整组件规范建议包含以下章节：

- 何时使用：说明组件适用场景和不适用边界。
- 基础用法：展示默认结构和最常见用法。
- 组合类型或布局：说明复合场景、变体和信息组织方式。
- 状态：覆盖默认、悬停、聚焦、选中、错误、禁用、加载等状态。
- 尺寸：说明小、中、大或业务约定尺寸。
- API：列出属性、说明、类型、默认值。
- 设计 Token：列出组件私有或共享样式变量。

## 交互原则

- 表单反馈应靠近字段，帮助用户快速定位问题。
- 选择类组件应明确当前值、展开状态、选项状态和清除操作。
- 日期与时间组件应同时照顾单值、范围、空状态和错误状态。
- 上传组件应展示文件队列、进度、成功、失败和删除能力。
- Button 基础变体只包含 Primary、Secondary、Brand Outline、Neutral Outline、Neutral Dashed、Text，不提供 Filled 基础变体。Secondary、Neutral Outline（灰色实线）与 Neutral Dashed（灰色虚线）的常态背景统一为 `color-text-primary` 10% 透明混合色，Hover 统一为 20%；Neutral Dashed 保留 `color-border-default` 虚线描边，适合“添加标签”等轻量创建入口。
- Button 的 Secondary 变体统一且仅使用 `spec-btn secondary` 类名，不再提供历史别名。
- Button Success 状态使用 Brand / Action Token 并跟随 Brand Theme：Outline 为 50% 品牌色描边，Hover 为 60% 描边与 10% 背景；Subtle 为 10% 背景，Hover 为 20%；Text Hover 为 10% 背景。透明色必须通过 `color-mix()` 生成。
- 无边框灰色纯文字按钮和 ICON 按钮的 Hover 背景统一使用 `color-action-neutral-bg-hover`，由 `color-mix(in srgb, var(--color-text-primary) 10%, transparent)` 生成；不得直接引用 Primitive 灰阶或复用容器 Hover 色。
- 按钮应根据任务优先级区分主按钮、次按钮、描边按钮、文本按钮、危险按钮和加载状态。

## 内容风格

文案保持简洁、明确、业务化。示例中优先使用 企业应用平台相关对象，例如 Agent、任务、店铺、素材、授权、回调地址、排期和批量操作参数。

## 扩展建议

新增组件时，先补齐组件入口和全局侧导航，并复用统一的侧导航文案顺序、尺寸规则和选中项自动居中逻辑，再补齐规范章节。若组件有复杂交互，应提供可操作示例；若组件仅作为展示规范，也应包含关键状态和 Token 表，确保设计与实现可以对齐。
