# CSS 3笔记
## 一、弹性盒布局（Flexbox）
### 1.1 概念
Flexbox 即弹性盒子布局模型，可快速实现元素的对齐、分布和空间分配，是一维单行/单列布局的首选方案。

### 1.2 核心概念
1. **容器与项目**：父元素设置弹性布局后称为**弹性容器**，子元素称为**弹性项目**；由父容器控制子元素的排列规则。
2. **主轴与交叉轴**：主轴是元素排列的主方向，交叉轴是与主轴垂直的方向；默认主轴为水平方向，交叉轴为垂直方向。
   
### 1.3 开启弹性布局
给父容器设置 `display: flex` 即可开启弹性布局，子元素将沿主轴方向排列：
1. 若子元素已设置宽高，则按设定尺寸显示（实际开发推荐明确子元素尺寸）
2. 若子元素未设置尺寸，则默认沿主轴方向拉伸填满容器
3. 若子元素总宽度超过容器宽度，默认会压缩子元素以适配容器，不会自动换行
  
### 1.4 弹性容器（父元素）属性汇总
| 属性 | 作用 | 示例 |
| --- | --- | --- |
| `display: flex` | 定义元素为块级弹性容器 | `.container { display: flex; }` |
| `flex-direction` | 定义主轴方向（项目排列方向） | `.container { flex-direction: row; }` |
| `flex-wrap` | 控制项目是否换行 | `.container { flex-wrap: wrap; }` |
| `justify-content` | 定义**主轴**上的项目整体分布方式 | `.container { justify-content: center; }` |
| `align-items` | 定义**交叉轴**上的单行项目对齐方式 | `.container { align-items: center; }` |
| `align-content` | 定义**多行场景**下交叉轴的整体对齐方式<br>（仅 `flex-wrap: wrap` 且内容换行时生效） | `.container { align-content: space-between; }` |

### 1.5 弹性项目（子元素）属性汇总
| 属性 | 作用 | 示例 |
| --- | --- | --- |
| `flex-grow` | 定义项目的放大比例，默认值为0（空间剩余时也不放大） | `.item { flex-grow: 1; }` |
| `flex-shrink` | 定义项目的缩小比例，默认值为1（空间不足时等比缩小，设为0则不缩小） | `.item { flex-shrink: 0; }` |
| `flex-basis` | 定义项目在主轴上的基准尺寸，优先级高于 `width/height` | `.item { flex-basis: 200px; }` |
| `flex` | `flex-grow` + `flex-shrink` + `flex-basis` 的简写属性 | `.item { flex: 1; }` |
| `align-self` | 覆盖容器的 `align-items`，单独定义单个项目的交叉轴对齐方式 | `.item { align-self: flex-end; }` |
| `order` | 定义项目的排列顺序，数值越小越靠前，默认值为0 | `.item { order: -1; }` |

### 1.6 主轴对齐方式 justify-content
| 属性值 | 效果 |
| --- | --- |
| `flex-start` | 默认值，所有项目靠主轴起点对齐 |
| `flex-end` | 所有项目靠主轴终点对齐 |
| `center` | 所有项目在主轴居中对齐 |
| `space-between` | 两端对齐，首尾项目贴边，剩余空间均匀分配在项目之间 |
| `space-around` | 每个项目两侧间隔相等，首尾与容器边界的间距是项目间距的一半 |
| `space-evenly` | 项目间距完全均匀，首尾与容器边界的间距等于项目间距 |

### 1.7 交叉轴对齐方式 align-items
| 属性值 | 效果 |
| --- | --- |
| `flex-start` | 项目靠交叉轴起点对齐 |
| `flex-end` | 项目靠交叉轴终点对齐 |
| `center` | 项目在交叉轴居中对齐（常用垂直居中方案） |
| `stretch` | 默认值，项目未设置高度时，拉伸填满交叉轴 |

### 1.8 主轴方向 flex-direction
| 属性值 | 描述 | 排列效果 | 代码示例 |
| --- | --- | --- | --- |
| `row` | 默认值，子元素沿水平主轴从左到右排列 | ABC（横向正序） | `.container { flex-direction: row; }` |
| `row-reverse` | 子元素沿水平主轴反向从右到左排列 | CBA（横向倒序） | `.container { flex-direction: row-reverse; }` |
| `column` | 子元素沿垂直主轴从上到下排列 | ABC（纵向正序） | `.container { flex-direction: column; }` |
| `column-reverse` | 子元素沿垂直主轴反向从下到上排列 | CBA（纵向倒序） | `.container { flex-direction: column-reverse; }` |
> ps:行内元素设置 `display: flex` 后，会直接变为块级弹性容器，可正常设置宽高、边距等属性。

### 1.9 换行属性 flex-wrap
| 属性值 | 排列效果 | 说明 |
| --- | --- | --- |
| `nowrap` | ABCDEFGH（全部横向排列，可能被压缩） | 默认值，不换行，空间不足时压缩子元素 |
| `wrap` | ABCD<br>EFGH | 换行，第一行在上方，第二行在下方，开发最常用 |
| `wrap-reverse` | EFGH<br>ABCD | 翻转换行，第一行在下方，第二行在上方，了解即可 |

### 1.10 多行交叉轴对齐 align-content
| 属性值 | 效果 |
| --- | --- |
| `flex-start` | 所有行靠交叉轴起点（顶部）对齐 |
| `flex-end` | 所有行靠交叉轴终点（底部）对齐 |
| `center` | 所有行在交叉轴居中对齐 |
| `space-between` | 两端对齐，首尾行贴边，行间距均匀分配 |
| `space-around` | 每行两侧间隔相等，首尾与容器边界间距为行间距的一半 |
| `space-evenly` | 所有间距完全均匀，首尾与容器边界间距等于行间距 |

### 1.11 子项目伸缩与 gap 间隙
#### 1. flex 简写属性
- 常用写法 `flex: 1` 等同于 `flex: 1 1 0%`，表示项目等比例分配剩余空间，是开发中最常用的写法

#### 2. gap 间隙属性
- 作用：统一设置行与行、列与列之间的间隙，不会作用在容器边缘
- 语法：
  - `gap: 20px;`：行间距、列间距均为20px
  - `gap: 20px 10px;`：行间距20px，列间距10px
- ps:可拆分为 `row-gap`（行间距）和 `column-gap`（列间距）单独设置

### 1.12 实战：多行商品布局实现方案
实现核心步骤（父容器设置）：
1. `display: flex` 开启弹性布局
2. `flex-wrap: wrap` 允许子元素自动换行
3. `flex-direction` 设置主轴方向
4. `justify-content` 设置主轴对齐
5. `align-items` 设置单行内交叉轴对齐
6. `align-content` 设置多行场景的行对齐

子元素设置：
1. `flex` 数值分配主轴剩余宽度，或通过 `width/flex-basis` 控制每行显示个数
2. 配合 `gap` 或内边距设置元素间距

#### 两种经典实现写法
1. 京东写法（calc计算宽度+外边距）
```css
.box .item {
  flex: 1;
  min-width: calc(16.6666667% - 16px);
  max-width: calc(16.6666667% - 16px);
  margin: 0 8px 16px;
}
```
2. 淘宝写法（负外边距抵消边缘间隙）
```css
.box {
  display: flex;
  flex-wrap: wrap;
  margin-left: -8px;
  margin-right: -8px;
}
.box .item {
  flex: 0 0 16.666666%;
  margin-bottom: 16px;
  padding: 0 8px;
  box-sizing: border-box;
}
```
> ps:Flex布局水平垂直居中万能写法：
> ```css
> .container {
>   display: flex;
>   justify-content: center;
>   align-items: center;
> }
> ```
> 这是前端开发最高频的居中方案，兼容所有现代浏览器。

---

## 二、网格布局（Grid）
### 2.1 基本概念
Grid 是 CSS 二维网格布局系统，可同时控制行和列的排列，适合复杂的二维页面布局；Flex 是一维布局，适合单行/单列的对齐分布。

### 2.2 网格容器基础
给父元素设置 `display: grid` 即可创建**块级网格容器**，设置 `display: inline-grid` 则创建行内网格容器。
> 注意：仅声明 `display: grid` 时，默认只有一列，子元素会纵向依次排列。

### 2.3 网格轨道
- 作用：定义网格的基础行列结构，为子元素提供固定的网格位置
- 定义属性：
  - `grid-template-columns`：定义列的数量与宽度，有几个属性值就创建几列
  - `grid-template-rows`：定义行的数量与高度，有几个属性值就创建几行

#### 轨道尺寸的常见写法
| 写法类型 | 说明 | 示例 | 适用场景 |
| --- | --- | --- | --- |
| 固定像素 | 固定列宽/行高 | `grid-template-columns: 100px 200px;` | 固定尺寸布局 |
| 百分比 | 按容器宽度比例分配 | `grid-template-columns: 30% 70%;` | 响应式比例布局 |
| fr 单位 | 分数单位，按比例分配剩余空间 | `grid-template-columns: 1fr 2fr;` | 自适应比例布局 |
| auto 自适应 | 由内容自动撑开尺寸 | `grid-template-columns: auto 100px;` | 内容自适应场景 |
| `repeat()` 函数 | 批量重复创建轨道 | `grid-template-columns: repeat(3, 1fr);` | 多列等宽布局 |
| `minmax()` 函数 | 设置尺寸的最小/最大值范围 | `grid-template-columns: minmax(100px, 1fr);` | 自适应响应式 |

### 2.4 轨道整体对齐方式
控制整个网格在容器内的对齐方式，分为水平方向和垂直方向：
| 属性 | 控制方向 | 常用属性值 |
| --- | --- | --- |
| `justify-content` | 水平方向（列方向） | `start` / `end` / `center` / `space-between` / `space-around` / `space-evenly` |
| `align-content` | 垂直方向（行方向） | `start` / `end` / `center` / `space-between` / `space-around` / `space-evenly` |
| `place-content` | 水平+垂直同时设置 | `place-content: center center;` |
> 属性值效果与Flex布局一致：
> - `start`：靠起始边缘对齐
> - `center`：居中对齐
> - `space-between`：两端贴边，中间间距均匀
> - `space-around`：每个轨道两侧间距相等
> - `space-evenly`：所有间距完全相等

### 2.5 网格间距 gap
- 作用：设置网格行与行、列与列之间的间隙
- 语法：
  - `gap: 20px;`：行间距、列间距均为20px
  - `gap: 20px 10px;`：行间距20px，列间距10px
- 拆分写法：`row-gap`（行间距）、`column-gap`（列间距）

### 2.6 自动填充实现响应式
利用 `repeat()` + `auto-fit` + `minmax()` 实现纯CSS自动响应式列布局：
```css
.container {
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
}
```
- `minmax(最小值, 最大值)`：限制每列的最小宽度和最大宽度
- `auto-fit`：尽可能多地创建列，填满容器宽度，空列会自动收缩
> ps:`auto-fit` 与 `auto-fill` 的区别：
> - `auto-fill`：尽可能多地创建列，即使容器有空余空间，也会保留空列的位置
> - `auto-fit`：尽可能多地创建列，空余空间会平均分配给已有列，让列自动撑满容器
> 实际开发中 `auto-fit` 更常用，适配效果更美观。

### 2.7 网格线与跨单元格
作用：通过网格线编号，实现子元素跨越多个网格单元格。
#### 语法
1. 跨列：`grid-column: 起始线编号 / 结束线编号`
2. 跨行：`grid-row: 起始线编号 / 结束线编号`
3. 简写形式：`grid-column: 起始线 / span 跨越数量`
> 注意：该属性设置在子项目（子元素）上，而非容器上。

### 2.8 自动排布规则 grid-auto-flow
控制子元素超出定义网格时的自动排布方向：
- `row`（默认值）：优先按行排列，满了自动换行
- `column`：优先按列排列，满了自动换列
- `dense`：稠密排布，自动填补空白位置，适合瀑布流类布局

### 2.9 替换元素适配 object-fit
`object-fit` 用于控制 `<img>`、`<video>` 等替换元素的内容如何适配容器尺寸：
| 属性值 | 描述 |
| --- | --- |
| `fill` | 默认值，拉伸内容填满容器，不保持宽高比，可能导致变形 |
| `contain` | 保持宽高比，缩放至内容完全显示在容器内，容器可能出现空白 |
| `cover` | 保持宽高比，缩放至完全覆盖容器，内容可能被裁剪（图片展示最常用） |

### 2.10 单元格内项目对齐
控制单个单元格内的项目对齐方式，设置在容器上：
| 属性 | 控制方向 | 常用属性值 |
| --- | --- | --- |
| `justify-items` | 单元格内水平对齐 | `start` / `end` / `center` / `stretch` |
| `align-items` | 单元格内垂直对齐 | `start` / `end` / `center` / `stretch` |
| `place-items` | 水平+垂直同时设置 | `place-items: center center;` |

> 也可给单个项目设置 `justify-self` / `align-self` / `place-self`，单独控制某一个单元格内的对齐方式。

---

## 三、媒体查询与响应式布局
### 3.1 响应式布局定义
响应式布局指：页面根据设备的屏幕尺寸、分辨率、屏幕方向等特征，自动调整布局结构和内容展示，实现一套代码适配PC、平板、手机等多端设备。

### 3.2 视口标签 viewport
移动端页面必须设置视口标签，保证页面在移动设备上按正确比例显示：
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```
| 属性 | 作用 | 标准值 | 说明 |
| --- | --- | --- | --- |
| `width` | 设置视口宽度 | `device-width` | 跟随设备屏幕宽度，移动端默认980px，必须设为device-width |
| `initial-scale` | 页面初始缩放比例 | `1.0` | 初始不缩放，保证1:1显示 |
| `minimum-scale` | 允许用户缩放的最小比例 | 0.1 | 设为1.0则禁止用户缩小 |
| `maximum-scale` | 允许用户缩放的最大比例 | 2.0 | 设为1.0则禁止用户放大 |
| `user-scalable` | 是否允许用户手动缩放 | `no` | 禁用缩放可提升体验，但需考虑无障碍需求 |

### 3.3 像素基础概念
1. **物理像素**：设备屏幕硬件层面的实际像素点总数，是屏幕的固有属性
2. **CSS像素**：CSS中使用的虚拟像素单位，用于布局和样式计算，通过**设备像素比（DPR）**与物理像素关联
   - 设备像素比 DPR = 物理像素 / CSS像素

### 3.4 移动端设计稿规范
移动端设计稿通常以 **750px** 作为标准宽度（@2x设计稿）：
1. 行业以iPhone6/7/8为基准，其物理分辨率为750×1334px，750px成为默认规范
2. 开发时按375px逻辑宽度编写（即设计稿尺寸÷2），即可对应1:1视觉还原
3. 图片、图标按2倍尺寸输出，在高清Retina屏上显示更清晰

### 3.5 移动端适配方案
#### 方案1：响应式布局
一套代码通过媒体查询断点，完成PC、平板、手机端的适配，适合官网、展示类网站。

#### 方案2：rem / vw 单位适配
通过相对单位实现全尺寸适配，适合移动端H5、小程序页面。
##### vw 单位适配
- vw 是基于视口宽度的相对单位，`1vw = 视口宽度的 1%`
- 换算公式：`CSS中vw值 = (设计稿中的px值 / 设计稿宽度) × 100vw`
- 实现方式：
  1. 手动使用 `calc()` 函数计算
  2. 使用 Less / Sass 等CSS预处理器封装函数
  3. 配合 postCSS 插件自动转换

### 3.6 响应式布局核心
1. **常用布局方式**：弹性盒Flex、网格Grid、百分比流式布局，配合vw/rem单位
2. **断点（Breakpoints）**：根据常见设备宽度设置样式切换的临界点
   - 手机端：< 768px
   - 平板端：768px ~ 1024px
   - PC端：> 1024px

### 3.7 标准响应式断点（Bootstrap 规范）
| 断点代号 | 宽度范围 | 媒体查询写法 | 适用设备 |
| --- | --- | --- | --- |
| xs | < 576px | `@media (max-width: 575px)` | 竖屏手机 |
| sm | ≥ 576px | `@media (min-width: 576px)` | 横屏手机 |
| md | ≥ 768px | `@media (min-width: 768px)` | 平板 |
| lg | ≥ 992px | `@media (min-width: 992px)` | 桌面中小屏 |
| xl | ≥ 1200px | `@media (min-width: 1200px)` | 桌面标准屏 |
| xxl | ≥ 1400px | `@media (min-width: 1400px)` | 超宽屏/4K屏 |

### 3.8 媒体查询语法
#### 基础语法
```css
@media 媒体类型 and (媒体条件) {
  /* 满足条件时生效的样式 */
}
```
- 常用媒体类型：`screen`（屏幕设备，最常用）
- 常用媒体条件：`min-width`（最小宽度）、`max-width`（最大宽度）

#### 示例：多断点商品列数适配
```css
/* 屏幕宽度 ≥ 1536px，一行放6个 */
@media screen and (min-width: 1536px) {
  .box .item {
    max-width: calc(16.66% - 16px);
    min-width: calc(16.66% - 16px);
  }
}

/* 屏幕宽度 1316px ~ 1535px，一行放5个 */
@media screen and (min-width: 1316px) and (max-width: 1535px) {
  .box .item {
    max-width: calc(20% - 16px);
    min-width: calc(20% - 16px);
  }
}

/* 屏幕宽度 < 1316px，一行放4个 */
@media screen and (max-width: 1315px) {
  .box .item {
    max-width: calc(25% - 16px);
    min-width: calc(25% - 16px);
  }
}
```

> ps:响应式开发最佳实践：
> 1. 推荐**移动端优先**写法：默认写手机端样式，通过 `min-width` 逐级向上适配平板、PC端，代码更简洁
> 2. 媒体查询统一写在样式表底部，按断点从小到大排列，便于维护
> 3. 响应式图片配合 `max-width: 100%` 使用，避免图片溢出容器
```