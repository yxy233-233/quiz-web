# 迷你网考系统（quiz-app）从零复现实验手册

> 面向 **Web App 基础编程** 课程 · 零基础起步 · 全程手写，禁止复制粘贴源码
>
> 全程用 Git 记录版本，每个 Phase 结束打一个标签，写坏可一键回退

---

## 目录

- [0 开始之前](#sec-0)
  - [0.1 这份手册怎么用](#sec-0-1)
  - [0.2 你最终要做出的东西](#sec-0-2)
  - [0.3 实验环境清单](#sec-0-3)
  - [0.4 三条学习纪律](#sec-0-4)
- [1 前置基础知识](#sec-1)
  - [1.1 三件套的分工与协作方式](#sec-1-1)
  - [1.2 HTML 必备知识](#sec-1-2)
  - [1.3 CSS 必备知识](#sec-1-3)
  - [1.4 JavaScript 必备知识](#sec-1-4)
  - [1.5 随堂自测](#sec-1-5)
- [2 核心操作技能：Emmet](#sec-2)
  - [2.1 Emmet 是什么](#sec-2-1)
  - [2.2 缩写的八条通用规则](#sec-2-2)
  - [2.3 Emmet 速查表](#sec-2-3)
  - [2.4 用 Emmet 打出 quiz-app 的完整骨架](#sec-2-4)
- [3 开发环境与 VS Code 必备配置](#sec-3)
  - [3.1 安装 VS Code](#sec-3-1)
  - [3.2 Live Server：实时预览服务器](#sec-3-2)
  - [3.3 Prettier：代码格式化](#sec-3-3)
  - [3.4 中文语言包与其他推荐插件](#sec-3-4)
  - [3.5 设置汇总与配置方法](#sec-3-5)
  - [3.6 环境验收](#sec-3-6)
- [4 Git 版本管理基础与实战](#sec-git)
  - [4.1 Git 是什么：三个区域](#sec-git-1)
  - [4.2 安装 Git 与首次配置](#sec-git-2)
  - [4.3 核心命令速查](#sec-git-3)
  - [4.4 分支 branch](#sec-git-4)
  - [4.5 标签 tag：给每个阶段存档](#sec-git-5)
  - [4.6 .gitignore：该忽略什么](#sec-git-6)
  - [4.7 实战演练：为练习工程铺一条版本线](#sec-git-7)
  - [4.8 Git 排错速查](#sec-git-8)
- [5 项目拆解实战](#sec-5)
  - [实战总览](#sec-5-0)
  - [Phase 0 工程初始化与跑通](#sec-phase-0)
  - [Phase 1 用 Emmet 搭建 HTML 骨架](#sec-phase-1)
  - [Phase 2 手写 CSS 完成页面视觉](#sec-phase-2)
  - [Phase 3 用 JavaScript 准备题库数据](#sec-phase-3)
  - [Phase 4 获取 DOM 元素并渲染题目](#sec-phase-4)
  - [Phase 5 实现判题与计分](#sec-phase-5)
  - [Phase 6 结算页与重新开始](#sec-phase-6)
  - [Phase 7 联调、验收与自查](#sec-phase-7)
- [6 常见错误排查手册](#sec-6)
- [7 验收清单与进阶挑战](#sec-7)
  - [7.1 验收清单](#sec-7-1)
  - [7.2 与原项目的自查比对](#sec-7-2)
  - [7.3 三道进阶挑战题](#sec-7-3)

---

<a id="sec-0"></a>

## 0 开始之前

<a id="sec-0-1"></a>

### 0.1 这份手册怎么用

本手册配套仓库中的 `quiz-app/` 目录，那是一个**已经写好的完整成品**（`index.html`、`style.css`、`script.js` 三个文件，共约 200 行代码）。

它的用途**不是让你抄**，而是两个：

1. **答案**：你写完之后，拿它逐项对照自查；
2. **参照**：卡住了先看一眼它怎么写的，理解后再回到自己的文件里手写一遍。

你的实验产物放在另一个**全新的目录**里（推荐 `D:\quiz-web\lab\quiz-app\`），不要在原目录上直接改，否则你将失去自查的依据。

手册的组织方式是 **Phase（阶段）+ 小步（Step）+ Git 节点**：

- 每个 Phase 有明确的**阶段目标**和**完成标志**；
- 每个 Step 固定包含五段：**本步目标 → 操作步骤 → 参考代码 → 测试步骤 → 易错点**；
- **测试步骤是强制项**。没跑通测试就不要进入下一步，否则错误会像滚雪球一样堆到最后；
- 每个 Phase 末尾有固定的 **Git 版本节点**：提交一次代码 + 打一个标签，把这个 Phase 冻成可回退的存档点。详见 [第 4 章](#sec-git)。

**全程会打的 9 个标签**（如果你想先预览最终成果长什么样）：

| 标签 | 对应 Phase | 那一刻系统是什么样 |
| --- | --- | --- |
| `v0.0-start` | 练习起点 | 三个空文件 + `.gitignore` |
| `v0.1-init` | Phase 0 | 浏览器里能看到 `Hello Quiz` |
| `v0.2-html` | Phase 1 | 结构完整，选项可点且互斥（还没样式） |
| `v0.3-css` | Phase 2 | 外观与原项目完全一致 |
| `v0.4-data` | Phase 3 | 题库就绪（页面还没接上） |
| `v0.5-render` | Phase 4 | 页面加载即显示第 1 题 |
| `v0.6-scoring` | Phase 5 | 能答完 4 题（结算页待补） |
| `v0.7-result` | Phase 6 | **功能完整** |
| `v1.0-final` | Phase 7 | 全部验收通过 |

写完最后一行时，你已经能用 `git checkout v0.3-css` 回到任何一个中间时刻，亲眼看看它当时的样子。**这就是这个实验里 Git 的意义。**

<a id="sec-0-2"></a>

### 0.2 你最终要做出的东西

一个能在浏览器里运行的四题单选迷你网考系统，具备以下行为：

| 能力 | 说明 |
| --- | --- |
| 展示题目 | 页面加载即显示第 1 题的题干和 4 个选项 |
| 单选互斥 | 4 个选项同一时刻只能选中一个 |
| 换题即清空 | 切到下一题时，上一题选中的选项自动取消 |
| 计分判题 | 点 Submit，对则 `score` 加 1，错则不加分 |
| 防呆 | 没选任何选项就点 Submit，什么也不发生 |
| 结算显示 | 4 题答完后，整块内容被替换为「You answered X/4 questions correctly」 |
| 一键重开 | 结算页按钮刷新页面，重新开始一轮 |

视觉上：一个浅蓝到白的**斜向渐变背景**，正中一张**白色圆角卡片**（带柔和阴影），卡片内上方是居中的题干，下面是 4 个选项，最底部是一条**通栏紫色按钮**，鼠标悬停变深、聚焦时更变暗。

原项目截图见 `quiz-app/index.html` 打开后的效果，本手册每一步都会让你对照检查。

<a id="sec-0-3"></a>

### 0.3 实验环境清单

| 项目 | 要求 | 说明 |
| --- | --- | --- |
| VS Code | 1.75 或以上 | 建议安装稳定版（Stable） |
| 浏览器 | Chrome / Edge 任选其一 | 需带开发者工具（F12） |
| **Git** | **2.20 或以上** | 本实验的版本管理工具，见 [第 4 章](#sec-git) |
| Node.js | 本实验**不需要** | 纯静态页面，不涉及构建工具 |
| 网络 | 仅 CSS 引入 Google Fonts 时需要 | 内网环境可跳过，字体降级为系统字体 |

<a id="sec-0-4"></a>

### 0.4 四条学习纪律

1. **不许复制粘贴代码。** 手册里的代码块是给你**对照抄写**的，不是给你粘贴的。手敲一遍和粘贴一遍，学习效果差 10 倍。
2. **不许一次写完整个文件。** 必须按 Step 粒度推进，每步跑通测试再往下。
3. **不许跳过测试。** 尤其是「预期」写着「和上一屏完全一样」时——那是用来确认你确实没搞坏东西的。
4. **不许漏掉 Git 节点。** 每个 Phase 结束都要提交并打标签（见每个 Phase 末尾的「Git 版本节点」）。标签是**唯一**能让你回头救场的坐标，漏打一个，后面某个 Phase 崩了就只能从头重写。

---

<a id="sec-1"></a>

## 1 前置基础知识

本项目的全部代码只用到 HTML、CSS、JavaScript 三件套中最基础的部分。这一章不追求讲全，只讲**你在 Phase 1-6 里会亲手用到的**。每节末尾有随堂练习，**请先做完再往下看答案**。

<a id="sec-1-1"></a>

### 1.1 三件套的分工与协作方式

把网页想成一个人：**HTML 是骨架，CSS 是衣服，JS 是大脑。**

| 语言 | 负责什么 | 本项目里的体现 |
| --- | --- | --- |
| HTML | 页面上**有什么**（结构） | 一张卡片、1 个标题、4 个选项、1 个按钮 |
| CSS | 这些东西**长什么样**（样式） | 渐变背景、白色卡片、紫色按钮 |
| JavaScript | 这些东西**怎么动**（行为） | 换题、判题、计分、结算 |

三者在项目里是这样连起来的（`quiz-app/index.html:6` 和 `:37`）：

```html
<!-- 在 head 里告诉浏览器：这个页面的样式放在 style.css -->
<link rel="stylesheet" href="style.css" />

<!-- 在 body 末尾告诉浏览器：这个页面的行为放在 script.js -->
<script src="script.js"></script>
```

两个关键点，后面会反复用到：

- `href` / `src` 的值是**相对路径**。`index.html` 和 `style.css` 必须在**同一个文件夹**里，否则浏览器找不到（这是最常见的「样式全丢了」的原因）。
- `<script>` 放在 `</body>` 的**前一行**，而不是 `<head>` 里。原因是：脚本一执行就要去找页面上的元素，如果放在 `<head>`，那时 `<body>` 还没被解析，元素还不存在，`document.getElementById(...)` 会返回 `null`，然后 JS 直接报错、整个页面停止工作。

> 如果你确实想把脚本挪到 `<head>`，必须加 `defer` 属性（`<script src="script.js" defer></script>`），它表示「等 HTML 解析完再执行」。本实验**不要求**这样做，知道有这回事即可。

<a id="sec-1-2"></a>

### 1.2 HTML 必备知识

#### 文档基本骨架

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="style.css" />
    <title>Quiz App</title>
  </head>
  <body>
    <!-- 页面内容写这里 -->
  </body>
</html>
```

逐行解释：

| 行 | 作用 |
| --- | --- |
| `<!DOCTYPE html>` | 告诉浏览器「这是 HTML5」，必须写在第一行 |
| `<html lang="en">` | 根元素，`lang` 声明页面语言 |
| `<meta charset="UTF-8" />` | 字符编码。不写的话中文会变成乱码 |
| `<meta name="viewport" ...>` | 移动端适配，手机上不让页面被缩放 |
| `<link rel="stylesheet">` | 引入外部 CSS |
| `<title>` | 浏览器标签页上显示的文字 |

#### 标签：成对与自闭合

```html
<div>内容</div>          <!-- 成对标签：有开有闭 -->
<link rel="stylesheet" /> <!-- 自闭合标签：HTML5 里不用写结束标签 -->
<input type="radio" />      <!-- 自闭合标签 -->
```

注意 `<input>` 没有结束标签，这是错的：

```html
<input type="radio"></input>  <!-- ✗ 多余的结束标签 -->
```

#### 属性：写在开始标签里

```html
<input type="radio" name="answer" id="a" class="answer" />
```

| 属性 | 值 | 作用 |
| --- | --- | --- |
| `type="radio"` | radio | 把 input 变成**单选按钮**（圆圈） |
| `name="answer"` | answer | **互斥组名**。同 `name` 的 radio 只能选一个 |
| `id="a"` | a | 元素唯一标识，CSS `#a`、JS `getElementById('a')` 都靠它 |
| `class="answer"` | answer | 元素分类标识，CSS `.answer`、JS `querySelectorAll('.answer')` 靠它 |

> **`name` 是单选互斥的唯一开关。** 如果 4 个 radio 的 `name` 不一样（或者干脆没有），用户可以同时勾选 4 个，整个判题逻辑立刻失效。Phase 1 会专门让你测这一条。

#### id 和 class 的区别

| | `id` | `class` |
| --- | --- | --- |
| 唯一性 | 整页**只能有一个** | 可以有很多个 |
| 写法 | `id="quiz"` | `class="answer"` |
| CSS 选择器 | `#quiz` | `.answer` |
| JS 选取 | `getElementById('quiz')` 返回**单个**元素 | `querySelectorAll('.answer')` 返回**一组**元素 |
| 用途 | 定位唯一元素 | 给同类元素批量打标签 |

本项目里：`#quiz`、`#question`、`#submit` 这类唯一元素用 `id`；4 个单选框都用 `class="answer"`，因为脚本要**一次性拿到全部 4 个**来清空选中状态和读取用户的选择。

#### 单选框和 label 的配合（本项目最关键的 HTML 知识点）

```html
<input type="radio" name="answer" id="a" class="answer" />
<label for="a" id="a_text">Java</label>
```

- `input` 的 `id="a"` 和 `label` 的 `for="a"` **必须一字不差地相同**；
- 二者的作用是：**点击 label 上的文字，也能选中对应的 radio**；
- 如果不写 `for`，用户就必须极其精确地点到那个小圆圈上，否则选不中——这会直接影响后面的 `getSelected()` 能否读到值。

> 原项目特意给每个 label 也加了 `id`（`a_text`、`b_text`…），这不是必需的，只是**为了方便 JS 找到它们并改写文字**。JS 改的是 label 的文字，不是 radio 本身。

#### 列表标签

```html
<ul>
  <li>选项一</li>
  <li>选项二</li>
</ul>
```

`ul` 是无序列表容器，`li` 是列表项。语义上「一组选项」用列表是正确的；本项目再用 CSS 把项目符号（`list-style-type: none`）去掉，让它纯粹用于排版。

#### 随堂练习（HTML）

1. 手写一个完整 HTML5 骨架，标题为 `Test`，并引入同目录下的 `test.css`。
2. 在 `<body>` 里写一个 `div`，`id` 为 `wrap`，`class` 为 `box`。
3. 在 `wrap` 里面写一个 `ul`，包含 **5** 个 `li`。
4. 写两个单选框，`name` 都是 `gender`，`id` 分别是 `male` 和 `female`；各自配一个 `label`，`for` 正确对应。
5. 解释：把第二个 radio 的 `name` 改成 `gender2` 会发生什么？

<details>
<summary>参考答案</summary>

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Test</title>
    <link rel="stylesheet" href="test.css" />
  </head>
  <body>
    <div id="wrap" class="box">
      <ul>
        <li>一</li>
        <li>二</li>
        <li>三</li>
        <li>四</li>
        <li>五</li>
      </ul>
      <input type="radio" name="gender" id="male" />
      <label for="male">男</label>
      <input type="radio" name="gender" id="female" />
      <label for="female">女</label>
    </div>
  </body>
</html>
```

第 5 题：两个 radio 的 `name` 不同，浏览器就认为它们是**两组独立的单选框**，可以同时被选中，互斥失效。

</details>

<a id="sec-1-3"></a>

### 1.3 CSS 必备知识

#### 选择器：怎么找到要改的元素

```css
h2            { }   /* 按标签名 */
#quiz         { }   /* 按 id，匹配 1 个 */
.answer       { }   /* 按 class，匹配多个 */
ul li         { }   /* 后代选择器：ul 里面的所有 li */
button:hover  { }   /* 伪类：鼠标悬停时 */
button:focus  { }   /* 伪类：元素获得焦点时 */
```

对应到本项目：

- `body` — 整个页面的背景和居中
- `.quiz-container` — 白色卡片
- `h2` — 题干
- `ul` — 去掉列表符号和默认内边距
- `ul li` — 每个选项的行距和字号
- `ul li label` — 让鼠标指针在文字上显示为手型（暗示可点）
- `button` / `button:hover` / `button:focus` — 按钮的三种状态

> 注意 `h2`、`ul`、`button` 写的是**标签名**，会作用于页面上所有同标签元素。本项目里每种只有一个，所以没问题；但养成习惯写 `.card h2` 这种更精确的写法会更专业。

#### 盒模型与 `box-sizing`

CSS 里每个元素的尺寸 = 内容 + 内边距(padding) + 边框(border)。默认 `content-box` 模式下，你写 `width: 600px`，元素的**实际占位是 600 + 左右 padding**。

```css
* {
  box-sizing: border-box;   /* 改成：width 就是最终总宽 */
}
```

这条规则放在样式表**第一段**，让后面所有 `width` 符合直觉。本项目的 `.quiz-container` 写了 `width: 600px`，有了这条规则，卡片总宽才正好是 600px，不会因为 padding 变成 632px 而在某些屏幕上溢出。

#### 常用单位

| 单位 | 含义 | 本项目用例 |
| --- | --- | --- |
| `px` | 像素 | `box-shadow` 的模糊半径 |
| `rem` | 相对根字号（16px） | `.quiz-header { padding: 4rem }` |
| `em` | 相对当前元素字号 | 本项目未用到 |
| `%` | 相对父元素 | `width: 100%`（按钮通栏） |
| `vh` | 视口高度的 1% | `height: 100vh`（撑满屏幕） |
| `vw` | 视口宽度的 1% | 本项目未用到 |

`4rem` 就是 64px，比 `64px` 更有「随根字号缩放」的意味。

#### Flex 居中（本项目视觉的基础）

```css
body {
  display: flex;
  align-items: center;        /* 垂直方向居中 */
  justify-content: center;    /* 水平方向居中 */
  height: 100vh;              /* 先把 body 撑满一屏，否则没地方居中 */
}
```

原理：`display: flex` 让 body 变成弹性容器，里面唯一的 `.quiz-container` 成为一个「弹性项」，`align-items` + `justify-content` 把它推到正中央。

**为什么必须先写 `height: 100vh`？** 因为 body 默认高度是「内容有多高就多高」，只有内容撑不满一屏，居中就无从谈起。

#### 渐变背景

```css
background-color: #b8c6db;                                  /* 兜底纯色 */
background-image: linear-gradient(315deg, #b8c6db 0%, #f5f7fa 100%);
```

- `linear-gradient(角度, 起点色 起点位置, 终点色 终点位置)`
- `315deg` 指渐变方向。CSS 中 0deg 指向**正上方**，角度顺时针增加，所以 `315deg` 是**右上方**指向左下方（本项目是从右上到左下变亮）。
- 先写 `background-color` 是为了在 `@import` 的网络字体或渐变加载失败时，页面仍有颜色，不至于白屏。

#### 其他会用到的小属性

| 属性 | 用途 | 本项目 |
| --- | --- | --- |
| `border-radius` | 圆角 | 卡片 `10px` |
| `box-shadow` | 阴影 | `0 0 10px 2px rgba(100,100,100,0.1)` |
| `overflow: hidden` | 裁掉超出部分 | 配合圆角，裁掉子元素溢出 |
| `margin: 0` | 清掉默认外边距 | `body` |
| `cursor: pointer` | 鼠标变手型 | 选项文字、按钮 |
| `font-family: inherit` | 按钮继承页面字体 | 按钮（否则会用系统默认字体） |
| `outline: none` | 去掉聚焦时的黑色方框 | `button:focus` |

> `box-shadow` 的四个值依次是：`水平偏移 垂直偏移 模糊半径 扩散半径`。`0 0 10px 2px rgba(...)` = 不偏移、模糊 10px、向外扩 2px、颜色是 10% 透明度的灰。

#### 随堂练习（CSS）

1. 写一段 CSS，让 `body` 渐变背景、上下左右居中、撑满屏幕。
2. 给一个 `id="card"` 的 `div` 加：白底、圆角 10px、阴影、宽 600px。
3. 写一条规则，鼠标悬停时把按钮背景从 `#8e44ad` 变成 `#732d91`。
4. 回答：如果删掉 `box-sizing: border-box` 而是删掉 `height: 100vh`，哪个会让卡片偏离屏幕正中央？为什么？
5. 回答：`ul { padding: 0 }` 和 `ul { margin: 0 }` 解决的是同一个问题吗？

<details>
<summary>参考答案</summary>

```css
body {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
  margin: 0;
  background-color: #b8c6db;
  background-image: linear-gradient(315deg, #b8c6db 0%, #f5f7fa 100%);
}

#card {
  background-color: #fff;
  border-radius: 10px;
  box-shadow: 0 0 10px 2px rgba(100, 100, 100, 0.1);
  width: 600px;
  overflow: hidden;
}

button:hover {
  background-color: #732d91;
}
```

第 4 题：删掉 `height: 100vh` 会让卡片偏离正中央。body 高度塌缩成内容高度，居中的参考区域变成内容本身那么高，卡片会挤在顶部。

第 5 题：不是。浏览器给 `ul` 默认加了 `padding-left`（缩进）和 `margin`。`padding: 0` 去掉的是**内边距造成的视觉缩进**；`margin: 0` 去掉的是外边距。本项目只需去掉 padding 缩进，所以只写了 `padding: 0`。

</details>

<a id="sec-1-4"></a>

### 1.4 JavaScript 必备知识

#### 变量与基本类型

```js
const quizData = []   // const：声明后不能重新赋值（数组内容仍可改）
let currentQuiz = 0   // let：可以重新赋值
```

本项目里：题库、DOM 引用用 `const`（引用不变），题号和分数用 `let`（每答一题都要变）。

还有一个本项目到处都在用的值：**`undefined`**。它表示「还没有值」。比如用户没选任何选项就点提交，`getSelected()` 就会返回 `undefined`，`if (answer)` 判断的就是「有没有值」。

#### 数组与对象

题库是一个**对象组成的数组**：

```js
const quizData = [
  {
    question: "Which language runs in a web browser?",
    a: "Java",
    b: "C",
    c: "Python",
    d: "JavaScript",
    correct: "d",
  },
  // ... 共 4 个这样的对象
]
```

访问方式：

```js
quizData.length          // 4        数组长度
quizData[0]              // 第 1 个对象
quizData[0].question     // 第 1 题的题干
quizData[3].correct      // 第 4 题的正确答案
```

**为什么 `correct` 存的是 `"d"` 而不是选项文字？** 因为 HTML 里每个 radio 的 `id` 就是 `a`/`b`/`c`/`d`，脚本拿到用户选的那个 `id`，直接和 `correct` 比字符串即可，不必再绕一圈去比对文字。

#### 函数

```js
function loadQuiz() {
  // 做一件事
}
```

本项目有 3 个函数：`loadQuiz()`（渲染题目）、`deselectAnswers()`（清空选中）、`getSelected()`（读取选中项）。

JS 有一条规则要提前知道：**函数声明会被「提升」**。也就是写在下面的 `loadQuiz` 函数，可以先在第 48 行调用 `loadQuiz()`，不会报错。但 `const quiz = document.getElementById('quiz')` 这种**赋值语句不会被提升**，必须先赋值再使用。

#### DOM：脚本和页面的唯一通道

```js
document.getElementById('quiz')          // 返回单个元素，或 null
document.querySelectorAll('.answer')     // 返回一组元素（NodeList）
```

| 方法 | 参数 | 返回 |
| --- | --- | --- |
| `getElementById(id)` | id 字符串，不带 `#` | 元素 或 `null` |
| `querySelectorAll(选择器)` | **完整的 CSS 选择器**，带 `.` 或 `#` | NodeList（类数组，有 `.length`、能 `.forEach`） |

> **最常见的低级错误**：把 `getElementById('quiz')` 写成 `getElementById('.quiz')` 或 `getElementById('#quiz')`——它**不接选择器**。
> 反过来，`querySelectorAll('#quiz')` 是**正确**的，它接选择器。

#### 修改元素内容：`innerText` 与 `innerHTML`

```js
questionEl.innerText = currentQuizData.question;   // 纯文字
quiz.innerHTML = `<h2>You answered 3/4 questions correctly</h2>`;  // 一整段 HTML
```

| | `innerText` | `innerHTML` |
| --- | --- | --- |
| 赋值内容 | 当成纯文本 | 解析成 HTML |
| 能否放 `<h2>` | 会显示成字面量 `<h2>...` | 正常渲染成标题 |
| 本项目用在哪 | 换题干、换 4 个选项文字 | 结算时整块替换 |

**换题目用 `innerText` 就够了；结算页要造一个 `<h2>` 和一个 `<button>`，必须用 `innerHTML`。**

#### 模板字符串

```js
quiz.innerHTML = `
    <h2>You answered ${score}/${quizData.length} questions correctly</h2>
    <button onclick="location.reload()">Reload</button>
`
```

- 用**反引号** `` ` `` 包裹（不是单引号）；
- `${}` 里可以放任何 JS 表达式，结算文案里的分数和题目总数就靠它自动填；
- 因为里面要写 HTML，引号**必须是反引号**，否则会被提前截断。

#### 事件

```js
submitBtn.addEventListener('click', () => {
  // 用户每点一次，这里执行一次
})
```

- 第一个参数是事件名（`'click'`）；
- 第二个参数是回调函数，`() => {}` 是**箭头函数**的简写，等价于 `function () {}`。

#### 遍历：`forEach`

```js
answerEls.forEach(answerEl => {
  answerEl.checked = false
})
```

读作「**把 `answerEls` 里的每一个元素，都执行一遍箭头函数里的操作**」。

`answerEls` 是 `querySelectorAll('.answer')` 的结果——一个包含 4 个 radio 的集合。`answerEl` 是每次循环时「当前这一个元素」的临时名字。

> 这里用 `forEach` 而不是传统 `for` 循环，是因为集合自带遍历能力，不用手写 `i++` 和下标，代码更短也更安全（不会越界）。

#### 判断与自增

```js
if (answer) {                    // answer 有值才继续（undefined / 空字符串为假）
    if (answer === quizData[currentQuiz].correct) {
        score++                 // 加 1
    }
    currentQuiz++                // 下一题
}
```

用 `===` 而不是 `==`，这是**严格相等**，类型和值都相同才算相等。现代 JS 里这是唯一应该用的写法。

#### 随堂练习（JavaScript）

在浏览器 F12 的 Console 里，用一行行敲（不要粘贴）的方式完成：

1. `const nums = [3, 1, 4]` 然后分别打出 `nums.length`、`nums[0]`、`nums[2]`。
2. 写一个对象 `const p = { name: 'Ada', age: 36 }`，打出 `p.name`。
3. 用 `forEach` 把 `[1,2,3]` 打到 Console。
4. 打开本项目的页面，在 Console 输入 `document.getElementById('quiz')` 和 `document.querySelectorAll('.answer').length`，说说这两句分别返回什么、为什么要用不同的写法。

<details>
<summary>参考答案</summary>

1. `3`、`3`、`4`
2. `'Ada'`
3.
   ```js
   ;[1, 2, 3].forEach(n => console.log(n))
   ```
   （注意开头的分号：上一行没有分号时，用 `[` 开头的行可能被误认为是上一行的数组索引）
4. 第一句返回一个 `<div>` 元素；第二句返回 `4`。因为容器只有一个且要直接操作它，用 id 最快；而 4 个选项要**批量**清空/读取，只能用 class 取一组。

</details>

<a id="sec-1-5"></a>

### 1.5 随堂自测

进入 Phase 1 之前，确认下面 8 题都能答上。答不出的回到对应小节重看。

1. `id` 和 `class` 最关键的区别是什么？各写一个 CSS 选择器和 JS 选取方式。
2. 为什么 4 个 radio 的 `name` 必须完全相同？
3. `<label for="b">` 里的 `for="b"` 对应的是哪个元素？
4. `box-sizing: border-box` 解决什么问题？
5. `height: 100vh` 在 flex 居中里扮演什么角色？
6. `getElementById` 和 `querySelectorAll` 的参数写法有什么区别？
7. `innerText` 和 `innerHTML` 分别在什么场景用？
8. `${}` 写在什么引号包裹的字符串里？

<details>
<summary>参考答案</summary>

1. id 全页唯一，用 `#quiz` / `getElementById('quiz')`；class 可重复，用 `.answer` / `querySelectorAll('.answer')`。
2. `name` 是 radio 的互斥组标识，不同则可以同时选中。
3. `id="b"` 的那个 `<input type="radio">`。
4. 让 `width`/`height` 表示「含 padding 和 border 的总尺寸」。
5. 给 flex 容器一个撑满视口的高度，居中才有参照系。
6. 前者传纯 id 字符串（无 `#`）返回单个元素；后者传完整 CSS 选择器（带 `.`/`#`）返回一组。
7. 改纯文字用 `innerText`；要插入一段 HTML 结构用 `innerHTML`。
8. 反引号 `` ` ``（模板字符串）。

</details>

---

<a id="sec-2"></a>

## 2 核心操作技能：Emmet

<a id="sec-2-1"></a>

### 2.1 Emmet 是什么

Emmet 是 VS Code **内置**的缩写展开功能（不需要额外安装）。你敲一段极短的符号，编辑器按下 `Tab`，它自动展开成完整的 HTML 或 CSS。

```text
ul>li*3     +  Tab      →     <ul>
                                 <li></li>
                                 <li></li>
                                 <li></li>
                             </ul>
```

**为什么要学它？** 本项目的 `index.html` 里有一个「4 个选项」的结构，四组几乎一模一样的标签，一共约 16 行。用 Emmet：

- 手写：约 40 秒，且容易漏引号；
- Emmet：敲 `ul>li*4` 按 Tab，5 秒生成，再 Tab 键在 4 个 `li` 之间跳转填写。

对于「结构重复度高」的 HTML，效率差距是数量级的。

<a id="sec-2-2"></a>

### 2.2 缩写的八条通用规则

| # | 规则 | 写法 | 展开结果 |
| --- | --- | --- | --- |
| 1 | **`>` 子元素** | `div>p` | `<div><p></p></div>` |
| 2 | **`+` 相邻兄弟** | `p+ul` | `<p></p><ul></ul>` |
| 3 | **`^` 向上返回父级** | `div>p^span` | `<div><p></p><span></span></div>` |
| 4 | **`*` 重复** | `li*4` | `<li></li>` × 4 |
| 5 | **`#` id** | `#quiz` | `<div id="quiz"></div>` |
| 6 | **`.` class** | `.answer` | `<div class="answer"></div>` |
| 7 | **`[]` 属性** | `input[type=radio]` | `<input type="radio">` |
| 8 | **`{}` 文本内容** | `button{Submit}` | `<button>Submit</button>` |

补充三条常用的：

| 规则 | 写法 | 展开结果 |
| --- | --- | --- |
| 类名可多个 | `.box.card` | `<div class="box card"></div>` |
| 编号递增 | `li.item$*3` | `class="item1" item2 item3` |
| 无序文本 | `p.lead` 后 Tab 后打 lorem | 自动填一段占位英文 |

**`^` 连续用多次可以连跳多层**：`^^button` 表示向上两层再放一个 button。这个技巧在 2.4 会用到。

<a id="sec-2-3"></a>

### 2.3 Emmet 速查表

#### HTML 常用

| 你想要 | 敲这个 + Tab |
| --- | --- |
| 完整 HTML5 骨架 | `html:5`（或 `!`） |
| 一个 div | `div` |
| 带 id 的 div | `div#wrap` 或 `#wrap` |
| 带 class 的 div | `div.box` 或 `.box` |
| 嵌套两层 | `div>div` |
| 三个段落 | `p*3` |
| 无序列表 | `ul>li` |
| 4 个列表项 | `ul>li*4` |
| 带属性的 input | `input[type=text][name=username]` |
| 带文字的按钮 | `button#submit{Submit}` |
| 带文字的标题 | `h2#question{Question text}` |
| 单选框 + 标签 | `input[type=radio][name=answer][id=a].answer+label[for=a]#a_text` |

#### CSS 常用

| 你想要 | 敲这个 + Tab |
| --- | --- |
| 通配选择器 | `*` |
| 类选择器 | `.quiz-container` |
| id 选择器 | `#quiz` |
| 后代选择器 | `ul li` |
| 伪类 | `button:hover` / `button:focus` |
| 10px 内外边距 | `m10` |
| 20px 下外边距 | `mb20` |
| 1rem 内边距 | `p1rem` |
| 宽 600px | `w600` |
| 圆角 10px | `br10` |
| 背景色 | `bg#fff` / `bgc#8e44ad` |
| 文字颜色 | `c#fff` |
| 字号 1.2rem | `fs1.2rem` |

**CSS 里也能用结构缩写**，用来批量生成选择器骨架：

```text
body>.quiz-container>.quiz-header>h2+ul>li*4      +  Tab
```

展开为：

```css
body > .quiz-container > .quiz-header > h2 { }
body > .quiz-container > .quiz-header > ul { }
body > .quiz-container > .quiz-header > ul > li { }
```

<a id="sec-2-4"></a>

### 2.4 用 Emmet 打出 quiz-app 的完整骨架

现在做一次**完整演练**，目的不是记住结果，而是让你确信「Emmet 能把本项目的结构完整产出」。

> 如果此刻还没装好环境，可以先跳过本节，Phase 1 会再完整走一遍。

**Step A｜生成骨架并清空 body**

在空文件里敲：

```text
html:5
```

按 `Tab`，得到标准骨架。把光标移到 `</body>` 的前一行。

**Step B｜一次打出整个卡片结构**

敲：

```text
.quiz-container#quiz>.quiz-header>h2#question+ul>li*4^^button#submit{Submit}
```

按 `Tab`，得到：

```html
<div class="quiz-container" id="quiz">
  <div class="quiz-header">
    <h2 id="question"></h2>
    <ul>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
    </ul>
  </div>
  <button id="submit">Submit</button>
</div>
```

拆解这串符号，巩固规则 1/2/3/4/5/6/8：

| 片段 | 含义 |
| --- | --- |
| `.quiz-container#quiz` | 卡片：class 和 id 同时有 |
| `>` | 进入卡片内部 |
| `.quiz-header` | 题头区域 |
| `>` | 进入题头内部 |
| `h2#question` | 题干：2 号标题 + id |
| `+` | 同级兄弟，开始列表 |
| `ul>li*4` | 4 个选项容器 |
| `^^` | 向上**两层**（跳出 `li` → 跳出 `ul`）回到 `.quiz-header`… 再向上到 `.quiz-container` |
| `button#submit{Submit}` | 按钮（注意 `^` 是两个，回到 `.quiz-container` 层级） |

**Step C｜用 Tab 键在重复结构间跳转**

展开后，光标停在**第一个 `li` 内部**。此时直接按 `Tab`，光标会跳到第二个 `li` 内，再按跳到第三个…… 这是 Emmet 的「tab stop」机制，用来批量填相同结构的标签，极其省事。

> 注意：Tab 跳转会消耗你的缩进配置。如果按 Tab 后 VS Code 插入的是空格而不是跳到下一个位置，说明当前缩进是空格。把它临时改成 Tab，或改用 `Ctrl+Alt+→`（Windows）跳到下一个匹配标签。

**Step D｜填写 4 个选项**

在每个 `li` 里敲：

```text
input[type=radio][name=answer][id=a].answer+label[for=a]#a_text
```

按 `Tab`，得到：

```html
<input type="radio" name="answer" id="a" class="answer" />
<label for="a" id="a_text"></label>
```

**Step E｜逐个替换字母和文字**

| li | id | label 的 id | label 文字 |
| --- | --- | --- | --- |
| 1 | `a` | `a_text` | Java |
| 2 | `b` | `b_text` | C |
| 3 | `c` | `c_text` | Python |
| 4 | `d` | `d_text` | JavaScript |

**Step F｜验证**

打开浏览器（此时还没有 CSS，页面会是白底黑字的裸结构），确认：

- 有一个大标题，下面 4 行文字；
- 每一行左边有一个**可以点亮的圆圈**；
- **点圆圈旁边的文字**，圆圈也会被选中 ← 这一条是 `for` 写对了的证据；
- 点第二个圆圈，第一个会自动取消 ← 这一条是 `name` 写对了的证据。

Phase 1 会把这件事拆成 4 个可单独验证的小步。请务必亲手重做一遍，不要直接进入 Phase 1。

<a id="sec-3"></a>

## 3 开发环境与 VS Code 必备配置

本实验不需要 Node.js、不需要任何构建工具，全部工作在这三个文件里完成。环境配置的目标只有一个：**让你写完一行代码，1 秒内看到效果**。

<a id="sec-3-1"></a>

### 3.1 安装 VS Code

1. 访问 <https://code.visualstudio.com/Download>，下载与你操作系统匹配的**稳定版**；
2. 安装时勾选 **"Add to PATH"**（Windows）；
3. 安装过程中如果弹出「推荐其他扩展」，可以关掉，我们自己一个个来。

**打开项目目录**：启动 VS Code → 菜单 `文件 / File` → `打开文件夹 / Open Folder` → 选中你准备练习的目录（**比如 `D:\quiz-web\lab\quiz-app`**）。

> **一定选「打开文件夹」而不是「打开文件」。** 打开文件夹后，VS Code 才会建立工作区，左侧文件树、相对路径、Emmet 行为、格式化设置才稳定可用。

<a id="sec-3-2"></a>

### 3.2 Live Server：实时预览服务器

**为什么需要它？** 双击打开 `index.html`，浏览器地址是 `file:///D:/.../index.html`，这是「本地文件」模式。它能做简单页面，但：

- `fetch()` 请求本地 JSON 会被浏览器安全策略拦截（本实验暂时用不到，但以后会）；
- 没有「保存即刷新」，每次都要手动 `Ctrl+R`；
- 部分浏览器对 `file://` 下的模块脚本有额外限制。

Live Server 会启动一个本地小服务器，把地址变成 `http://127.0.0.1:5500/index.html`，并且**你一保存文件，浏览器自动刷新**。

**安装**

1. `Ctrl + Shift + X` 打开左侧扩展面板；
2. 搜索框输入 `Live Server`；
3. 找到作者为 **Ritwick Dey** 的 `Live Server`（安装量 5000 万+，是那个）；点 **Install**。

**使用**

1. 左侧文件树中右键 `index.html` → **Open with Live Server**；
2. 浏览器自动打开，地址形如 `http://127.0.0.1:5500/index.html`；
3. 之后每次在 VS Code 里 `Ctrl + S` 保存，浏览器**自动刷新**。

**停止**：浏览器右下角状态栏会出现 `Port: 5500` 的提示，点它，或按 `Ctrl + C`（在 VS Code 的终端里）。

**常用设置**

`Ctrl + Shift + P` → 输入 `Preferences: Open User Settings (JSON)`，加入：

```json
{
  "liveServer.port": 5500,
  "liveServer.openExternalFiles": false
}
```

| 设置 | 作用 |
| --- | --- |
| `liveServer.port` | 端口被占用时换一个（例如 5501） |
| `liveServer.openExternalFiles` | 设为 `false` 时，右键非 HTML 文件不会误触发启动 |

<a id="sec-3-3"></a>

### 3.3 Prettier：代码格式化

**为什么需要它？** 手敲 HTML 极易出现缩进乱、引号不统一。格式化工具让代码**自动对齐**，diff 也干净。

**安装**

1. `Ctrl + Shift + X` → 搜索 `Prettier - Code formatter`；
2. 作者是 **Prettier** 的那个 → **Install**。

> Prettier 自带对 HTML / CSS / JavaScript / JSON / Markdown 的支持，**不需要**再装 html-prettier 之类的子插件。

**配置为默认格式化器**

1. `Ctrl + Shift + P` → 输入 `Preferences: Open User Settings (JSON)`；
2. 确认里面有这两项：

```json
{
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true
}
```

- `editor.defaultFormatter` 里的 `esbenp.prettier-vscode` 就是 Prettier 插件的 ID；
- `editor.formatOnSave: true` 表示**保存时自动格式化**。

**不想让保存时自动格式化？** 把 `formatOnSave` 改回 `false`，需要时按 `Shift + Alt + F` 手动格式化当前文件。实验早期建议保持 `false`，避免格式化在你写到一半时改动代码。

**为项目加一份 Prettier 配置（重要）**

在本项目根目录新建文件 `.prettierrc.json`：

```json
{
  "singleQuote": true,
  "semi": false,
  "tabWidth": 2,
  "printWidth": 80,
  "endOfLine": "lf"
}
```

| 字段 | 作用 | 为什么这么设 |
| --- | --- | --- |
| `singleQuote` | 字符串用单引号 | 与原项目 `quiz-app/script.js` 一致 |
| `semi` | 不加分号 | 与原项目一致 |
| `tabWidth` | 缩进 2 个空格 | 与原项目 HTML/CSS 一致 |
| `printWidth` | 每行 80 字符后换行 | Prettier 默认值 |
| `endOfLine` | 统一换行符 | 避免 Windows/macOS 混编时整文件被标记为改动 |

> **为什么要专门配这个？** Prettier 默认用**双引号 + 分号 + 2 空格**。原项目是**单引号 + 无分号**。如果你不配这份文件，一旦触发格式化，整个 `script.js` 会瞬间变成另一副样子，你在 7.2 做「与原项目比对」时会看到满屏差异，分不清是逻辑写错还是纯格式差异。
>
> 配好之后，格式化只会改变你**真正写乱的部分**，diff 干净可比。

<a id="sec-3-4"></a>

### 3.4 中文语言包与其他推荐插件

| 插件 | 作者/搜索词 | 作用 | 必需？ |
| --- | --- | --- | --- |
| Live Server | Ritwick Dey | 本地实时预览服务器 | ✅ 必需 |
| Prettier - Code formatter | Prettier | 代码格式化 | ✅ 必需 |
| Chinese (Simplified) 中文语言包 | Microsoft | VS Code 界面汉化 | 强烈推荐 |
| Live Preview | Microsoft | 另一个预览服务器，功能更强 | 可选（与 Live Server 二选一即可） |
| Error Lens | Alexander | 把错误信息直接显示在代码行尾 | 推荐 |
| HTML CSS Support | Junbin Chen | 在 HTML 里自动补全 class / id 选择器 | 推荐 |
| ESLint | Microsoft | JavaScript 实时报错（少写分号会提示等） | 进阶，可后装 |

**中文语言包安装后需要重启生效**：安装完成后 VS Code 右下角会弹提示，点 **Change Language and Restart**。

> **避免插件冲突**：`Live Server` 和 `Live Preview` 都提供右键「Open with ...」菜单，功能重叠。**只装一个**，推荐 `Live Server`（本实验够用）。

<a id="sec-3-5"></a>

### 3.5 设置汇总与配置方法

**打开设置 JSON 的两种方式**：

- 界面：`Ctrl + ,` → 搜索框输入 `format on save` → 勾选；
- 文本：`Ctrl + Shift + P` → `Preferences: Open User Settings (JSON)`。

**本实验推荐的完整 User Settings**：

```json
{
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true,
  "editor.tabSize": 2,
  "editor.insertSpaces": true,
  "files.autoSave": "afterDelay",
  "files.eol": "\n",
  "emmet.showExpandedAbbreviation": "always",
  "emmet.includeLanguages": {
    "javascript": "html",
    "typescriptreact": "html",
    "vue": "html"
  },
  "liveServer.port": 5500
}
```

逐条说明：

| 设置项 | 作用 | 说明 |
| --- | --- | --- |
| `editor.defaultFormatter` | 默认格式化器 = Prettier | 让 `Shift+Alt+F` 生效 |
| `editor.formatOnSave` | 保存时自动格式化 | 写代码时保持整齐 |
| `editor.tabSize` / `editor.insertSpaces` | 缩进 2 个空格 | 与项目风格一致 |
| `files.autoSave` | 停止输入 1 秒后自动保存 | 配合 Live Server，**不用按 Ctrl+S 也能看到效果** |
| `files.eol` | 换行符统一为 LF | 避免 Windows 下和原项目 diff 全红 |
| `emmet.showExpandedAbbreviation` | 输入时实时预览展开结果 | `"never"` / `"always"` / `"inMarkupAndStylesheetFiles"` |
| `emmet.includeLanguages` | 让 `.js` 文件里也能用 HTML Emmet | 例如在 JS 里拼模板字符串时有用 |
| `liveServer.port` | 预览服务器端口 | 冲突时改 |

> **`files.autoSave` 是本实验的效率开关。** 开启后你写完 CSS 会自动存盘，浏览器自动刷新，不用手动保存 + 刷新两个动作。

<a id="sec-3-6"></a>

### 3.6 环境验收

在动手写项目之前，先花 5 分钟确认环境可用。

**Step 1｜新建临时文件确认 Emmet**

新建 `test.html`，输入 `ul>li*3` 后按 `Tab`。

- **预期**：自动展开为 `<ul>` + 3 个 `<li>`，屏幕右下方出现黄色提示 `Emmet Abbreviation expanded`。

**Step 2｜新建文件确认 Prettier**

新建 `test2.html`，故意写成乱糟糟的缩进，按 `Ctrl + S`。

- **预期**：保存瞬间缩进被自动整理整齐。

**Step 3｜确认 Live Server**

右键 `test.html` → **Open with Live Server**。

- **预期**：浏览器打开，地址栏是 `http://127.0.0.1:5500/test.html`（**不是** `file:///`）。

**Step 4｜确认开发者工具**

在浏览器里按 `F12`。

- **预期**：DevTools 面板打开，能看到 `Elements`、`Console`、`Sources` 几个标签页。

**Step 5｜确认 Git**

打开 VS Code 集成终端（`Ctrl + ~`），执行：

```bash
git --version
git config --list
```

- **预期**：第一条输出 `git version x.x.x`；第二条能看到 `user.name` 和 `user.email`（如果你还没配，见 [4.2](#sec-git-2)）。

五项全过 → 删掉两个测试文件，进入 [第 4 章](#sec-git) 配置 Git，然后走 [Phase 0](#sec-phase-0)。
任意一项不过 → 回到对应小节排查，**不要硬着头皮往下走**。

> **VS Code 的源代码管理面板此时应该能用。** 左侧资源管理器下方的「源代码管理」图标（三根分叉的线）已经出现，只是当前目录还不是 Git 仓库，所以是灰的。等 Phase 0 执行了 `git init`，它就会亮起来——那时你可以用图形界面点「提交」按钮来代替敲 `git commit`，两条路完全等价。

<a id="sec-git"></a>

## 4 Git 版本管理基础与实战

本实验从 Phase 0 到 Phase 7，一共要写 200 行左右代码、反复改十几个版本。**如果没有版本管理，只要 Phase 5 改坏了 script.js，你既不知道哪一行是错的，也回不到 Phase 4 那个能跑的版本。**

Git 解决的就是这件事。本章只讲**你在本实验里真正用得到的部分**，讲完在 4.7 动手铺一条版本线，然后从 Phase 0 开始，每个 Phase 结束打一个标签。

### <a id="sec-git-1"></a>4.1 Git 是什么：三个区域

Git 的核心模型只有三个区域，理解了它，Git 的全部命令就都能推出来了。

```
  工作区                    暂存区              版本库
(你正在编辑的文件)  →  git add  →  (下次提交的内容)  →  git commit  →  (历史记录)
  Working Directory           Staging Area              Repository
     index.html                   ...                      commit
     style.css
     script.js
```

**一句话记住流程**：**改 → add → commit**。

| 动作 | 你在做什么 |
| --- | --- |
| 你在 VS Code 里敲字 | 改动进入**工作区** |
| `git add .` | 把改动放进**暂存区**，排队等着被记录 |
| `git commit -m "说明"` | 把暂存区的内容**永久写进历史**，附一条说明 |

**为什么要有暂存区这么麻烦的一层？** 因为它让你能**选择性地提交**。改了 3 个文件，但只想提交 1 个，就可以只 `add` 那一个；或者反过来，先全 add，再把某个文件 `restore --staged` 撤出来。

**两条关键认知**

1. **Git 只记录「文件的快照」，不记录你敲代码的过程。** 一次提交 = 一张所有文件的照片。哪怕你改了一行，Git 存的是改完后全部内容的新快照。
2. **你不 `add`，Git 就不知道。** 这是新手最常见的困惑：「我明明改了文件，为什么 `git status` 说没有变化？」——因为它只看你**暂存区**的状态，不看工作区。

### <a id="sec-git-2"></a>4.2 安装 Git 与首次配置

**安装**

| 系统 | 方式 |
| --- | --- |
| Windows | 访问 <https://git-scm.com/download/win>，下载安装，全程默认下一步 |
| macOS | 系统通常已自带；没有的话执行 `brew install git`，或安装 Xcode Command Line Tools |
| Linux | `sudo apt install git` |

**验证安装**：打开 VS Code 的集成终端（`Ctrl + ~`），执行

```bash
git --version
```

**预期**：输出类似 `git version 2.47.1`。

> 如果提示「不是内部或外部命令」，说明没装好或没加入 PATH，**重启 VS Code**（安装时勾了 Add to PATH 的话，重启即可生效）。

**首次配置（必做）**

Git 每次提交都要记录「谁提交的」，所以必须先告诉它你的身份：

```bash
git config --global user.name "你的名字"
git config --global user.email "you@example.com"
```

**再配三条对本实验有用的项**

```bash
git config --global init.defaultBranch main
git config --global core.quotepath false
git config --global core.autocrlf input
```

| 配置 | 作用 |
| --- | --- |
| `user.name` / `user.email` | 提交者身份，会写进每一条历史记录 |
| `init.defaultBranch main` | 新仓库的默认分支叫 `main`（而不是老的 `master`） |
| `core.quotepath false` | Windows 上 `git status` 能正常显示中文文件名，不再显示成 `"\"\346\226\207\346\241\214.txt\""` |
| `core.autocrlf input` | 提交时统一转成 LF 换行，避免 Windows 的 CRLF 让整个文件显示为「已修改」 |

**验证配置生效**

```bash
git config --list
```

**预期**：能看到 `user.name=你的名字` 和 `user.email=...`。

### <a id="sec-git-3"></a>4.3 核心命令速查

下面这些命令**覆盖了本实验的全部需求**。记不住没关系，随时回来查这张表。

#### 查看状态（最高频）

```bash
git status
```

告诉 Git「在哪个目录」「当前分支是什么」「哪些文件改了」「哪些已经 add 了」。

> **养成习惯：每次提交前先敲 `git status`。** 它同时是你的「防手滑检查清单」——如果你以为只改了 3 个文件，结果 `status` 显示 7 个文件都变了，那多半是 VS Code 自动格式化动了别的文件，值得停下来看一眼。

#### 暂存与提交

```bash
git add .                    # 把当前目录所有改动放进暂存区
git add index.html           # 只暂存指定文件
git commit -m "feat: 添加题库数据 quizData"
```

> **提交信息怎么写？** 本实验统一用这个格式：`<类型>: <一句话说明>`
>
> | 类型 | 用于 |
> | --- | --- |
> | `feat` | 新功能（本实验的绝大多数提交） |
> | `style` | 只改样式，不改逻辑 |
> | `fix` | 修 bug |
> | `chore` | 工程杂务（初始化、配置文件） |
> | `test` | 测试与验收 |
>
> 说明用中文即可，但**要具体**。❌ `修改` ✅ `feat: 用 innerHTML 替换结算页内容`

#### 查看历史与差异

```bash
git log --oneline --decorate --graph --all
git show <commit>            # 看某次提交的完整内容
git diff                     # 工作区 vs 暂存区（还没 add 的改动）
git diff --staged            # 暂存区 vs 上次提交（已 add 待提交的改动）
```

`git log --oneline --decorate` 是**本实验最有用的命令**——`--decorate` 会把每个提交旁边的**分支名和标签名**显示出来：

```
* 8f2a1c3 (HEAD -> main, tag: v1.0-final) test: 联调验收并与原项目自查比对
* 1a7b3d9 (tag: v0.7-result) feat: 完成结算页与重新开始
* 5c2e8f4 (tag: v0.6-scoring) feat: 实现判题与计分逻辑
```

一眼就能看出「哪个提交打了哪个标签」。

#### 回退与撤销

| 场景 | 命令 | 效果 |
| --- | --- | --- |
| 改乱了，**还没 add** | `git restore index.html` | 把该文件丢弃，回到上次提交的样子 |
| 已 add，但**还没 commit** | `git restore --staged index.html` | 从暂存区撤出，文件内容保留 |
| commit 了，但**想保留改动重新组织** | `git reset --soft HEAD~1` | 撤掉这次提交，改动回到暂存区 |
| 误删了文件 | `git restore index.html` | 恢复 |

> **`restore` 和 `reset` 的区别**：`restore` 管**文件**，`reset` 管**提交**。初学阶段只记 `restore` 就行，`reset --soft` 知道有这回事即可。

#### 检出历史版本（只读参观）

```bash
git checkout v0.3-css        # 把工作区变成 v0.3-css 那一刻的样子
git switch main              # 回到主线最新状态
```

> **注意**：检出标签后 Git 会提示 `You are in 'detached HEAD' state`（你处于游离头指针状态）。这不是错误，意思是「你正看着一个历史快照，不在任何分支上」。**看完必须 `git switch main` 回来**，否则你接下来写的东西不会属于主线。

#### 查看某个标签具体有什么

```bash
git show v0.3-css           # 看这个标签指向的提交详情
git tag -n1                 # 列出所有标签及其说明
```

### <a id="sec-git-4"></a>4.4 分支 branch

**分支是干什么的？** 让你在不破坏主线的前提下试验新想法。

本实验的 Phase 0–7 是一条**固定的线性主线**，不需要分支。但有两种情况建议开分支：

| 场景 | 做法 |
| --- | --- |
| Phase 5 想换一种写法试试 | `git switch -c try-alternative`，放心折腾，失败了切回来 |
| 想做 7.3 的进阶挑战 | `git switch -c challenge-1`，挑战完打标签，主线保持干净 |

**核心命令**

```bash
git branch                    # 列出本地分支（* 表示当前所在）
git switch -c challenge-1     # 新建并切换到该分支
git switch main               # 切回主线
git branch -d challenge-1     # 删掉分支（-d 要求分支内容已合并到主线）
git switch challenge-1        # 切回分支继续做
```

**分支和标签的分工**

| | 分支 branch | 标签 tag |
| --- | --- | --- |
| 会移动吗 | **会**，每提交一次就往前挪 | **不动**，死死钉在某个提交上 |
| 用来表达 | 「我正在这条路上继续开发」 | 「这一刻是可交付的稳定版本」 |
| 本实验中的角色 | 可选的试验场 | **Phase 0–7 的存档点** |

### <a id="sec-git-5"></a>4.5 标签 tag：给每个阶段存档

这是本章的重点。**本实验约定：每个 Phase 完成后打一个标签，形成一条完整的时间线。**

**两种标签**

| 类型 | 命令 | 特点 |
| --- | --- | --- |
| 轻量标签 | `git tag v0.3-css` | 只是一个指针，不带说明。**本实验不用** |
| 附注标签 | `git tag -a v0.3-css -m "说明"` | 带说明文字和时间戳。**本实验统一用这个** |

> **为什么要用 `-a`？** 半年后你看到 `v0.5-render` 这个标签，`git show` 出来能直接告诉你「这是 Phase 4，做的是 DOM 元素获取和题目渲染」。轻量标签只有一个光秃秃的名字，等于没写笔记。

**命令格式**

```bash
git tag -a <标签名> -m "<一句话说明>"
```

**本实验的标签约定表**

| Phase | 标签名 | 标签说明（`-m` 后面写这个） |
| --- | --- | --- |
| 演练起点 | `v0.0-start` | Phase 之前的空工程起点 |
| Phase 0 工程初始化 | `v0.1-init` | 空目录跑通 Hello Quiz，三个文件互相连通 |
| Phase 1 HTML 骨架 | `v0.2-html` | Emmet 搭出卡片/题干/4选项/按钮，选项可点且互斥 |
| Phase 2 CSS 视觉 | `v0.3-css` | 渐变背景+白色卡片+紫色按钮，视觉对齐原项目 |
| Phase 3 题库数据 | `v0.4-data` | quizData 4 道题，Console 可打印 |
| Phase 4 渲染题目 | `v0.5-render` | 8 个 DOM 引用+loadQuiz，页面显示第 1 题 |
| Phase 5 判题计分 | `v0.6-scoring` | getSelected+判题+切题，能答完 4 题 |
| Phase 6 结算页 | `v0.7-result` | innerHTML 结算页+Reload 重新开始 |
| Phase 7 联调验收 | `v1.0-final` | 全部验收项通过，与原项目自查一致 |

**命名规律**：`v主版本.次版本-阶段含义`

- `v0.x` 表示「还没做完，做了一半」；
- `v1.0` 表示「第一个完整可交付版本」；
- `-css`、`-render` 这段让你不看提交信息也能想起来。

**常用标签操作**

```bash
git tag                          # 列出全部标签
git tag -n1                      # 列出标签 + 说明
git show v0.3-css                # 看这个标签指向哪次提交
git tag -d v0.3-css              # 删标签（代码不动）
git tag -f -a v0.3-css -m "新说明"   # 移动标签到当前提交
```

> **标签打错了要重来？** 用 `git tag -f -a <名字> -m "<新说明>"`。**但不要改已完成的 Phase 的标签**——那等于篡改实验记录，7.2 的自查就失去意义了。

### <a id="sec-git-6"></a>4.6 .gitignore：该忽略什么

`.gitignore` 告诉 Git「哪些文件不要记录」。

**本实验的 `.gitignore`**（在 `lab/quiz-app/` 下创建）

```gitignore
# 系统垃圾文件
.DS_Store
Thumbs.db
desktop.ini

# 编辑器个人配置（每个人不一样，不该进历史）
.vscode/*
!.vscode/settings.json

# 依赖与构建产物（本实验用不到，先留着以防万一）
node_modules/
dist/
```

**三条使用规则**

1. **`.gitignore` 只对「未跟踪」的文件生效。** 如果某个文件你**已经 add 过**，后来才加进 `.gitignore`，它仍会被继续跟踪。要先取消跟踪：

   ```bash
   git rm --cached <文件路径>
   ```

2. **不要忽略 `index.html`、`style.css`、`script.js`。** 这三个文件正是实验的全部产出，忽略了等于白做。
3. **别忽略 `.prettierrc.json`。** 它属于工程配置，应该进版本库。

**如何确认它生效了**

```bash
git status
```

**预期**：`Untracked files`（未跟踪文件）列表里**只有**你的 `index.html`、`style.css`、`script.js`，没有任何系统垃圾文件。

### <a id="sec-git-7"></a>4.7 实战演练：为练习工程铺一条版本线

> **做这一步之前**，请先完成 [5 Phase 0 的 Step 0.1](#sec-phase-0)，确保 `lab/quiz-app/` 目录和三个空文件已经建好。

#### Step G.1｜初始化仓库

**操作**：VS Code 集成终端（`Ctrl + ~`）里确认当前目录是 `lab/quiz-app`，然后执行：

```bash
git init
```

**预期输出**

```
Initialized empty Git repository in D:/quiz-web/lab/quiz-app/.git/
```

**测试步骤**

```bash
git status
```

**预期**：`On branch main` 开头，并列出 `index.html`、`script.js`、`style.css` 三个 `Untracked files`（未跟踪文件）。

> **报错 `fatal: not a git repository`？** 你不在 `lab/quiz-app` 目录里。用 `cd lab/quiz-app` 切过去。

#### Step G.2｜建 .gitignore

**操作**：新建文件 `.gitignore`，内容见 4.6。

**测试步骤**

```bash
git status
```

**预期**：`.gitignore` 自己出现在未跟踪列表里（它确实该被跟踪）。如果列表里还有 `Thumbs.db` 之类，说明规则没写对。

#### Step G.3｜打第一个提交和标签

**操作**

```bash
git add .
git status
git commit -m "chore: 初始化 quiz-app 练习工程"
git tag -a v0.0-start -m "三个空文件 + .gitignore，练习正式开始前的起点"
git log --oneline --decorate -1
```

**逐条解释**

| 命令 | 作用 |
| --- | --- |
| `git add .` | 暂存当前目录下所有文件（`.` = 当前目录） |
| `git status` | **提交前必看**，确认将要提交的东西和你想的一致 |
| `git commit -m "..."` | 把暂存区内容写入历史 |
| `git tag -a v0.0-start -m "..."` | 在当前提交上打一个附注标签 |
| `git log --oneline --decorate -1` | 看最近 1 条提交，含标签显示 |

**测试步骤**

1. `git status` → **预期**：`nothing to commit, working tree clean`（没有待提交的改动，工作区干净）
2. `git tag` → **预期**：输出 `v0.0-start`
3. `git tag -n1` → **预期**：`v0.0-start  三个空文件 + .gitignore，练习正式开始前的起点`

#### Step G.4｜演练「写坏了怎么救回来」

这一步**故意搞破坏**，让你亲眼看到 Git 的价值。

**测试步骤**

1. 在 `index.html` 里随便乱敲几行（比如删掉 `</body>`，或者把标题改成乱码）；
2. 浏览器刷新，确认页面**已经坏了**；
3. 在终端执行：

   ```bash
   git status
   ```

   **预期**：`Changes not staged for commit:` 下方出现 `modified: index.html`。

4. 执行回退：

   ```bash
   git restore index.html
   ```

   **预期**：终端无输出（`restore` 成功时是安静的）。

5. 浏览器刷新。

   **预期**：页面**完好如初** —— 因为三个文件本来是空的，恢复成空文件是正确的。

6. 再执行一次 `git status`。

   **预期**：`nothing to commit, working tree clean`。**工作区已经被完全还原了**。

> 如果三个文件此时已经有内容（说明你提前做了 Phase 0），恢复后也回到「最近一次提交时的样子」。**这就是每个 Phase 结束必须打标签的原因**：标签越密，你能回退的颗粒度越细。

#### Step G.5｜演练「试验不想要主线」

**测试步骤**

1. 执行：

   ```bash
   git switch -c sandbox
   git branch
   ```

   **预期**：第一句输出 `Switched to a new branch 'sandbox'`；`git branch` 输出两行，`*` 号标记在 `sandbox` 上。

2. 在 `style.css` 里乱写一些内容并存盘；
3. 浏览器刷新看看效果；
4. 执行：

   ```bash
   git switch main
   ```

   **预期**：`Switched to branch 'main'`，且 **`style.css` 的改动自动消失了**——因为那些改动没提交，只存在于工作区里，切分支时被 Git 丢弃了。浏览器刷新后也回到了原来的样子。

5. 清理：

   ```bash
   git branch -d sandbox
   git branch
   ```

   **预期**：输出含 `Deleted branch sandbox`；`git branch` 只剩 `* main` 一行。

> **这个练习真正的收获**：你亲眼看到了 Git 的两条核心保证——
>
> - **未提交的改动**：`git switch` 会直接把它丢掉（这就是为什么每次切分支前要先 commit 或 stash）；
> - **已提交的改动**：切分支时 Git 会把工作区恢复成分支上的样子，改动永远跟着分支走，不会串。
>
> 这两条理解到位了，你就不会再出现「切了分支代码莫名其妙没了」或「改完切回来代码还在」的困惑。

> **注意**：练习中的 `git init`、`.gitignore`、第一次提交与 `v0.0-start` 标签，都是在 [5 Phase 0 Step 0.1](#sec-phase-0) 里真正做的。本节 4.7 只是**提前练一遍命令**，把两次做不会浪费额外时间。如果你想边读边上手，就在这里做完；如果 [Phase 0](#sec-phase-0) 就在眼前，直接跳过本节去 Phase 0 做也一样。

#### Step G.6｜收尾自检

**测试步骤**

执行下面三条命令，确认输出符合预期。

```bash
git log --oneline --decorate
```

**预期**：至少能看到两条提交，每条旁边挂着对应标签，形如：

```
* 1a2b3c4 (HEAD -> main, tag: v0.1-init) chore: 初始化练习工程并跑通 Hello Quiz
* 5d6e7f8 (tag: v0.0-start) chore: 初始化 quiz-app 练习工程
```

```bash
git status
```

**预期**：`On branch main` + `nothing to commit, working tree clean`。

```bash
git tag -n1
```

**预期**：两个标签都带说明文字。

**G.6 已经就绪**，接下来进入 [5 Phase 0](#sec-phase-0)。

### <a id="sec-git-8"></a>4.8 Git 排错速查

| 你看到的报错 | 原因 | 解决 |
| --- | --- | --- |
| `Please tell me who you are` / `Author identity unknown` | 没配 `user.name` / `user.email` | 执行 4.2 的两条 `git config --global` |
| `fatal: not a git repository` | 当前目录不是仓库 | `cd` 到 `lab/quiz-app`，或先 `git init` |
| `nothing added to commit but untracked files present` | 文件存在但**没 add** | `git add .` 再 commit |
| `fatal: pathspec 'xxx' did not match any file(s) known to git` | 文件名拼错，或文件在别的目录 | 用 `git status` 确认真实路径 |
| `fatal: tag 'v0.3-css' already exists` | 标签重名 | `git tag -f -a v0.3-css -m "说明"` 覆盖 |
| `You are in 'detached HEAD' state` | 你检出了历史标签 | 执行 `git switch main` 回到主线 |
| `LF will be replaced by CRLF`（Windows 警告） | 换行符转换 | 配 `git config --global core.autocrlf input` 消除 |
| `warning: LF will be replaced by CRLF in index.html` 之后 `git status` 永远显示已修改 | 同上 | 同上；或对该文件执行 `git add --renormalize .` |
| 改了 `.gitignore` 但文件仍被跟踪 | `.gitignore` 不影响已跟踪文件 | `git rm --cached <文件>` |
| 中文文件名显示成 `"\"\346\226\207\346\241\214.txt\""` | Git 的路径转义 | `git config --global core.quotepath false` |
| `Please commit or stash them` | 工作区有未提交改动，禁止切换分支 | 先 `git add . && git commit`，或 `git stash` |

---

<a id="sec-5"></a>

## 5 项目拆解实战

<a id="sec-5-0"></a>

### 实战总览

整个实战拆成 **8 个 Phase、28 个 Step**。

| Phase | 内容 | 产出 | 完成标志 | Git 标签 |
| --- | --- | --- | --- | --- |
| 0 | 工程初始化 | 三个空文件跑起来 | 浏览器显示 `Hello Quiz` | `v0.1-init` |
| 1 | HTML 骨架 | 结构完整、选项可选 | 点文字能选中圆圈，且互斥 | `v0.2-html` |
| 2 | CSS 样式 | 外观与原项目一致 | 渐变背景 + 白色卡片 + 紫色按钮 | `v0.3-css` |
| 3 | 题库数据 | `quizData` 数组 | Console 打印出 4 道题 | `v0.4-data` |
| 4 | DOM 与渲染 | 页面显示第 1 题 | 题干和选项变成题目内容 | `v0.5-render` |
| 5 | 判题与计分 | 能答完 4 题 | 分数正确累加 | `v0.6-scoring` |
| 6 | 结算页 | 显示得分 | 显示 `X/4` 并能重开 | `v0.7-result` |
| 7 | 联调验收 | 与原项目一致 | 通过 7.1 全部验收项 | `v1.0-final` |

**Step 的统一读法**：每个 Step 的「测试步骤」是**门禁**。跑不出预期结果，回到本 Step 排查，不要往下走。往回改的成本远低于往前背。

**Phase 的统一收尾**：每个 Phase 全部 Step 跑通后，做一次 **Git 提交 + 打标签**，再进下一个 Phase。每个 Phase 都有一段固定的「Git 版本节点」小节，命令已经写好，你只需要照敲。

**Git 在实战里的三个用途**

1. **存档**：标签把每个 Phase 冻成一个可交付状态，进度看得见（`git log --oneline --decorate`）。
2. **救场**：某个 Phase 改崩了，`git restore .` 回到上一个标签的状态，不用重写。
3. **验收**：7.2 用 `git show` 和 `git diff` 把你的最终版本和每个中间版本摊开逐个审，而不是「看着还行」。

> **开始实战前**，请先完成 [4.7 实战演练](#sec-git-7)，把 `v0.0-start` 这个起点标签打好。

---

<a id="sec-phase-0"></a>

### Phase 0 工程初始化与跑通

**本阶段目标**：把空目录变成一个能被浏览器访问的工程。

**本阶段完成标志**：浏览器里能看到 `Hello Quiz` 五个字。

#### Step 0.1｜建立练习目录并初始化 Git

**操作**

1. 在 VS Code 资源管理器中，鼠标悬停在工作区根目录上，点「新建文件夹」；
2. 文件夹命名为 `lab`；
3. 在 `lab` 里再建一个文件夹，命名 `quiz-app`；
4. 在 `lab/quiz-app` 里新建 3 个**空文件**；
5. 打开 VS Code 集成终端（`Ctrl + ~`），执行 `cd lab/quiz-app`；
6. 执行 `git init`；
7. 新建 `.gitignore`，内容见 [4.6](#sec-git-6)。

```
lab/
└── quiz-app/
    ├── .gitignore
    ├── index.html
    ├── style.css
    └── script.js
```

> **为什么叫 `lab`？** 目的是和仓库里已有的 `quiz-app/` 分开。你写的是**自己的版本**，原有的 `quiz-app/` 是**参考答案**。混淆两者会让你在验收时无法判断到底抄没抄。
>
> **三个文件必须在同一个文件夹里**，不能把 `style.css` 放进子文件夹。后面 `href="style.css"` 用的是相对路径，找不到就全盘失效。

**测试步骤**

1. 左侧文件树里能看到 `lab/quiz-app/index.html`；
2. 点开 `index.html`，是**空白**的（不是有默认内容）。

**预期**：空白编辑器窗口。

#### Step 0.2｜写入最小可运行页面

**操作**：在 `index.html` 中手敲以下内容（只有 3 行）：

```html
<h1>Hello Quiz</h1>
```

**测试步骤**

1. 在文件树上右键 `index.html` → **Open with Live Server**；
2. 浏览器自动打开；
3. 按 `F12` 打开 DevTools，切到 **Console** 标签。

**预期**

- 页面显示一个大号的 `Hello Quiz`；
- 地址栏是 `http://127.0.0.1:5500/...`，**不是** `file:///`；
- Console 里**没有任何红色报错**（有黄色提示可以忽略）。

**易错点**

| 现象 | 原因 | 解决 |
| --- | --- | --- |
| 右键菜单里没有 Live Server | 插件没装好或没启用 | 回 3.2 重装 |
| 打开的是 `file:///` | 用双击打开的，不是 Live Server | 右键菜单重新启动 |
| 端口提示被占用 | 5500 已被其他服务占用 | 改 `liveServer.port` 为 5501 |

#### Step 0.3｜建立三个文件之间的连接

**操作**：把 `index.html` 替换成下面内容（这时已经带上 CSS 和 JS 的引用，但两个文件还是空的）：

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="style.css" />
    <title>Quiz App</title>
  </head>
  <body>
    <h1>Hello Quiz</h1>
    <script src="script.js"></script>
  </body>
</html>
```

在 `style.css` 里写入一行：

```css
h1 {
  color: purple;
}
```

在 `script.js` 里写入一行：

```js
console.log('script.js 已成功加载')
```

**测试步骤**

1. 保存所有文件（`Ctrl + K` 然后 `S` 可一次保存全部）；
2. 切到浏览器，按 `Ctrl + R` 刷新；
3. 看 Console 面板。

**预期**

- `Hello Quiz` 变成**紫色** ← 说明 `style.css` 链接成功；
- Console 打印 `script.js 已成功加载` ← 说明 `script.js` 链接成功；
- Console 里**没有红色报错**。

**易错点**

> **改 `style.css` 但页面颜色没变？** 按顺序排查：
> 1. 改的是 `lab/quiz-app/style.css` 还是仓库里的 `quiz-app/style.css`？（很容易改错文件）
> 2. `style.css` 里选择器拼对了吗？`h1` 还是 `hl`？
> 3. 刷新了吗？Live Server 的自动刷新有时在某些情况下不触发，手动 `Ctrl + R` 最保险。
> 4. DevTools → Network 标签 → 刷新，看 `style.css` 这一行状态码是不是 `200`。如果是 `404`，说明路径不对。

#### 本阶段完成态

```
lab/quiz-app/
├── .git/            ← 4.7 生成的版本库
├── .gitignore       ← 4.7 建的忽略规则
├── index.html       ← 完整骨架 + Hello Quiz
├── style.css        ← h1 变紫
└── script.js        ← console.log
```

**Phase 0 通过标志**：Console 无报错、标题变紫、日志打印成功。

#### Git 版本节点：`v0.1-init`

在 VS Code 终端（`Ctrl + ~`）里确认当前目录是 `lab/quiz-app`，执行：

```bash
git status
git add .
git commit -m "chore: 初始化练习工程并跑通 Hello Quiz"
git tag -a v0.1-init -m "空目录跑通 Hello Quiz，三个文件互相连通"
git log --oneline --decorate -1
```

| 项 | 值 |
| --- | --- |
| 提交信息 | `chore: 初始化练习工程并跑通 Hello Quiz` |
| **标签** | **`v0.1-init`** |
| 标签说明 | 空目录跑通 Hello Quiz，三个文件互相连通 |

**测试步骤**

1. `git status` → **预期**：`nothing to commit, working tree clean`
2. `git tag -n1` → **预期**：`v0.1-init  空目录跑通 Hello Quiz，三个文件互相连通`
3. `git log --oneline --decorate -1` → **预期**：形如 `a1b2c3d (HEAD -> main, tag: v0.1-init) chore: 初始化练习工程并跑通 Hello Quiz`

> **`git add .` 之前务必先 `git status`。** 如果输出里出现了你没打算改的文件（比如 `style.css` 有你不小心删的内容），先问自己为什么，再决定要不要提交。

---

<a id="sec-phase-1"></a>

### Phase 1 用 Emmet 搭建 HTML 骨架

**本阶段目标**：用 Emmet 搭出完整的页面结构，让它「长得像」原项目，但**暂时没有样式**。

**本阶段完成标志**：页面上能看到一个标题、4 个可选项、1 个按钮，且选项**点文字也能选中**、**互相排斥**。

#### Step 1.1｜生成卡片容器和题头

**操作**

把光标移到 `<h1>Hello Quiz</h1>` 这一行，**整行删掉**（这一步开始不再需要它），光标停在 `<body>` 的下一行。输入：

```text
.quiz-container#quiz>.quiz-header>h2#question
```

按 `Tab`。

**预期展开**

```html
<div class="quiz-container" id="quiz">
  <div class="quiz-header">
    <h2 id="question"></h2>
  </div>
</div>
```

在 `<h2 id="question"></h2>` 里填入文字 `Question text`：

```html
<h2 id="question">Question text</h2>
```

**测试步骤**

1. 保存，浏览器自动刷新；
2. 页面上出现居左显示的 `Question text`。

**预期**：能看到这行文字，**位置偏左上、字号普通**（因为还没有 CSS）。

> 现在看不出任何样式是**正常的**。Phase 2 才上色。

#### Step 1.2｜加 4 个选项容器

**操作**：光标移到 `</h2>` 的**下一行**，输入：

```text
ul>li*4
```

按 `Tab`。

**预期展开**

```html
<ul>
  <li></li>
  <li></li>
  <li></li>
  <li></li>
</ul>
```

在每个 `<li>` 里分别填入 `Question`（和原项目一致，四个都一样，Phase 4 时 JS 会替换掉）：

```html
<li>Question</li>
```

**测试步骤**

1. 保存并刷新；
2. 页面上 `Question text` 下方出现 4 行 `Question`。

**预期**：看到 4 行项目符号列表（前面有小圆点，因为还没写 `list-style: none`）。

#### Step 1.3｜给每个选项加单选框和标签

**操作**：逐个进入 `<li>` 内（可直接把光标放在 `<li>` 和 `</li>` 之间），输入：

```text
input[type=radio][name=answer][id=a].answer+label[for=a]#a_text
```

按 `Tab`。第 1 个 `li` 展开为：

```html
<li>
  <input type="radio" name="answer" id="a" class="answer" />
  <label for="a" id="a_text">Question</label>
</li>
```

其余三个把字母依次替换：

| 第几个 `li` | `id` | `for` | label 的 `id` |
| --- | --- | --- | --- |
| 1 | `a` | `a` | `a_text` |
| 2 | `b` | `b` | `b_text` |
| 3 | `c` | `c` | `c_text` |
| 4 | `d` | `d` | `d_text` |

同时把每个 label 里的文字 `Question` 也替换成不同内容（原项目第一题内容）：

```html
<input type="radio" name="answer" id="a" class="answer" />
<label for="a" id="a_text">Java</label>

<input type="radio" name="answer" id="b" class="answer" />
<label for="b" id="b_text">C</label>

<input type="radio" name="answer" id="c" class="answer" />
<label for="c" id="c_text">Python</label>

<input type="radio" name="answer" id="d" class="answer" />
<label for="d" id="d_text">JavaScript</label>
```

**测试步骤（本阶段最重要的一步）**

1. 保存并刷新；
2. **点击 `Java` 这两个字**（不是点圆圈）；
3. 看 `Java` 左边的圆圈。

   **预期**：圆圈被填实心选中。

4. **再点击 `Python` 这两个字**；
5. 看 `Java` 的圆圈和 `Python` 的圆圈。

   **预期**：`Java` 的圆圈**自动取消**，`Python` 的圆圈被选中。

6. 依次点击 `C`、`JavaScript`，重复验证互斥。

7. 把鼠标移到选项文字上。

   **预期**：鼠标指针变成**手型**（因为 CSS 里已经写了 `cursor: pointer`——如果你还没写 CSS，这一条先跳过，Phase 2 Step 2.6 再验）。

**如果第 2 步点文字选不中** → 检查 label 的 `for` 和 input 的 `id` 是否**完全一致**（大小写、拼写）。
**如果互斥失效** → 检查 4 个 input 的 `name` 是否都是 `answer`。

#### Step 1.4｜加提交按钮

**操作**：光标移到 `</div>`（`.quiz-header` 的结束标签）**下一行**，注意要**跳出 `.quiz-header`**，落在 `.quiz-container` 内部。输入：

```text
button#submit{Submit}
```

按 `Tab`。

**预期展开**

```html
<button id="submit">Submit</button>
```

**测试步骤**

1. 保存并刷新；
2. 点击 `Submit` 按钮。

**预期**：按钮**能被点到、有按下效果**，但**页面什么都不会发生**——因为脚本还没写。这正是当前的正确表现。

3. 把鼠标移到按钮上悬停。

   **预期**：**目前没有变色效果**（CSS 还没写）。Phase 2 Step 2.7 会补上。

#### 本阶段完成态

`index.html` 完整内容应该是：

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="style.css" />
    <title>Quiz App</title>
  </head>
  <body>
    <div class="quiz-container" id="quiz">
      <div class="quiz-header">
        <h2 id="question">Question text</h2>
        <ul>
          <li>
            <input type="radio" name="answer" id="a" class="answer" />
            <label for="a" id="a_text">Java</label>
          </li>

          <li>
            <input type="radio" name="answer" id="b" class="answer" />
            <label for="b" id="b_text">C</label>
          </li>

          <li>
            <input type="radio" name="answer" id="c" class="answer" />
            <label for="c" id="c_text">Python</label>
          </li>

          <li>
            <input type="radio" name="answer" id="d" class="answer" />
            <label for="d" id="d_text">JavaScript</label>
          </li>
        </ul>
      </div>
      <button id="submit">Submit</button>
    </div>
    <script src="script.js"></script>
  </body>
</html>
```

**Phase 1 通过标志**：点文字能选中、四个选项互斥、按钮存在且可点。

#### Git 版本节点：`v0.2-html`

```bash
git status
git add .
git commit -m "feat: 用 Emmet 搭建 quiz-app 的 HTML 骨架"
git tag -a v0.2-html -m "卡片/题干/4 选项/按钮结构完成，选项可点且互斥"
git log --oneline --decorate -1
```

| 项 | 值 |
| --- | --- |
| 提交信息 | `feat: 用 Emmet 搭建 quiz-app 的 HTML 骨架` |
| **标签** | **`v0.2-html`** |
| 标签说明 | 卡片/题干/4 选项/按钮结构完成，选项可点且互斥 |
| 本 Phase 预期改动 | **只有 `index.html`**（`style.css` 和 `script.js` 应该还是空的） |

**测试步骤**

1. `git status` → **预期**：只有 `index.html` 出现在待提交列表里。
2. `git tag -n1` → **预期**：`v0.2-html  卡片/题干/4 选项/按钮结构完成，选项可点且互斥`
3. `git show --stat v0.2-html` → **预期**：这次提交只动了 `index.html` 一个文件。

> **第 3 步是个很值钱的自检。** 如果它显示 `style.css` 和 `script.js` 也被提交了，说明你在 Phase 1 不小心动了它们——用 `git show v0.2-html` 看看具体改了什么，早发现早回退。

> **两处刻意的差异，不用改**：和参考项目 `quiz-app/index.html` 相比，上面的完成态有两点不同——
>
> 1. `<input ... />` 末尾多了一个斜杠。HTML5 里 `<input>` 是**空元素（void element）**，加不加 `/` 都能正确解析。VS Code 的 Emmet 默认会加 `/`，Prettier 也保留它，两种写法完全等价。
> 2. 选项文字填的是 `Java` / `C` / `Python` / `JavaScript`，而不是参考项目里的占位符 `Question`。因为你马上就要写脚本去替换它们，先填真实内容更好调试。
>
> 这两点**不影响运行，也不影响验收**。在 7.2 做比对时，属于「可以不同」的范畴。

> `<li>` 之间的空行不是必须的（原项目里有空行），加不加都不影响功能。你也可以照着原项目的风格留空行，保持可读性。

---

<a id="sec-phase-2"></a>

### Phase 2 手写 CSS 完成页面视觉

**本阶段目标**：用 CSS 把裸结构变成和原项目一样的外观。

**本阶段原则**：**从上往下**按「背景 → 容器 → 内容 → 按钮」的顺序写。每写一小段就存盘看一次效果，这样一旦出问题，你能立刻定位是哪一段的 CSS 写错了。

现在 `style.css` 里还有 Step 0.3 写的 `h1 { color: purple }`，**请先把那一整段删掉**，然后从下面的 Step 2.1 开始。

#### Step 2.1｜引入字体并设置全局盒模型

**操作**：在 `style.css` 最顶部写入：

```css
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@200;400&display=swap');

* {
  box-sizing: border-box;
}
```

**解释**

- `@import` 引入 Google Fonts 的 **Poppins** 字体。注意 `@import` 必须写在 CSS 文件的**最开头**，写在任何规则之后会失效；
- `*` 是**通配选择器**，匹配页面上每一个元素；
- `box-sizing: border-box` 让所有元素进入「边框盒」模式。必须放在最前面，这样后面所有 `width` 才是你以为的那个宽度。

**测试步骤**

1. 保存，浏览器刷新；
2. 按 `F12` → **Network** 标签 → 刷新页面；
3. 在请求列表里找 `fonts.googleapis.com`。

**预期**

- **在线环境**：能看到该请求返回 `200` 或 `304`，页面字体变成 Poppins（`1` 的笔画更细，字母形状略有不同）；
- **离线/内网环境**：该请求显示失败 `(failed)` 或超时。**这是正常的**，页面会自动降级成系统默认字体，不影响任何功能。继续往下做。

> **别在这里卡住。** 字体只是锦上添花，判定通过的标准是「页面没崩、Console 无红色报错」。

#### Step 2.2｜body 渐变背景与居中

**操作**：紧接在 `*` 规则之后写入：

```css
body {
  background-color: #b8c6db;
  background-image: linear-gradient(315deg, #b8c6db 0%, #f5f7fa 100%);
  font-family: 'Poppins', sans-serif;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
  overflow: hidden;
  margin: 0;
}
```

**逐行解释**

| 行 | 作用 | 为什么需要 |
| --- | --- | --- |
| `background-color` | 纯色兜底 | 渐变加载失败时仍有底色 |
| `background-image` | 斜向渐变 | 主视觉 |
| `font-family` | 页面默认字体 | 'Poppins' 不可用时退回 `sans-serif` |
| `display: flex` | 开启弹性布局 | 居中的前提 |
| `align-items` | 垂直居中 | |
| `justify-content` | 水平居中 | |
| `height: 100vh` | 撑满一屏 | **没有它，居中会失效** |
| `overflow: hidden` | 裁掉溢出 | 配合卡片的圆角 |
| `margin: 0` | 清掉 body 默认外边距 | 否则四周有 8px 白边 |

**测试步骤**

1. 保存并刷新；
2. 看整个页面背景；
3. 看卡片的位置。

**预期**

- 背景是**从右上到左下**的浅蓝→白渐变；
- 白色卡片已经**移动到屏幕正中央**（尽管它还没样式）。

**易错点**

> **卡片没居中，挤在左上角？** 检查 `height: 100vh` 是否写了，以及 `display: flex` 是否写在了 `body` 上而不是别的选择器。
>
> **渐变方向反了？** 角度 `315deg` 的含义是「渐变线朝向 315° 方向」。改成 `45deg` 试试，对比一下效果，记住这个规律即可。

#### Step 2.3｜白色卡片

**操作**：继续往下写：

```css
.quiz-container {
  background-color: #fff;
  border-radius: 10px;
  box-shadow: 0 0 10px 2px rgba(100, 100, 100, 0.1);
  width: 600px;
  overflow: hidden;
}
```

**逐行解释**

| 行 | 作用 |
| --- | --- |
| `background-color: #fff` | 卡片主体白色 |
| `border-radius: 10px` | 四角圆角 10px |
| `box-shadow` | `水平偏移 垂直偏移 模糊 扩散 颜色`，这里是无偏移、模糊 10px、扩散 2px、10% 不透明的灰 |
| `width: 600px` | 卡片宽 600px（因为有 `box-sizing: border-box`，这是总宽） |
| `overflow: hidden` | 裁掉子元素溢出，让子元素也受圆角约束 |

**测试步骤**

1. 保存并刷新；
2. 观察卡片。

**预期**

- 中间出现一块**白色圆角矩形**，边缘有**很淡的灰色阴影**；
- 卡片宽度约为 600px，在大屏上居中，两侧留白。

**易错点**

> **卡片比预期宽了一圈？** `box-sizing: border-box` 没生效。检查 `*` 那条规则是不是写在 `@import` **之后**（必须紧跟其后，但不能有其它规则插在中间）。
>
> **没有任何阴影？** 检查 `box-shadow` 的值是否 5 段齐全，颜色必须是带透明度的 `rgba(...)` 或 8 位十六进制。

#### Step 2.4｜题头区域的内边距

**操作**：继续写：

```css
.quiz-header {
  padding: 4rem;
}
```

`4rem` = 64px，四边内边距。卡片里内容离边缘的距离就是靠它撑出来的。

**测试步骤**

1. 记下当前 `Question text` 距卡片左边缘的距离；
2. 保存并刷新，再看一次。

**预期**：文字往卡片内部缩进了一截，与卡片边缘之间出现了明显的空隙。

#### Step 2.5｜题干样式

**操作**：继续写：

```css
h2 {
  padding: 1rem;
  text-align: center;
  margin: 0;
}
```

**逐行解释**

| 行 | 作用 |
| --- | --- |
| `padding: 1rem` | 题干自己的内边距（让文字不贴边） |
| `text-align: center` | **居中显示** |
| `margin: 0` | 清掉 `h2` 默认的上下外边距，让排版紧凑 |

**测试步骤**

1. 保存并刷新。

**预期**：`Question text` **水平居中**地显示在卡片内，四个选项所在的列表紧随其下。

#### Step 2.6｜选项列表样式

**操作**：继续写：

```css
ul {
  list-style-type: none;
  padding: 0;
}

ul li {
  font-size: 1.2rem;
  margin: 1rem 0;
}

ul li label {
  cursor: pointer;
}
```

**逐行解释**

| 规则 | 作用 |
| --- | --- |
| `list-style-type: none` | 去掉项目符号（圆点） |
| `padding: 0` | 去掉浏览器给列表的默认左缩进 |
| `font-size: 1.2rem` | 选项字号放大到 19.2px |
| `margin: 1rem 0` | 每个选项**上下**留 16px 间距，**左右**为 0（CSS 的 `margin` 简写顺序是：上 右 下 左） |
| `cursor: pointer` | 鼠标移到文字上变手型 |

**测试步骤**

1. 保存并刷新；
2. 看四个选项。

**预期**

- 左侧的小圆点（项目符号）**消失**；
- 整个列表不再有左侧缩进，选项文字和卡片左边缘对齐（比题干靠内一点，因为题干有 `text-align: center`）；
- 四个选项之间有均匀的垂直间距；
- 鼠标移到选项文字上，**指针变成手型**。

#### Step 2.7｜提交按钮样式

**操作**：继续写：

```css
button {
  background-color: #8e44ad;
  color: #fff;
  border: none;
  display: block;
  width: 100%;
  cursor: pointer;
  font-size: 1.1rem;
  font-family: inherit;
  padding: 1.3rem;
}

button:hover {
  background-color: #732d91;
}

button:focus {
  outline: none;
  background-color: #5e3370;
}
```

**逐行解释**

| 行 | 作用 | 为什么需要 |
| --- | --- | --- |
| `background-color: #8e44ad` | 紫色底 | 主视觉 |
| `color: #fff` | 白色文字 | 保证可读 |
| `border: none` | 去掉浏览器默认边框 | 默认灰色边框很丑 |
| `display: block` | 让按钮独占一行 | 默认 `inline-block` 不会自动撑满 |
| `width: 100%` | 通栏 | 配合上一行 |
| `cursor: pointer` | 手型光标 | 可点击提示 |
| `font-size: 1.1rem` | 按钮字号 | |
| `font-family: inherit` | **继承页面字体** | 关键：否则按钮会用系统默认字体，和正文不一致 |
| `padding: 1.3rem` | 上下内边距 | 给按钮足够的点击高度 |
| `:hover` | **悬停时**变深紫色 | 交互反馈 |
| `:focus` | **聚焦时**变更深、去掉默认焦点框 | 键盘 Tab 导航时可见 |

**测试步骤**

1. 保存并刷新；
2. 把鼠标**移到**按钮上停住。

   **预期**：按钮变成**更深的紫色**（`#732d91`）。

3. 把鼠标**移开**。

   **预期**：恢复原来的紫色（`#8e44ad`）。

4. 先点一下按钮（让页面加载后焦点在按钮上）再移开鼠标。

   **预期**：按钮保持**最深的紫色**（`#5e3370`），这是 `:focus` 状态。（如果点完按钮焦点不在它身上，按 `Tab` 键让键盘焦点落到按钮上。）

5. 观察按钮宽度。

   **预期**：按钮**横贯卡片整个底部**，左右边不留白，且和上方选项列表之间有一致的呼吸感。

#### 本阶段完成态

`style.css` 完整内容应该是：

```css
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@200;400&display=swap');

* {
  box-sizing: border-box;
}

body {
  background-color: #b8c6db;
  background-image: linear-gradient(315deg, #b8c6db 0%, #f5f7fa 100%);
  font-family: 'Poppins', sans-serif;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
  overflow: hidden;
  margin: 0;
}

.quiz-container {
  background-color: #fff;
  border-radius: 10px;
  box-shadow: 0 0 10px 2px rgba(100, 100, 100, 0.1);
  width: 600px;
  overflow: hidden;
}

.quiz-header {
  padding: 4rem;
}

h2 {
  padding: 1rem;
  text-align: center;
  margin: 0;
}

ul {
  list-style-type: none;
  padding: 0;
}

ul li {
  font-size: 1.2rem;
  margin: 1rem 0;
}

ul li label {
  cursor: pointer;
}

button {
  background-color: #8e44ad;
  color: #fff;
  border: none;
  display: block;
  width: 100%;
  cursor: pointer;
  font-size: 1.1rem;
  font-family: inherit;
  padding: 1.3rem;
}

button:hover {
  background-color: #732d91;
}

button:focus {
  outline: none;
  background-color: #5e3370;
}
```

**Phase 2 通过标志**

拿它和 `quiz-app/style.css` 打开对照，**视觉上应该几乎一致**。然后做一次**交互验证**：

1. 点击 `Java` 文字 → 圆圈选中；
2. 鼠标移到按钮 → 变深；
3. 移开 → 恢复；
4. 刷新页面 → 一切回到初始（选中被清空，这是浏览器默认行为）。

#### Git 版本节点：`v0.3-css`

```bash
git status
git add .
git commit -m "style: 手写 CSS 完成页面视觉"
git tag -a v0.3-css -m "渐变背景+白色卡片+紫色按钮，视觉对齐原项目"
git log --oneline --decorate -1
```

| 项 | 值 |
| --- | --- |
| 提交信息 | `style: 手写 CSS 完成页面视觉` |
| **标签** | **`v0.3-css`** |
| 标签说明 | 渐变背景+白色卡片+紫色按钮，视觉对齐原项目 |
| 本 Phase 预期改动 | **只有 `style.css`** |

**测试步骤**

1. `git status` → **预期**：只有 `style.css` 出现在待提交列表里。
2. `git tag -n1` → **预期**：`v0.3-css  渐变背景+白色卡片+紫色按钮，视觉对齐原项目`
3. **与原项目做一次精确比对**（这是 Phase 2 独有的红利——CSS 可以逐行比）：

   ```bash
   git show v0.3-css:style.css > /tmp/mine.css
   git diff --no-index -w /tmp/mine.css quiz-app/style.css
   ```

   **预期**：**没有任何输出**，说明你的 `style.css` 与原项目完全一致（`-w` 忽略空白差异）。

   > 有输出也没关系，先看是「实质差异」还是「只有空格/换行」。若有实质差异，把差异行对照 [Step 2.2–2.7](#sec-phase-2) 逐项检查。
   >
   > Windows PowerShell 用户改用：`git diff --no-index -w lab/quiz-app/style.css quiz-app/style.css`（在你有这两个目录的上级目录执行）。

4. `git log --oneline --decorate` → **预期**：三条记录依次挂着 `v0.1-init`、`v0.2-html`、`v0.3-css`。

> **到这里你会体会到 Git 的价值**：`v0.2-html` 这个标签现在是一个「随时可以回去」的存档。如果你 CSS 写到一半改崩了想推倒重来，`git checkout v0.2-html` 就能回到「HTML 完整、CSS 是空」的状态重新写，**HTML 一个字都不会丢**。

---

<a id="sec-phase-3"></a>

### Phase 3 用 JavaScript 准备题库数据

**本阶段目标**：创建 4 道题的题库数组。

**本阶段完成标志**：在 Console 里能打印出完整的 4 道题。

#### Step 3.1｜清空并写入第一题

**操作**：`script.js` 里还留着 Phase 0 的 `console.log`，**整行删掉**，然后写入：

```js
const quizData = [
  {
    question: "Which language runs in a web browser?",
    a: "Java",
    b: "C",
    c: "Python",
    d: "JavaScript",
    correct: "d",
  },
]
```

**数据结构的含义**

| 字段 | 类型 | 作用 |
| --- | --- | --- |
| `question` | 字符串 | 题干文字 |
| `a` / `b` / `c` / `d` | 字符串 | 四个选项的文字 |
| `correct` | 字符串 | 正确答案，存的是**选项的 id**（`a`/`b`/`c`/`d`），不是文字 |

整个 `quizData` 是一个数组（`[...]`），里面放了 4 个对象（`{...}`），每个对象代表一道题。

**测试步骤**

1. 保存并刷新；
2. 切到 DevTools 的 **Console** 标签，输入：

   ```js
   quizData.length
   ```

3. 回车。

**预期**：输出 `1`。

#### Step 3.2｜补齐剩余三道题

**操作**：把第 1 道题的对象后面加逗号，然后补上另外 3 个对象：

```js
const quizData = [
  {
    question: "Which language runs in a web browser?",
    a: "Java",
    b: "C",
    c: "Python",
    d: "JavaScript",
    correct: "d",
  },
  {
    question: "What does CSS stand for?",
    a: "Central Style Sheets",
    b: "Cascading Style Sheets",
    c: "Cascading Simple Sheets",
    d: "Cars SUVs Sailboats",
    correct: "b",
  },
  {
    question: "What does HTML stand for?",
    a: "Hypertext Markup Language",
    b: "Hypertext Markdown Language",
    c: "Hyperloop Machine Language",
    d: "Helicopters Terminals Motorboats Lamborginis",
    correct: "a",
  },
  {
    question: "What year was JavaScript launched?",
    a: "1996",
    b: "1995",
    c: "1994",
    d: "none of the above",
    correct: "b",
  },
]
```

**抄写检查清单**（每题都是 6 个字段：`question`、`a`、`b`、`c`、`d`、`correct`）

| # | 题干 | 正确答案 |
| --- | --- | --- |
| 1 | Which language runs in a web browser? | `d` |
| 2 | What does CSS stand for? | `b` |
| 3 | What does HTML stand for? | `a` |
| 4 | What year was JavaScript launched? | `b` |

**测试步骤**

1. 保存并刷新；
2. 在 Console 依次输入：

   ```js
   quizData.length        // 预期 4
   quizData[0].question   // 预期 "Which language runs in a web browser?"
   quizData[3].correct    // 预期 "b"
   quizData[1].d          // 预期 "Cars SUVs Sailboats"
   ```

3. 把鼠标悬停在 Console 输出的 `quizData` 上（或输入 `quizData` 回车），展开查看结构。

**预期**：全部符合。

**易错点**

| 报错信息 | 原因 | 解决 |
| --- | --- | --- |
| `Uncaught SyntaxError: Unexpected token ']'` | 对象之间漏了逗号，或某个对象结尾多了逗号 | 检查每个 `},` 后的逗号 |
| `quizData is not defined` | 数组最后一条后面**多了**逗号（JS 允许对象数组尾逗号，但不允许对象内字段尾逗号）；或 `const` 写在了别的花括号里 | 检查 `const quizData` 是不是在最外层 |
| `quizData.length` 输出 `0` | 写成了 `[]` | 确认里面真的有 4 个 `{}` |

> **页面上什么都看不到变化？** 这是**正常的**。这个数组只是数据，还没有代码去把它显示到页面上。Phase 4 才会接线。

#### Git 版本节点：`v0.4-data`

```bash
git status
git add .
git commit -m "feat: 添加 quizData 题库数据（4 道题）"
git tag -a v0.4-data -m "quizData 4 道题，Console 可打印"
git log --oneline --decorate -1
```

| 项 | 值 |
| --- | --- |
| 提交信息 | `feat: 添加 quizData 题库数据（4 道题）` |
| **标签** | **`v0.4-data`** |
| 标签说明 | quizData 4 道题，Console 可打印 |
| 本 Phase 预期改动 | **只有 `script.js`** |

**测试步骤**

1. `git status` → **预期**：只有 `script.js` 出现在待提交列表里。
2. `git tag -n1` → **预期**：`v0.4-data  quizData 4 道题，Console 可打印`
3. `git show v0.4-data:script.js` → **预期**：Git 把它打印出来，你可以**逐字核对题库内容**（4 道题、答案依次是 `d`、`b`、`a`、`b`）。

> **第 3 步是个查题库的利器。** 肉眼在编辑器里扫 20 行题库容易看漏，用 `git show` 打出来一行行看，错别字（比如 `Cascading Style Sheets` 少个词）会显眼得多。

---

<a id="sec-phase-4"></a>

### Phase 4 获取 DOM 元素并渲染题目

**本阶段目标**：让页面上的题干和选项文字，变成题库里的内容。

**本阶段完成标志**：打开页面就直接显示第 1 题（而不是 `Question text` 和 `Java/C/Python/JavaScript`）。

#### Step 4.1｜获取页面上要操作的元素

**操作**：接在 `quizData` 下面（数组结尾的 `]` 之后）写入：

```js
const quiz = document.getElementById('quiz')
const answerEls = document.querySelectorAll('.answer')
const questionEl = document.getElementById('question')
const a_text = document.getElementById('a_text')
const b_text = document.getElementById('b_text')
const c_text = document.getElementById('c_text')
const d_text = document.getElementById('d_text')
const submitBtn = document.getElementById('submit')
```

**逐行解释**

| 变量 | 拿到的是什么 | 后面用来干什么 |
| --- | --- | --- |
| `quiz` | 整张卡片 `<div id="quiz">` | 结算时把里面内容**整体换掉** |
| `answerEls` | **4 个** radio 组成的集合 | 批量清空选中 / 批量读取选中项 |
| `questionEl` | 题干 `<h2 id="question">` | 改题干文字 |
| `a_text` … `d_text` | 4 个 `<label>` | 改 4 个选项的文字 |
| `submitBtn` | `<button id="submit">` | 挂点击事件 |

**两个函数的参数写法区别（再看一遍，很重要）**

```js
document.getElementById('quiz')         // 传 id 本身，不带 #，返回单个元素
document.querySelectorAll('.answer')    // 传完整 CSS 选择器，带 .，返回一组元素
```

如果写成 `getElementById('.quiz')`，它会返回 `null`，后面用到它的代码就会报错。

**测试步骤**

1. 保存并刷新；
2. 在 Console 输入：

   ```js
   quiz            // 预期显示 <div class="quiz-container" id="quiz">…</div>
   questionEl      // 预期显示 <h2 id="question">Question text</h2>
   answerEls.length // 预期 4
   ```

**预期**：全部符合。

#### Step 4.2｜声明状态变量

**操作**：接在上面这组 `const` 之后写入：

```js
let currentQuiz = 0
let score = 0
```

**含义**

| 变量 | 初值 | 含义 |
| --- | --- | --- |
| `currentQuiz` | `0` | 当前是第几题（从 0 开始，因为数组也是从 0 开始数的） |
| `score` | `0` | 答对了几题 |

用 `let` 而不是 `const`，因为它们**每答一题都要变**。

**测试步骤**

1. 保存并刷新页面；
2. 切到 Console，输入：

   ```js
   currentQuiz
   score
   ```

**预期**：分别输出 `0` 和 `0`。页面外观**没有任何变化**（此时还没有任何代码去改变它们）。

3. 在 Console 输入 `currentQuiz = 3` 然后回车，再输入 `currentQuiz`。

**预期**：输出 `3`。这说明变量可以重新赋值——如果你当初用的是 `const`，这一步会报错 `Assignment to constant variable`。

4. 刷新页面，重新输入 `currentQuiz`。

**预期**：又变回 `0`。**每次刷新，脚本都会从头执行一遍**，`let currentQuiz = 0` 重新执行。这是后面「Reload 能重开一轮」的原理。

#### Step 4.3｜写清空选中函数和渲染函数

**操作**：继续往下写：

```js
function deselectAnswers() {
  answerEls.forEach(answerEl => (answerEl.checked = false))
}

function loadQuiz() {
  deselectAnswers()

  const currentQuizData = quizData[currentQuiz]

  questionEl.innerText = currentQuizData.question
  a_text.innerText = currentQuizData.a
  b_text.innerText = currentQuizData.b
  c_text.innerText = currentQuizData.c
  d_text.innerText = currentQuizData.d
}
```

**逐段解释**

`deselectAnswers()` —— 把 4 个 radio 全部取消选中：

```js
answerEls.forEach(answerEl => (answerEl.checked = false))
```

- `answerEls.forEach(...)`：遍历 4 个 radio；
- `answerEl` 是每次循环里「当前这个 radio」的临时名字；
- `.checked = false`：取消选中。`checked` 是单选框的**选中状态属性**（`true` 选中 / `false` 未选中）。

`loadQuiz()` —— 把当前题渲染到页面上：

| 代码 | 作用 |
| --- | --- |
| `deselectAnswers()` | **先清空**上一题残留的选中状态 |
| `quizData[currentQuiz]` | 取出当前题的数据对象 |
| `questionEl.innerText = ...` | 用题目数据替换题干文字 |
| `a_text.innerText = ...` | 用 `a` 字段替换第 1 个选项文字 |

> **顺序很重要**：必须先 `deselectAnswers()` 再写文字。如果反了……其实这个例子反了也看不出问题，但**先重置状态、再渲染内容**是好习惯，本项目后面换题时能救命。

**测试步骤**

写完函数但**先不要**写 Step 4.4 的调用，改用 Console 手动调用来验证：

1. 保存并刷新页面，打开 Console；
2. 输入 `loadQuiz()` 然后回车。

   **预期**：题干和 4 个选项**没有任何变化**（还是 HTML 里写死的 `Question text` / `Java` / `C` / `Python` / `JavaScript`）。因为此时脚本里还没有任何地方自动调用它。

3. 在 Console 输入 `deselectAnswers()` 然后回车。

   **预期**：**无报错**。如果 4 个 radio 里恰好有选中的，它会被取消。

4. 先点选 `Python`，再输入 `deselectAnswers()` 回车。

   **预期**：`Python` 的圆圈**被取消选中** ← 这是 `deselectAnswers()` 真正生效的证据。

5. 在 Console 输入 `getElementById('question')` 并回车，展开返回的元素。

   **预期**：能看到 `<h2 id="question">Question text</h2>`，证明脚本能拿到这个元素、可以修改它。

**易错点**

| 现象 | 原因 |
| --- | --- |
| Console 报 `loadQuiz is not defined` | 函数名拼写不一致（大小写） |
| Console 报 `Cannot read properties of undefined` | `currentQuiz` 当前值超出数组范围（`>= 4`），`quizData[currentQuiz]` 取不到 |
| 点选项没反应 | 正常！本Step 只是写函数，还没有挂任何事件 |

#### Step 4.4｜触发第一次渲染

**操作**：在**两个函数定义之后**（也就是 `loadQuiz` 函数的结尾大括号下面），写入：

```js
loadQuiz()
```

现在 `script.js` 的整体结构是：

```js
const quizData = [ /* 4 道题 */ ]

// 获取 DOM 元素
const quiz = document.getElementById('quiz')
const answerEls = document.querySelectorAll('.answer')
// ... 其余 const

// 状态变量
let currentQuiz = 0
let score = 0

// 函数定义
function deselectAnswers() { ... }
function loadQuiz() { ... }

// 启动：渲染第一题
loadQuiz()
```

**关于「为什么调用写在函数定义后面」**

JavaScript 会把**函数声明自动提到文件最前面**（叫「函数提升」），所以理论上 `loadQuiz()` 写在函数定义**之前**也能跑。

**但请一律写在后面。** 初学阶段遵循「先定义、后调用」的顺序，代码读起来符合直觉，也避免以后把函数换成箭头函数（`const loadQuiz = () => {}`）时突然报错——**箭头函数不会被提升**。

**测试步骤**

1. 保存并刷新页面；
2. 观察页面。

**预期**

- 题干变成 `Which language runs in a web browser?`；
- 四个选项变成 `Java` / `C` / `Python` / `JavaScript`；
- 四个 radio **全部未选中**。

3. 点击 `JavaScript` 选中，然后按 `F5` 刷新。

**预期**：刷新后回到未选中状态（因为 `loadQuiz()` 每次都调用 `deselectAnswers()`）。

4. 在 Console 输入 `currentQuiz` 和 `score`。

**预期**：分别输出 `0` 和 `0`。

**易错点**

| 现象 | 原因 | 解决 |
| --- | --- | --- |
| 页面标题还是 `Question text` | `loadQuiz()` 没调用，或函数名拼错 | 检查最后那行 |
| Console 报 `Cannot set properties of null` | 某个 `getElementById` 返回了 `null` | 逐个在 Console 里 `console.log(第N行那个变量)`，找出是 `null` 的那个，对照 HTML 检查 id 拼写 |
| Console 报 `currentQuizData is undefined` | `currentQuiz` 越界（比如被手改成 4） | 改回 `0` |
| 报 `loadQuiz is not defined` | 函数名拼写不一致 | 检查大小写 |

#### Git 版本节点：`v0.5-render`

```bash
git status
git add .
git commit -m "feat: 获取 DOM 元素并用 loadQuiz 渲染题目"
git tag -a v0.5-render -m "8 个 DOM 引用+loadQuiz，页面加载即显示第 1 题"
git log --oneline --decorate -1
```

| 项 | 值 |
| --- | --- |
| 提交信息 | `feat: 获取 DOM 元素并用 loadQuiz 渲染题目` |
| **标签** | **`v0.5-render`** |
| 标签说明 | 8 个 DOM 引用+loadQuiz，页面加载即显示第 1 题 |
| 本 Phase 预期改动 | **只有 `script.js`** |

**测试步骤**

1. `git status` → **预期**：只有 `script.js`。
2. `git tag -n1` → **预期**：`v0.5-render  8 个 DOM 引用+loadQuiz，页面加载即显示第 1 题`
3. `git diff v0.4-data v0.5-render --stat` → **预期**：显示 `script.js | N +++++` 一行，**只有这一个文件**。这叫「打两个标签之间的差异」：

   ```bash
   git diff v0.4-data v0.5-render -- script.js
   ```

   **预期**：输出里能清楚看到你新加的 8 行 DOM 引用、2 行状态变量、2 个函数和 1 行 `loadQuiz()` 调用。

> **为什么这个 Phase 值得单独打标签？** 因为它后面还有 Phase 5、6 要继续改 `script.js`。一旦改崩了，`git restore --source v0.5-render script.js` 能把 `script.js` **精确恢复**到「刚学会渲染、还没开始判题」的状态——这是你最容易出错的起点，务必保住。

---

<a id="sec-phase-5"></a>

### Phase 5 实现判题与计分

**本阶段目标**：点击 Submit 后，正确计分并进入下一题。

**本阶段完成标志**：能连续答完 4 题，分数正确累加（但还没有结算页）。

#### Step 5.1｜写读取选中项的函数

**操作**：接在 `loadQuiz` 函数之后写入：

```js
function getSelected() {
  let answer

  answerEls.forEach(answerEl => {
    if (answerEl.checked) {
      answer = answerEl.id
    }
  })

  return answer
}
```

**逐段解释**

```js
let answer                    // 先声明，但不给值 → 此时是 undefined
```

```js
answerEls.forEach(answerEl => {
  if (answerEl.checked) {    // 挨个检查：哪个被选中了？
    answer = answerEl.id     // 把它的 id 存下来（'a'/'b'/'c'/'d'）
  }
})
```

```js
return answer                 // 把找到的 id 返回出去
```

**为什么要用 `if` 而不是直接赋值？** 因为 4 个 radio 里只有 1 个会被选中，其余 3 个 `checked` 是 `false`。`if` 保证只有**被选中的那个**才会写入 `answer`。

**万一一个都没选呢？** 循环跑完，`answer` 从头到尾没被赋值，函数返回 `undefined`。这个「空值」正是 Step 5.2 防呆逻辑的依据。

**测试步骤**

1. 保存并刷新；
2. 在 Console 输入：

   ```js
   getSelected()
   ```

**预期**：输出 `undefined`（因为当前页面加载后没有选中任何项）。

3. 点击 `C` 这个选项，再输入：

   ```js
   getSelected()
   ```

**预期**：输出 `'b'` —— 因为第 2 个 radio 的 `id="b"`。

4. 点击 `Java`（换成第 1 个），再执行 `getSelected()`。

**预期**：输出 `'a'` —— 且之前选中的第 2 个自动取消选中。

#### Step 5.2｜挂上点击事件并做防呆判断

**操作**：接在 `getSelected` 函数之后写入：

```js
submitBtn.addEventListener('click', () => {
  const answer = getSelected()

  if (answer) {
    // TODO: 判题逻辑写在下一小步
  }
})
```

**解释**

- `submitBtn.addEventListener('click', ...)`：给按钮挂一个点击监听器。用户每点一次，大括号里的代码就执行一次；
- `const answer = getSelected()`：先拿到用户选了什么；
- `if (answer)`：**防呆核心**。`answer` 是 `undefined`（或空字符串）时，这个条件为假，整块逻辑被跳过——用户没选就点提交，页面不会有任何反应，也不会跳题。

**测试步骤**

1. **先确保当前页面（刷新后）没有任何选项被选中**；
2. 点击 `Submit`。

**预期**：**页面毫无变化**——不跳题、不加分、不报错。

3. 点选 `JavaScript`，再点 `Submit`。

**预期**：**仍然毫无变化**（因为 `if (answer)` 里现在是空的 TODO）。

4. 刷新页面重来。

**易错点**

> **一点 Submit 页面就变白了？** 说明 `addEventListener` 的大括号没闭合对，或者误把 `loadQuiz()` 的调用写进了监听器里。检查括号配对。

#### Step 5.3｜实现判题与切题

**操作**：把 Step 5.2 里那个 TODO 注释替换成真正的逻辑：

```js
submitBtn.addEventListener('click', () => {
  const answer = getSelected()

  if (answer) {
    if (answer === quizData[currentQuiz].correct) {
      score++
    }

    currentQuiz++

    if (currentQuiz < quizData.length) {
      loadQuiz()
    }
  }
})
```

**逐段解释**

```js
if (answer === quizData[currentQuiz].correct) {
  score++
}
```

- 左边是用户选的 id，右边是题库里写的正确答案；
- 相等 → `score` 加 1；不相等 → **什么都不做**（不扣分，只是不加分）。

```js
currentQuiz++
```

题号 +1，准备下一题。

```js
if (currentQuiz < quizData.length) {
  loadQuiz()
}
```

**边界判断**：如果新题号还小于总题数（4），就渲染新题；否则**什么都不做**。

> 目前最后一种情况「什么都不做」是个小 bug —— 答完第 4 题后点提交，按钮毫无反应。**这是刻意留下的**，Phase 6 会补上结算页。这样拆分是为了让你能单独验证「判题 + 切题」这一层逻辑。

**测试步骤（本阶段核心测试）**

先刷新页面，然后按顺序做下面 5 组，每组之间记得刷新。

**测试用例 1｜全对**

1. 选 `JavaScript`（第 1 题正确）→ 点 Submit
2. 选 `Cascading Style Sheets`（第 2 题正确）→ 点 Submit
3. 选 `Hypertext Markup Language`（第 3 题正确）→ 点 Submit
4. 选 `1995`（第 4 题正确）→ 点 Submit

**预期**：题干和选项依次换成第 2、3、4 题的内容；每题加载后**上一个选项都自动取消选中**；答完第 4 题点 Submit，页面无变化（结算页还没做）。

在 Console 输入 `score` → **预期输出 `4`**。
在 Console 输入 `currentQuiz` → **预期输出 `4`**。

**测试用例 2｜全错**

1. 刷新；
2. 依次选 `Java`（错）、`Central Style Sheets`（错）、`Hypertext Markdown Language`（错）、`1996`（错），每次都点 Submit。

**预期**：题目正常往下走，但 Console 里 `score` → **预期输出 `0`**，`currentQuiz` → `4`。

**测试用例 3｜不选就提交（防呆）**

1. 刷新；
2. 直接点 Submit，连点 5 次。

**预期**：题干始终是第 1 题；Console 里 `currentQuiz` → **0**，`score` → **0**。

**测试用例 4｜对错混合**

1. 刷新；
2. 选 `Java`（错）→ Submit；选 `Cascading Style Sheets`（对）→ Submit；选 `1996`（错）→ Submit；选 `none of the above`（错）→ Submit。

**预期**：`score` → **1**，`currentQuiz` → **4**。

**测试用例 5｜每题只能提交一次**

1. 刷新；
2. 选对第 1 题 → Submit；
3. **不换选项**，直接连点 Submit。

**预期**：页面**停在第 2 题不动**，`currentQuiz` → **1**。

> 这条能验证一个关键点：点 Submit 后 `loadQuiz()` 会调 `deselectAnswers()`，所以你上一次的选择被清空了。不清空的话就会「连点跳过好几题」。

#### Git 版本节点：`v0.6-scoring`

```bash
git status
git add .
git commit -m "feat: 实现判题与计分逻辑"
git tag -a v0.6-scoring -m "getSelected+判题+切题，能答完 4 题（结算页待补）"
git log --oneline --decorate -1
```

| 项 | 值 |
| --- | --- |
| 提交信息 | `feat: 实现判题与计分逻辑` |
| **标签** | **`v0.6-scoring`** |
| 标签说明 | getSelected+判题+切题，能答完 4 题（结算页待补） |
| 本 Phase 预期改动 | **只有 `script.js`** |

**测试步骤**

1. `git status` → **预期**：只有 `script.js`。
2. `git tag -n1` → **预期**：`v0.6-scoring  getSelected+判题+切题，能答完 4 题（结算页待补）`
3. `git diff v0.5-render v0.6-scoring --stat` → **预期**：仍然只有 `script.js` 一个文件，净增若干行。

> **标签说明里的「（结算页待补）」是有意写上的。** `v0.6-scoring` 这个版本**功能上是残缺的**——答完第 4 题点提交没反应。它是一个「已知未完成」的存档，不是 bug。你要用它做对照时，记得跳过这个已知问题。

**如果 Phase 5 写崩了怎么救**

```bash
git restore --source v0.5-render script.js
```

这会把 `script.js` 恢复到「只完成渲染」的状态，**Phase 4 的成果完好**，你可以从 Step 5.1 重新开始。

---

<a id="sec-phase-6"></a>

### Phase 6 结算页与重新开始

**本阶段目标**：答完 4 题后，显示得分，并提供重新开始的按钮。

**本阶段完成标志**：最后一题提交后，卡片变成「You answered X/4 questions correctly」+ 一个 Reload 按钮。

#### Step 6.1｜补上结算分支

**操作**：回到 Step 5.3 的代码，把最后的 `if (currentQuiz < quizData.length)` 改成 `if...else` 结构：

```js
submitBtn.addEventListener('click', () => {
  const answer = getSelected()

  if (answer) {
    if (answer === quizData[currentQuiz].correct) {
      score++
    }

    currentQuiz++

    if (currentQuiz < quizData.length) {
      loadQuiz()
    } else {
      quiz.innerHTML = `
        <h2>You answered ${score}/${quizData.length} questions correctly</h2>

        <button onclick="location.reload()">Reload</button>
      `
    }
  }
})
```

**逐段解释**

```js
quiz.innerHTML = `...`
```

- `quiz` 是 Step 4.1 拿到的**整张卡片**；
- `innerHTML =` 一执行，**卡片里原有的所有内容被彻底删除**，换成新的 HTML。

  这就是为什么结算页上原来的 4 个选项、紫色 Submit 按钮全都不见了 —— 它们被**替换**了，不是隐藏。

```js
<h2>You answered ${score}/${quizData.length} questions correctly</h2>
```

- `${score}` 和 `${quizData.length}` 是**模板字符串**的插值，会被实际数字替换；
- 例如 `score` 是 3、`quizData.length` 是 4，渲染出来就是 `You answered 3/4 questions correctly`。

```js
<button onclick="location.reload()">Reload</button>
```

- 写在 HTML 属性里的双引号，和外层模板字符串的反引号**不冲突**；
- `location.reload()` 是浏览器内置方法，重新加载当前页面。因为 `score` 和 `currentQuiz` 是普通变量、刷新后会被重置为 0，所以刷新即开始新一轮。

**测试步骤**

1. 保存并刷新；
2. **全对一遍**（步骤同 5.3 的测试用例 1）；
3. 最后一题点 Submit。

**预期**

- 卡片内容**整体被替换**成一行居中的 `You answered 4/4 questions correctly`；
- 下方出现一个紫色 `Reload` 按钮，样式和之前的 Submit 一样（因为 CSS 里的 `button` 选择器对所有按钮生效）；
- 页面上**不再有**任何 radio 选项，也**没有**题目。

4. 点击 `Reload`。

**预期**：页面恢复成第 1 题，4 个选项重新出现，分数归零（可以再次用 Console 的 `score` 确认）。

5. **再测一次全错**：刷新 → 4 题全选错 → 最后一题提交。

**预期**：显示 `You answered 0/4 questions correctly`。

6. **再测一次 2 对 2 错**。

**预期**：显示 `You answered 2/4 questions correctly`。

#### Step 6.2｜验证结算页的边界行为

**测试步骤**

1. 结算页出现后，**再点一次 Reload 之外的区域**（比如卡片空白处）。

   **预期**：什么都不会发生（不报错）。

2. 结算页出现后，在 Console 输入 `submitBtn`。

   **预期**：仍然指向原来的那个 button 元素——**但它已经从页面上被删掉了**。这是 JavaScript 的特性：变量还持有那个对象的引用，可页面上已经没有它了。

   > 这个细节解释了为什么结算页**不能**再点出第二次计分：`submitBtn` 上挂的事件监听器作用在一个已脱离文档的节点上，用户已经点不到它了。

3. 刷新页面，确认一切回到初始状态。

**预期**：第 1 题，无选中，变量重置为 0。

#### Git 版本节点：`v0.7-result`

```bash
git status
git add .
git commit -m "feat: 完成结算页与重新开始"
git tag -a v0.7-result -m "innerHTML 结算页+Reload 重新开始，功能完整"
git log --oneline --decorate -1
```

| 项 | 值 |
| --- | --- |
| 提交信息 | `feat: 完成结算页与重新开始` |
| **标签** | **`v0.7-result`** |
| 标签说明 | innerHTML 结算页+Reload 重新开始，**功能完整** |
| 本 Phase 预期改动 | **只有 `script.js`** |

**测试步骤**

1. `git status` → **预期**：只有 `script.js`。
2. `git tag -n1` → **预期**：`v0.7-result  innerHTML 结算页+Reload 重新开始，功能完整`
3. `git diff v0.6-scoring v0.7-result -- script.js` → **预期**：差异很小，应该只有 `else` 分支和那段模板字符串——**正好验证了 Phase 5/6 的拆分是干净的**。

> **这是你的第一个功能完整版本。** 建议现在顺手做两件事：
>
> 1. 把 `v0.7-result` 旁边的页面截图存下来；
> 2. 用 `git show v0.7-result` 把三份文件的内容完整打出来存一份。
>
> 接下来 Phase 7 的验收如果发现问题，你可以随时 `git restore --source v0.7-result .` 回到这个干净状态重验。

---

<a id="sec-phase-7"></a>

### Phase 7 联调、验收与自查

**本阶段目标**：跑完完整测试，并和原项目逐项比对。

**本阶段完成标志**：第 7 节的验收清单**全部通过**。

#### Step 7.1｜完整流程走查

**测试步骤（本阶段主测试）**

按顺序做一遍，每一步都要确认现象：

| # | 操作 | 预期 |
| --- | --- | --- |
| 1 | 打开页面 | 显示第 1 题，无选中 |
| 2 | 不选，直接点 Submit | 无任何反应 |
| 3 | 点 `Java` | 选中第 1 项 |
| 4 | 点 `C`（第 2 项）文字 | 选中第 2 项，第 1 项自动取消 |
| 5 | 点 `Python` 和 `JavaScript` | 互斥始终生效 |
| 6 | 选 `JavaScript` → Submit | 显示第 2 题，无选中 |
| 7 | 选 `C` → Submit | 显示第 3 题 |
| 8 | 选 `Hypertext Markup Language` → Submit | 显示第 4 题 |
| 9 | 选 `1996` → Submit | 显示结算页，分数为 2 |
| 10 | 点 Reload | 回到第 1 题，分数归零 |
| 11 | 鼠标悬停 Submit | 按钮变深紫 |
| 12 | 键盘 `Tab` 键 | 焦点可依次落在 4 个选项和按钮上 |
| 13 | 缩窄浏览器窗口 | 卡片保持 600px，可能出现横向滚动 |
| 14 | 打开 Console | **没有任何红色报错** |

#### Step 7.2｜Console 必须干净

**测试步骤**

1. 打开 DevTools → **Console**；
2. 点击控制台顶部的 🚫（Clear console）图标清空；
3. 按 `F5` 刷新页面；
4. 答完全部 4 题，点一次 Reload；
5. 再次查看 Console。

**预期**：全程**零红色报错**。

> 黄色警告（Warning）通常来自浏览器扩展或字体加载失败，与你的代码无关，可以忽略。**红色报错（Error）必须清零。**

#### Step 7.3｜代码自查

**测试步骤（代码自查）**

对着 `quiz-app/script.js` 逐项核对你的 `lab/quiz-app/script.js`：

- [ ] `quizData` 数组有 4 个对象，每个都有 6 个字段
- [ ] 4 道题的题干和选项文字与原项目**完全一致**
- [ ] 4 道题的 `correct` 分别是 `d`、`b`、`a`、`b`
- [ ] `quiz`、`answerEls`、`questionEl`、`a_text`…`d_text`、`submitBtn` 共 8 个元素引用**一个不少**
- [ ] `currentQuiz` 和 `score` 用的是 `let`（不是 `const`）
- [ ] `deselectAnswers()` 在 `loadQuiz()` 内部**被调用了**
- [ ] `loadQuiz()` 在脚本末尾**被调用了一次**
- [ ] `getSelected()` 里用 `if (answerEl.checked)` 判断
- [ ] 提交事件里**先 `if (answer)` 再判题**
- [ ] 判题用 `===`，不是 `==`
- [ ] 结算页用 `innerHTML`，换文字用 `innerText`

**自查方法**：把两个 `script.js` 并排放，逐段比对逻辑。**不要求缩进和空格完全相同**，只要求逻辑一致。

<a id="sec-6"></a>

## 6 常见错误排查手册

按**「你看到的现象」**查，而不是按「你觉得哪里错了」查。90% 的问题都在下表里。

> **写崩了先别急着改，先想一句：**
>
> ```bash
> git status
> ```
>
> 如果这个 Phase 还没提交，直接 `git restore .` 就能回到上一个标签的干净状态重来，**代价是丢掉这个 Phase 的进度**。如果已经提交了但打错了标签，用 `git restore --source <上一个标签> .` 回到那个状态。
>
> 什么错都修不好的时候，先**回到上一个能跑的标签**，把功能跑通，再回来一点点加——这比在一个坏状态上反复打补丁快得多。Git 相关报错查 [4.8](#sec-git-8)。

### 6.1 样式完全没生效

| 检查顺序 | 操作 | 判断标准 |
| --- | --- | --- |
| 1 | 确认改的是哪个文件 | 是不是在改仓库里的 `quiz-app/style.css`，而不是 `lab/quiz-app/style.css`？ |
| 2 | 确认 `<link>` 存在且正确 | `<link rel="stylesheet" href="style.css" />`，且 `style.css` 与 `index.html` **在同一文件夹** |
| 3 | 刷新页面 | `Ctrl + R`；Live Server 的自动刷新偶尔不触发 |
| 4 | DevTools → Network → 刷新，找 `style.css` | 状态码必须是 `200`。`404` = 路径错 |
| 5 | DevTools → Elements → 点某个元素 | 右侧 Styles 面板若该规则是**灰色带删除线**的，说明被更高优先级覆盖了 |

### 6.2 脚本完全没执行

| 现象 | 原因 | 解决 |
| --- | --- | --- |
| Console 一片安静，页面毫无反应 | `<script src="script.js">` 没写，或写错路径 | 检查 `</body>` 前一行 |
| Console 报 `Uncaught SyntaxError` | JS 语法错（多半是少逗号、括号不配对） | 看报错里的**行号列号**，跳过去数一遍括号 |
| `script.js` 里第一行就报错，后面全不执行 | 语法错误会**中断整个脚本** | 先把语法错误清零，其它错误才看得见 |

### 6.3 `Cannot set properties of null (setting 'innerText')`

**成因**：`document.getElementById('xxx')` 返回了 `null`，说明页面上没有 `id="xxx"` 的元素。

**排查步骤**

1. 复制报错里的元素 id（比如 `a_text`）；
2. 在 Console 输入：

   ```js
   document.getElementById('a_text')
   ```

3. 输出 `null` → 页面上确实没这个 id。

**常见原因**

| 原因 | 说明 |
| --- | --- |
| id 拼写不一致 | HTML 里是 `a_text`，JS 里写成 `a-text` 或 `atext` |
| 大小写不一致 | HTML 是 `id="a_text"`，JS 里写成 `getElementById('A_text')`。**id 区分大小写** |
| 参数带了 `#` | `getElementById('#quiz')` ✗ 正确是 `getElementById('quiz')` |
| 元素还没被解析 | 把 `<script>` 挪到了 `<head>` 里。挪回 `</body>` 前，或加 `defer` |

### 6.4 点选项文字选不中

| 检查 | 说明 |
| --- | --- |
| label 的 `for` 和 input 的 `id` 是否完全一致 | `for="a"` 对应 `id="a"`，一个字符都不能差 |
| 是不是漏写 `for` 属性 | 没有 `for`，浏览器不知道这个 label 对应哪个控件 |
| Elements 面板里点一下 label | 看它是否高亮了对应的 input；没高亮就是绑定失败 |

### 6.5 可以同时选中多个选项

| 原因 | 解决 |
| --- | --- |
| 4 个 radio 的 `name` 不一致 | 全部改成 `name="answer"` |
| 某个 radio 压根没写 `name` | 补上 |
| 用的是 `checkbox` 而不是 `radio` | 改成 `<input type="radio" />` |

### 6.6 点 Submit 后一次跳过多题

| 原因 | 说明 |
| --- | --- |
| `loadQuiz()` 里**漏掉** `deselectAnswers()` | 上一次的选择没被清掉，`getSelected()` 一直返回同一个值 |
| 连点按钮太快 | 正常现象。`loadQuiz()` 重新渲染后选中被清空，此时再点会被 `if (answer)` 拦住 |

### 6.7 结算页显示空白

| 原因 | 说明 |
| --- | --- |
| 模板字符串的**引号写成了单引号** | 里面要写 HTML，必须用**反引号** `` ` `` |
| 忘了闭合 `` ` `` | 整段都要在反引号内 |
| `quiz` 变量拿错了元素 | 必须是用 `document.getElementById('quiz')` 拿到的**整张卡片**，不是 `.quiz-container` 里的某个子元素 |
| `${}` 里写了非法表达式 | 例如 `${score}/${quizData.length}` 中的 `/` 是分隔符，**不是除号** |

### 6.8 页面出现横向/纵向滚动条

| 原因 | 解决 |
| --- | --- |
| 卡片固定 `width: 600px`，窗口更窄 | 缩小窗口验证时属正常；生产项目可用 `max-width: 90%` 替代 |
| `box-sizing` 没生效导致实际宽度超标 | 检查 `* { box-sizing: border-box }` |
| 多个 body 上叠加了规则 | 确认选择器没写重复 |

### 6.9 布局突然全乱

按这个顺序快速定位：

1. **F12 → Elements**：找到出问题的元素，看它在 DOM 树里的**嵌套层级**对不对（尤其 label 有没有跑到 `ul` 外面）；
2. **F12 → Styles**：看是**哪一条规则**在生效（生效的规则不会显示删除线）；
3. **F12 → Styles 右上角 `:hov` / `cls`**：把鼠标移到按钮上看是不是 `:hover` 在干扰；
4. 在 Elements 面板里**临时改样式**做实验，确认方向后回到文件里改。

### 6.10 Prettier 把代码改得面目全非

见 3.3。如果格式化后引号变成双引号、分号冒出来，说明 `.prettierrc.json` 没建或没生效（确认它和 `index.html` 在同一层）。

### 6.11 Emmet 不展开

| 原因 | 解决 |
| --- | --- |
| 光标位置不对 | 光标必须在**空白处或标签内**，不能在文字中间 |
| 没有按 `Tab` | Emmet 展开的默认快捷键是 `Tab` |
| 文件类型不对 | 在 `.txt` 里 Emmet 不生效，确认扩展名是 `.html` |
| 缩写写错 | 从简短的开始试，比如先敲 `ul` 试，再加 `>li*4` |
| 想在 `.js` 里用 HTML Emmet | 需要配 `emmet.includeLanguages`（见 3.5） |

### 6.12 字体没加载

Network 里 `fonts.googleapis.com` 请求失败 = 当前无外网。**不影响功能**，页面会用系统字体正常显示。这是唯一允许「跳过」的检查项。

---

<a id="sec-7"></a>

## 7 验收清单与进阶挑战

<a id="sec-7-1"></a>

### 7.1 验收清单

提交实验前，逐项打勾。**任何一项没通过，都说明还有 bug。**

**环境**

- [ ] 3.6 的四项环境验收全部通过
- [ ] 练习文件在 `lab/quiz-app/`，与参考用的 `quiz-app/` 相互独立

**文件结构**

- [ ] `lab/quiz-app/` 下有且仅有 `index.html`、`style.css`、`script.js`（可另加 `.prettierrc.json`）

**HTML**

- [ ] 有完整的 `<!DOCTYPE html>` 和 `lang` 属性
- [ ] `<meta charset="UTF-8">` 存在
- [ ] `<link rel="stylesheet" href="style.css">` 在 `<head>` 里
- [ ] `<script src="script.js">` 在 `</body>` 前一行
- [ ] 卡片 `<div>` 同时有 `class="quiz-container"` 和 `id="quiz"`
- [ ] 题干是 `<h2 id="question">`
- [ ] 4 个 `<li>`，每组里一个 `<input type="radio">` + 一个 `<label>`
- [ ] 4 个 radio 的 `name` 都是 `answer`，`id` 是 `a`/`b`/`c`/`d`
- [ ] 4 个 radio 都有 `class="answer"`
- [ ] 4 个 label 的 `for` 与对应 input 的 `id` 一致，`id` 是 `a_text`…`d_text`
- [ ] 有 `<button id="submit">Submit</button>`

**CSS**

- [ ] 第一行是 Google Fonts 的 `@import`
- [ ] `* { box-sizing: border-box }` 紧随其后
- [ ] `body` 有渐变背景、flex 居中、`height: 100vh`、`margin: 0`、`overflow: hidden`
- [ ] `.quiz-container` 有白底、圆角、阴影、`width: 600px`、`overflow: hidden`
- [ ] `ul` 去掉了列表符号和左内边距
- [ ] `ul li` 设置了字号和上下间距
- [ ] `ul li label` 设置了 `cursor: pointer`
- [ ] `button` 有背景色、白字、无边框、`width: 100%`、`font-family: inherit`
- [ ] `button:hover` 和 `button:focus` 状态都写了
- [ ] 视觉与参考项目一致

**JavaScript**

- [ ] `quizData` 有 4 道题，字段完整，答案依次为 `d`、`b`、`a`、`b`
- [ ] 8 个 DOM 元素引用齐全
- [ ] `currentQuiz` 和 `score` 用 `let`，初值为 0
- [ ] `loadQuiz()` 内调用了 `deselectAnswers()`
- [ ] `getSelected()` 用 `if (answerEl.checked)` 找选中项
- [ ] 点击事件里**先判空**再判题
- [ ] 判题用 `===`
- [ ] 结算页用 `innerHTML` + 模板字符串

#### Git 版本节点：`v1.0-final`

```bash
git status
git add .
git commit -m "test: 联调验收并与原项目自查比对"
git tag -a v1.0-final -m "全部验收项通过，与 quiz-app 原项目自查一致"
git log --oneline --decorate --graph --all
```

| 项 | 值 |
| --- | --- |
| 提交信息 | `test: 联调验收并与原项目自查比对` |
| **标签** | **`v1.0-final`** |
| 标签说明 | 全部验收项通过，与 quiz-app 原项目自查一致 |
| 含义 | **第一个完整可交付版本** |

**测试步骤**

1. `git status` → **预期**：`nothing to commit, working tree clean`

2. `git tag` → **预期**：9 个标签，按时间顺序列出：

   ```
   v0.0-start
   v0.1-init
   v0.2-html
   v0.3-css
   v0.4-data
   v0.5-render
   v0.6-scoring
   v0.7-result
   v1.0-final
   ```

3. `git log --oneline --decorate --graph --all` → **预期**：一条干净的线性历史，每个提交旁挂着对应标签，最新一个是 `(HEAD -> main, tag: v1.0-final)`。

4. **验收整条版本线是否可复现**（这是最有力的收尾检查）：

   ```bash
   git checkout v0.2-html
   ```

   **预期**：Console 提示进入 detached HEAD，`style.css` 和 `script.js` **变回空文件**，`index.html` 回到只有骨架的状态。

   ```bash
   git switch main
   ```

   **预期**：三个文件全部恢复成最终版。

5. `git show v1.0-final --stat` → **预期**：这次提交可能只动了 `.prettierrc.json` 或什么小东西，**甚至可能显示 `no changes`**——如果整个 Phase 7 都没改代码，那就**不要**空提交，直接把标签打到 `HEAD`：

   ```bash
   git tag -a v1.0-final -m "全部验收项通过，与 quiz-app 原项目自查一致" HEAD
   ```

   > 标签可以打在**任意提交**上，不必新建提交。如果 Phase 7 你在原项目里没改任何代码，这是正常情况。

**行为**

- [ ] 打开页面直接显示第 1 题
- [ ] 不选就点 Submit → 无反应
- [ ] 点文字能选中对应圆圈
- [ ] 4 个选项互斥
- [ ] 换题后上一个选中被清空
- [ ] 全对 → 显示 `4/4`
- [ ] 全错 → 显示 `0/4`
- [ ] 混合对错 → 显示正确分数
- [ ] 结算页有 Reload 按钮，点击后能重新开始
- [ ] 全程 Console 无红色报错

**Git 版本线**

- [ ] `lab/quiz-app/` 已是 Git 仓库（`git status` 不报 `not a git repository`）
- [ ] 有 `.gitignore`，且 `git status` 里没有系统垃圾文件
- [ ] `git tag` 输出**恰好 9 个**标签：
  `v0.0-start`、`v0.1-init`、`v0.2-html`、`v0.3-css`、`v0.4-data`、`v0.5-render`、`v0.6-scoring`、`v0.7-result`、`v1.0-final`
- [ ] `git tag -n1` 里每个标签都**带有说明文字**，且说明与 [4.5 的约定表](#sec-git-5)一致
- [ ] `git log --oneline --decorate` 的历史是**连续的**，每个 Phase 一次提交，没有把多个 Phase 塞进一次提交
- [ ] 提交信息符合 `<类型>: <说明>` 格式，没有出现 `修改`、`update`、`111` 这类无意义描述
- [ ] `git checkout v0.3-css` 能正确回到那个视觉完成的版本，`git switch main` 能回来
- [ ] `HEAD` 当前指向 `v1.0-final`（在 main 分支上，不是游离状态）

<a id="sec-7-2"></a>

### 7.2 与原项目的自查比对

**比对方法**

1. 并排打开 `lab/quiz-app/` 和 `quiz-app/` 下的三个文件；
2. **逐文件**比对：

| 文件 | 怎么看 | 判定标准 |
| --- | --- | --- |
| `index.html` | 标签、id、class、结构 | **结构和属性必须一致**；缩进和空行不一致没关系 |
| `style.css` | 选择器和声明 | **逻辑必须一致**；缩进和属性顺序没关系 |
| `script.js` | 语句和变量名 | **必须能逐条对应上**；变量名可以自己起，但含义要对 |

3. 用浏览器打开两个版本，**并排切换标签页**，做同样的操作，对比每一步的现象。

**只看效果不比代码行不行？**

不行。效果一致只能证明你**碰巧**写对了，无法暴露潜在的错误（比如把 `>=` 写成 `>` 在当前题库下没差别，但换题库就会出问题）。**代码必须能对上。**

#### 方法 A｜用 Git 做精确比对（推荐）

因为你的工程已经是 Git 仓库，比对可以直接用标签，**逐行、逐字**地看。

**先确认你的最终版本就是 `v1.0-final` 指向的内容**

```bash
git status
git show v1.0-final:style.css
```

**逐文件与原项目比对**（在你拥有 `lab/` 和 `quiz-app/` 两个目录的上一级目录执行）

```bash
git show v1.0-final:style.css > mine-style.css
git diff --no-index -w mine-style.css quiz-app/style.css
git show v1.0-final:script.js > mine-script.js
git diff --no-index mine-script.js quiz-app/script.js
```

| 命令 | 预期输出 |
| --- | --- |
| `style.css` 的 diff | **完全没有输出** —— CSS 应当逐字一致 |
| `script.js` 的 diff | 只有**排版类**差异（引号、分号、空格）；**不应有逻辑差异** |

比对完清理临时文件：

```bash
rm mine-style.css mine-script.js
```

> **`--no-index` 是关键**：它让 Git 比较两个**不在同一仓库里**的文件，不要求它们被 Git 跟踪。
>
> **`-w` 只在比对 CSS 时加**。`script.js` 的分号、引号差异是有意义的（涉及你选的代码风格约定），所以不加 `-w`，让它把风格差异也如实列出来——但你判断的标准是「**只看逻辑行**」。

#### 方法 B｜直接看两个标签之间的演进

这条命令能让你看清「我是怎么一步步把 quiz-app 写出来的」：

```bash
git diff v0.0-start v1.0-final --stat
```

**预期输出**

```
 .gitignore     |  8 +++++++
 index.html     | 34 ++++++++++++++++++++++++++++++
 script.js      | 95 +++++++++++++++++++++++++++++++++++++++++++++++++++++++
 style.css      | 66 ++++++++++++++++++++++++++++++++++++++++++
 4 files changed, 203 insertions(+)
```

和自己动手写之前对一下数量级。**如果你的行数明显偏少，多半是某个 Phase 偷懒了**（比如 `deselectAnswers()` 没写全）。

再看单个文件从哪个版本开始成型：

```bash
git log --oneline --decorate --follow -- index.html
```

**预期**：显示 `index.html` 在 `v0.1-init`（骨架）和 `v0.2-html`（完整）两次改动——**因为 Phase 2 之后你再也没碰过 HTML**。这本身就是一条验收信号：**如果这里出现第三次改动，说明你在 CSS 或 JS 阶段误改了 HTML**。

**逐个标签过一遍，确认每个都是可运行的**

```bash
git checkout v0.3-css
```

浏览器打开页面，确认此时：渐变背景、白色卡片、紫色按钮都在，选项文字还是 HTML 里写死的 `Java` / `C` / `Python` / `JavaScript`（因为 `v0.4-data` 才有题库）。

看完切回来：

```bash
git switch main
```

> 逐个 `checkout` 过去、刷新页面看一眼、再 `switch main` 回来——**这是本实验最有说服力的自查方式**，它证明你打的每个标签都真的是一个独立可运行的状态。

#### 方法 C｜辅助工具

```bash
git diff --no-index quiz-app/script.js lab/quiz-app/script.js
```

`-w` 会忽略所有空白差异：

```bash
git diff --no-index -w quiz-app/script.js lab/quiz-app/script.js
```

<a id="sec-7-3"></a>

### 7.3 三道进阶挑战题

基础版全部通过后，再做这三题。**每题都只给需求和验收标准，不给代码**——这是检验你是否真的理解了原理的时刻。

#### 挑战 1｜倒计时

**需求**：每题限时 15 秒，超时自动进入下一题；界面上显示剩余秒数。结算页显示「用时 xx 秒」。

**验收**

- [ ] 切题后计时**重新从 15 开始**
- [ ] 秒数每秒递减
- [ ] 归零时自动进入下一题（**视为未作答，不计分**）
- [ ] 最后一题超时后能正常进入结算页
- [ ] 点击 Reload 后计时重新开始

**提示**：需要 `setInterval`（定时器）和一个全局计时器变量；切题时要先 `clearInterval` 再启动新的，否则会叠加多个定时器。

#### 挑战 2｜进度与题号

**需求**：题干上方显示 `Question 1 / 4`；卡片顶部有一条进度条，随答题进度向右延伸。

**验收**

- [ ] 切题时题号同步更新
- [ ] 进度条随题目推进变长
- [ ] 结算页不显示进度条

**提示**：进度条需要一个额外 DOM 节点（`div` 里套一个 `div`），并在 `loadQuiz()` 里更新宽度。

#### 挑战 3｜本地存储最高分

**需求**：用 `localStorage` 记住历史最高分，结算页显示 `Best: x/4`；刷新后最高分不丢。

**验收**

- [ ] 首次答题后显示 `Best: 4/4`（以全对为例）
- [ ] 第二次只答 2 题时，`Best` 仍显示 `4/4`
- [ ] 第二次全对（超过原成绩）时，`Best` 更新
- [ ] 关闭浏览器重开，`Best` 仍在
- [ ] 结算页提供「清空记录」按钮

**提示**：`localStorage.setItem('key', 值)` 存，`localStorage.getItem('key')` 取，取出来是**字符串**需要 `parseInt`；在 `file://` 协议下某些浏览器会限制 `localStorage`，请用 Live Server 打开。

### 完成

三道挑战是**选做**。做完任意一道并通过验收，说明你已经从「照着写」跨到了「自己设计」——这才是这个实验真正想训练的能力。

---
