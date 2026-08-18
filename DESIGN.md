# Studio Design Notes

## 定位

Studio 是面向内容运营和创作者工作流的静态原型，当前由 `index.html`、`content.html`、`edit.html`、`edit2.html` 四个页面组成。设计系统不再只描述抽象组件，而是服务于这四个页面的统一视觉、深色模式、导航行为、内容列表、专辑编辑、单集编辑和字幕管理。

当前实现以深色模式为默认体验，同时保留浅色主题。所有主题差异通过 CSS Variables 和 `<html data-theme>` 驱动，不复制 DOM，不反转图片、Logo、头像或视频内容。

## 页面范围

- `index.html`：Studio 首页 / Dashboard，展示全局工作台框架、顶部导航、侧导航和欢迎区。
- `content.html`：内容管理页，包含内容类型 tab、筛选器、搜索、内容列表、分页和行级操作。
- `edit.html`：专辑详情编辑页，包含面包屑、页面操作、基础信息、封面、多语言编辑、发行设置、右侧预览和剧集信息。
- `edit2.html`：单集详情编辑页，继承专辑编辑页的结构，并增加 Episode tab、字幕上传、字幕语言添加和字幕编辑器。

## 设计目标

- 清晰：页面结构应让运营人员快速判断当前位置、内容状态和可执行操作。
- 稳定：顶部导航、侧导航、表单控件、标签、弹窗和列表状态跨页面保持一致。
- 高效：后台工作台信息密度适中，支持扫描、筛选、编辑、保存和发布。
- 贴近业务：文案和示例围绕内容、剧集、封面、字幕、多语言、发布状态、付费状态和地区发行展开。

## 字体

- 全局字体统一使用 `Inter`。
- 字体栈以 `"Inter var", "Inter", "PingFang SC", "Microsoft YaHei", Arial, sans-serif` 为准。
- 不使用 `SF Pro`、`system-ui`、`-apple-system`、`BlinkMacSystemFont` 等会回退到苹果系统字体的别名。
- 常规界面文字、导航项、tab、按钮和表单标签统一使用 `font-weight: 500`。
- 标题不使用负字距，`letter-spacing` 保持 `0`。

## 主题 Token

浅色主题基础值：

- `--color-bg-page: #fafbfc`
- `--color-bg-header: #ffffff`
- `--color-bg-surface: #ffffff`
- `--color-bg-elevated: #ffffff`
- `--color-bg-subtle: #f6f7f8`
- `--color-bg-selected: #e8ebee`
- `--color-text-primary: #0b0d0f`
- `--color-text-secondary: #71717a`
- `--color-text-muted: #52525b`
- `--color-text-disabled: #a1a1aa`
- `--color-text-inverse: #ffffff`
- `--color-border-default: #edf0f2`
- `--color-border-subtle: #d4d7dc`
- `--color-brand-primary: #00d157`
- `--color-brand-hover: #00bd4f`
- `--color-brand-active: #00a846`
- `--color-status-success-bg: #eef8f2`
- `--color-mask: rgba(11, 13, 15, 0.72)`
- `--color-shadow: rgba(20, 23, 26, 0.08)`

深色主题基础值：

- `--color-bg-page: #111417`
- `--color-bg-header: #171b1f`
- `--color-bg-surface: #1c2228`
- `--color-bg-elevated: #20272e`
- `--color-bg-subtle: #252d35`
- `--color-bg-selected: #2a353f`
- `--color-text-primary: #f4f7f8`
- `--color-text-secondary: #aab4bf`
- `--color-text-muted: #c7d0d8`
- `--color-text-disabled: #68737f`
- `--color-text-inverse: #08110d`
- `--color-border-default: #2d3741`
- `--color-border-subtle: #3a4652`
- `--color-brand-primary: #00d157`
- `--color-brand-hover: #12e06b`
- `--color-brand-active: #73f2a0`
- `--color-status-success-bg: rgba(0, 209, 87, 0.14)`
- `--color-mask: rgba(0, 0, 0, 0.66)`
- `--color-shadow: rgba(0, 0, 0, 0.34)`

语义别名：

- `--brand` 映射品牌主色。
- `--brand-600` 映射品牌激活色。
- `--text` 映射主文字。
- `--muted` 映射辅助文字。
- `--line` 映射默认分割线。
- `--surface` 映射主要承载面。
- `--page` 映射页面背景。
- `--nav-hover` 映射导航 hover 背景。
- `--nav-active` 映射导航选中背景。
- `--radius: 6px` 是默认圆角。

## 深色模式

- 首次访问默认深色模式，根节点为 `<html data-theme="dark">`。
- 每页 head 内置主题初始化脚本，读取 `localStorage.studio-theme`，避免先闪浅色再切深色。
- 用户手动切换后写入 `studio-theme`，刷新和跨页面跳转时保持选择。
- 深色页面使用深灰层级，不使用纯黑作为页面大面积背景。
- 顶栏使用 `--color-bg-header`，侧栏和弹窗使用 `--color-bg-elevated`，卡片和输入控件使用 `--color-bg-surface`。
- hover 使用 `--color-bg-subtle`，选中态使用 `--color-bg-selected`。
- 品牌绿不做反相，主按钮、选中 checkbox、Episode 编辑入口和成功状态继续使用品牌色。
- 深色模式不默认使用液态玻璃、模糊、反色或高饱和装饰；需要质感变化时必须保证文字对比、滚动性能和布局稳定。

## 全局顶部导航

- 顶部栏高度 `64px`，固定在视口顶部，`z-index: 100`。
- 顶栏左右 padding 桌面端 `24px`，移动端 `16px`。
- 左侧品牌区由 `logo.png` 和 `Studio` 文本组成，Logo 当前显示高度为 `20px`。
- 右侧操作区依次包含语言切换、主题切换、账号入口，间距 `16px`。
- 图标按钮尺寸 `36px × 36px`，圆角 `6px`，默认透明，hover 显示弱背景。
- 语言菜单和账号菜单都是相对触发器的 absolute 浮层，展开不改变页面布局。
- 主题按钮浅色模式显示月亮图标，深色模式显示太阳图标。
- 主题切换 tooltip 只在 hover 时出现；点击切换后按钮主动失焦，减少重复提示。
- 所有顶栏控件必须提供 `aria-label`；语言和账号按钮用 `aria-expanded` 同步展开状态。

## 全局侧导航

- 侧栏位于顶栏下方，桌面端宽度 `240px`，固定到视口底部。
- 侧栏 padding 为 `20px 16px`，背景为 `--surface`，右侧使用 `1px solid var(--line)`。
- 顶部固定展示 `Upload` 主按钮，按钮高度不低于 `42px`，按钮与导航列表间距 `24px`。
- 导航项包含 `Dashboard`、`Content`、`Analytics`、`Subtitles`，每项包含图标和文本。
- 首页额外在侧栏底部展示 `Settings` 入口。
- 当前页面使用 `aria-current="page"` 标记，并使用中性选中背景，不使用品牌绿色作为侧栏选中底色。
- 导航项 hover 和 active 使用中性背景；active 可有轻微 `translateY(1px)` 反馈。
- 移动端侧栏转为顶部横向导航，位于顶栏下方，导航列表横向滚动。

## 页面布局

- `body` 默认 `overflow: hidden`，页面滚动由 `.workspace` 或内部滚动区承载。
- `.shell` 桌面端从顶栏和侧栏后开始布局，基础 padding 为 `64px 0 0 240px`。
- `.workspace` 占满剩余高度并独立滚动。
- 首页工作区 padding 为 `12px 28px 28px`。
- Content 页工作区 padding 为 `20px var(--workspace-x) 28px`，默认 `--workspace-x: 28px`。
- 编辑页 `.workspace-inner` 使用 `28px 0 120px`，内容间距 `24px`。
- 移动端改为页面整体滚动，`.shell` 顶部为顶栏和横向侧栏让位。

## Index 页面

- Index 是最轻量的工作台首页，只展示全局框架和欢迎内容。
- 主内容包含 eyebrow `Dashboard`、标题 `Welcome back to Studio` 和一段摘要。
- 标题最大宽度 `720px`，桌面端字号 `32px/40px`，移动端 `26px/34px`。
- 首页应优先作为进入其它业务页的 shell，不增加营销式 hero、装饰卡片或额外大图。

## Content 页面

- Content 页是内容运营列表页，左侧全局导航中 `Content` 为当前项。
- 顶部内容工具栏 `.content-toolbar-frame` sticky 到工作区顶部，保持类型 tab 和筛选区可见。
- Content menu 高度 `48px`，横向展示 `All`、`Short`、`ComicDrama`、`Movie`、`Drama`、`Anime`；中文对应 `全部`、`短剧`、`漫剧`、`电影`、`电视剧`、`动漫`。
- Content menu 选中项通过品牌绿色 `2px` 下划线表达，未选中项使用辅助文字。
- 筛选区包含 Free/Paid、Publication、Search。下拉浮层相对筛选按钮定位，默认收起。
- 发布状态 filter 的按钮本身中，选中 `Online / Offline / Draft` 后，状态值区域最小宽度 `64px`，状态 pill 在值区域内水平居中。
- 搜索框宽度 `300px`，在筛选区靠右；有内容时显示清除按钮。
- 内容列表使用独立 card，表头 sticky，列表横向滚动时首列 sticky。
- 列表列结构为内容名称、免费/付费状态、发布状态、更新时间、操作。
- 内容缩略图尺寸 `128px`，比例 `16:9`；Short、ComicDrama、Movie、Drama、Anime 使用不同渐变占位。
- 草稿内容的缩略图上层遮罩为 `rgba(0, 0, 0, 0.8)`。
- Draft 行的 Free/Paid Status 显示 `-`，发布状态显示 Draft pill。
- 行级操作包括 `Edit` 和删除图标按钮；Edit 链接进入 `edit.html` 或后续详情页。
- 分页包含范围文本、页码、上一页/下一页、每页条数下拉。

## Edit Album 页面

- `edit.html` 用于编辑专辑级内容，标题为 `Edit Album details` / `编辑专辑详情`。
- 顶部使用面包屑：`Content > 当前内容 > Edit Album details`。
- 页面标题行左侧包含标题和内容类型 tag，右侧包含 `Undo Changes`、`Save`、`Publish/Republish`、More 图标按钮。
- 页面主体为两栏布局：左侧编辑表单 `minmax(0, 2fr)`，右侧预览栏 `minmax(320px, 1fr)`，间距 `20px`。
- 表单 section、预览卡片和剧集卡片使用 `1px` 边框、`6px` 圆角和轻阴影。
- 表单 section padding 为 `20px`，字段间距 `18px 16px`，两列栅格，full 字段跨两列。
- 基础信息包含 Title、Description、Cover 等必填字段。
- Title 和 Description 有字符数限制、错误提示和多语言编辑入口。
- Cover 支持 `16:9 Landscape`、`3:4 Portrait`、`1:1 Square` 三种预览。
- 封面 hover 时显示半透明黑色操作层，提供 `Re-upload` 和 `Title wrap`。
- Title wrap 面板可取消、恢复默认、应用到当前、应用到全部。
- Distribution 区包含 Free/Paid、Publication、Visibility、地区和标签类选择。
- 右侧预览卡展示封面、标题、Content ID、类型、格式、发布状态等信息。
- 剧集信息卡展示 Episode 列表，可展开预览并进入单集编辑。

## Edit Episode 页面

- `edit2.html` 用于编辑单集详情，标题随 episode 参数更新，例如 `Edit Episode 01 details`。
- 面包屑包含返回 `Edit Album details` 的链接。
- 页面顶部增加 Episode tabs，使用与 Content menu 相同的横向 tab 语言，单个 tab 高度 `44px`。
- Episode tabs 支持点击切换、键盘左右切换，并同步 URL 参数。
- 基础信息和封面编辑沿用专辑页模式，但作用域是当前单集。
- 单集页面增加 Subtitles section。
- 字幕区顶部包含 Add subtitle 和 Upload subtitle 操作。
- 已生成字幕以 subtitle card 展示，包含语言、来源状态和 Edit 操作。
- 上传文件列表展示文件名、状态、重试和移除；状态包括 loading、success、error。
- 字幕编辑弹窗宽度可达 `1040px`，分为视频预览和字幕列表编辑两栏。
- 视频预览保持 `16:9`，字幕预览 caption 叠加在视频底部，背景为半透明黑色。
- 字幕列表项包含时间段、文本和编辑态 textarea；当前播放片段使用选中态背景。

## 表单与输入

- 基础输入、选择器、textarea 均使用 `1px solid var(--line)`、`6px` 圆角和 `--surface` 背景。
- 单行输入和选择器高度 `38px`，左右 padding `11px`。
- textarea 最小高度 `96px`，可垂直 resize，并为字符统计预留底部空间。
- focus 态只改变边框为品牌绿，不叠加外发光或 box-shadow。
- 必填字段用 label 后的红色 `*` 标记。
- 错误态边框使用 `#d92d20`，错误文字字号 `12px/16px`，靠近对应字段展示。
- 禁用按钮使用弱背景、默认边框和禁用文字色，鼠标为 `not-allowed`。

## 按钮

- 主按钮用于 Upload、Publish、Republish、Confirm、Apply current 等关键提交行为。
- 主按钮背景为品牌绿，hover 使用 `--color-brand-hover`。
- 次级按钮默认使用 surface 背景和轻描边，hover 使用 subtle 背景。
- 图标按钮优先只显示图标，尺寸通常为 `34px` 或 `36px`。
- 按钮文案保持短促明确，避免解释功能本身。
- 同一操作区内按钮间距通常为 `8px`。

## 标签与状态

内容类型 tag：

- Short：浅色 `#e8f8ef / #00a846`，深色 `rgba(0, 209, 87, 0.16) / #73f2a0`。
- ComicDrama：浅色紫色系，深色 `rgba(178, 123, 209, 0.18) / #d9b2ef`。
- Movie：浅色蓝色系，深色 `rgba(96, 165, 250, 0.16) / #a9d0ff`。
- Drama：浅色橙色系，深色 `rgba(245, 158, 11, 0.15) / #f3b95f`。
- Anime：浅色蓝灰系，深色 `rgba(148, 163, 184, 0.16) / #c5ced9`。

发布状态 pill：

- Online / Publish：无底色，图标和文字使用品牌绿。
- Offline：无底色，图标和文字使用橙色提示色。
- Draft：无底色，图标和文字使用中性辅助色。
- 状态 pill 使用 `inline-flex`，图标 `16px`，图文间距 `4px`，字号 `12px/16px`。

## 封面与媒体

- 封面和视频内容不随主题做 filter、invert 或整体透明度处理。
- 缩略图、封面、poster 通过容器背景、边框和 overlay 表达状态。
- 封面标题叠加在底部，使用白色文字和文字阴影保障可读性。
- 封面底部默认有从透明到黑色的渐变，hover 操作层使用半透明黑。
- 草稿列表缩略图应明显不可发布，当前使用 80% 黑色遮罩。
- 视频字幕预览 caption 使用半透明黑底和白字，不遮挡控制条。

## 浮层与弹窗

- 下拉菜单、账号菜单、筛选菜单、分页条数菜单、select 菜单均使用 absolute 浮层。
- 浮层 `z-index` 高于页面内容，展开不改变触发控件所在区域高度。
- 点击触发器和浮层外部区域后自动关闭。
- Modal backdrop 固定全屏，`z-index: 120`，深色模式使用 `--color-mask`。
- Translation modal 宽度 `min(720px, 100%)`，最大高度 `min(760px, calc(100vh - 48px))`。
- Subtitle edit modal 宽度 `min(1040px, 100%)`。
- Modal header 高度不低于 `64px`，标题 `18px/26px`。
- 弹窗内容区独立滚动，页面底层不应发生布局跳动。

## 多语言与本地状态

- 页面文案由 `messages.en` 和 `messages.zh` 管理。
- 可翻译文本使用 `data-i18n`；ARIA 文案使用 `data-i18n-aria-label`；placeholder 使用 `data-i18n-placeholder`。
- 语言选择写入 `localStorage.studio-language`，并跨页面恢复。
- 主题选择写入 `localStorage.studio-theme`，并跨页面恢复。
- 语言切换后需要同步 document title、菜单选中态、筛选文案、列表文案、弹窗文案和主题按钮 tooltip。

## 滚动与响应式

- 顶栏和侧栏固定，主工作区独立滚动。
- 横向 tab、内容列表、字幕列表和弹窗列表应保留滚动能力。
- 滚动条使用细滚动条；轨道透明，滑块为中性色。
- 宽屏下编辑页保持两栏，右侧预览栏 sticky 或稳定停靠。
- 中等屏幕下编辑页可压缩两栏比例。
- 平板和移动端下编辑页转为单列，预览栏置于表单后。
- 移动端 tab 和导航允许横向滚动，避免文字换行导致高度跳动。

## 可访问性

- 当前页面导航项必须使用 `aria-current="page"`。
- Tab 类控件使用 `role="tab"`、`aria-selected` 和可键盘访问逻辑。
- 下拉触发器使用 `aria-haspopup` 与 `aria-expanded`。
- 弹窗使用 `role="dialog"` 和 `aria-modal="true"`，关闭按钮必须有可翻译 `aria-label`。
- 图标按钮必须有 `aria-label`，纯装饰图标使用 `aria-hidden="true"`。
- focus 态必须可见，且不依赖 box-shadow；以品牌绿边框为主。

## 内容文案

- UI 文案以英文为默认，中文作为语言切换版本。
- 操作文案使用动词或短命令，例如 `Upload`、`Save`、`Publish`、`Edit`、`Delete`。
- 状态文案保持名词化，例如 `Online`、`Offline`、`Draft`、`Platform generated`。
- 页面标题直接说明当前任务，例如 `Edit Album details`、`Edit Episode 01 details`。
- 不在界面中加入解释产品功能、快捷键或视觉规则的大段说明。

## 扩展原则

- 新页面优先复用现有 topbar、sidebar、theme、language、account menu 结构。
- 新增主题色时先扩展语义 token，再使用到组件，避免局部硬编码。
- 新增列表页时沿用 Content 页的 sticky toolbar、筛选浮层、列表 card 和分页模型。
- 新增编辑页时沿用编辑页的面包屑、标题操作区、两栏布局、form section、preview card 和 modal 模型。
- 新增媒体状态时只调整容器、边框、标签和遮罩，不直接修改媒体资产本身。
- 所有改动需同时考虑深色模式、浅色模式、中文文案、英文文案和移动端断点。
