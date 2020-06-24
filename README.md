---
title: Markdown to PPT
speaker: Edward
plugins:
    - echarts: {theme: infographic}
    - mermaid: {theme: forest}
    - katex
---

<slide class="bg-black-blue aligncenter" image="https://source.unsplash.com/C1HhAQrbykQ/ .dark">
# Markdown To PPT {.text-landing .text-shadow}

By Edward  {.text-intro.animated.fadeInUp.delay-500}

[:fa-github: Github](https://github.com/crazybber){.button.ghost}

<slide :class="size-50 aligncenter">
### MARKDOWN 写PPT 神器 `nodeppt`

入坑，就四个命令：

```bash
-   npm i -g nodeppt
-   nodeppt new：使用线上模板创建一个新的 md 文件 {.animated.fadeInUp}
-   nodeppt serve：启动一个 md 文件的 webpack dev server
-   nodeppt build：在当前目录的dist目录下，编译产出html/js/css
```

<slide :class="size-60 aligncenter">
### 使用模式
{.animated.fadeInUp}

---
<h4 align="left">演讲者模式</h4>

nodeppt 有演讲者模式，在页面 url 后面增加`?mode=speaker` 既可以打开演讲者模式，双屏同步

<h4 align="left">Keyboard Shortcuts</h4>

-   Page: ↑/↓/←/→ Space Home End
-   Fullscreen: F
-   Overview: -/+
-   Speaker Note: N
-   Grid Background: Enter

<slide :class="size-70 aligncenter">
### public resource
---
<h2 align="center">公共资源：public 文件夹</h2>

如果项目文件夹下，存在`public`文件夹，可以直接通过 url 访问，参考`webpack dev server`的 `contentBase` 选项。

在`build`的时候，public 文件夹中的文件会完全 copy 到`dist`文件夹中



<slide :class="size-30 aligncenter">
### basic Using


整个 markdown 文件分为两部分，第一部分是写在最前面的**配置**，然后是使用`<slide>`隔开的每页幻灯片内容。


<slide class="bg-gradient-r" :class=" size-80 aligncenter" image="https://cn.bing.com/az/hprichbg/rb/WinterLynx_ZH-CN7158207296_1920x1080.jpg .dark">
### 配置

---
nodeppt 的配置是直接写在 md 文件顶部
-   title: 演讲主题 {.animated.fadeInUp}
-   speaker：演讲者  {.animated.fadeInUp.delay-400}
-   url：地址
-   js：js 文件数组，放到 body 之前
-   css：css 文件数组，放到头部
-   prismTheme：prism 配色，取值范围 `['dark', 'coy', 'funky', 'okaidia', 'tomorrow', 'solarizedlight', 'twilight']`
-   plugins：目前支持 [echarts](https://echarts.baidu.com/)，[mermaid](https://mermaidjs.github.io/)和 [katex](https://katex.org) 三个插件

<slide :class="size-50 aligncenter">
#### 插件

---
目前 nodeppt 支持 [图表 echarts](https://echarts.baidu.com/)，[流程图 mermaid](https://mermaidjs.github.io/)，[数学符号 KaTeX](https://katex.org) 三个插件。

echarts 主题配色可以直接在`yaml`配置的 js 中引入。echarts 采用`fence`语法，如下：

```echarts
{
    xAxis: {
        type: 'category',
        data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
    },
    yAxis: {
        type: 'value'
    },
    series: [{
        data: [820, 932, 901, 934, 1290, 1330, 1320],
        type: 'line'
    }]
}
```

详见[echarts.md](./echarts.md)


<slide :class="size-30 aligncenter">
#### mermaid

mermaid 主题配色可以直接在`yaml`配置的 js 中引入。mermaid 采用`fence`语法，如下：

```mermaid
sequenceDiagram
    Alice ->> Bob: Hello Bob, how are you?
    Bob-->>John: How about you John?
    Bob--x Alice: I am good thanks!
    Bob-x John: I am good thanks!
    Note right of John: Bob thinks a long<br/>long time, so long<br/>that the text does<br/>not fit on a row.

    Bob-->Alice: Checking with John...
    Alice->John: Yes... John, how are you?
```

详见[mermaid.md](./mermaid.md)

<slide :class="size-30 aligncenter">

#### ketex

参考：[markdown-it-katex](https://www.npmjs.com/package/markdown-it-katex)语法

<slide :class="size-30 aligncenter">

### `<slide>` 语法 1

nodeppt 会根据`<slide>`对整个 markdown 文件进行拆分，拆成单页的幻灯片内容。`<slide>` 标签支持下面标签：

```html
-   class/style 等：正常的 class 类，可以通过这个控制居中（aligncenter），内容位置，背景色等
-   image：背景图片，基本语法 `image="img_url"`
-   video：背景视频，基本语法 `video="video_src1,video_src2"`
-   :class：wrap 的 class，下面详解
```

每个 slide 会解析成下面的 html 结构：


```html
<section class="slide" attrs...><div class="wrap" wrap="true">// 具体 markdown 渲染的内容</div></section>
```

其中`<slide>` 的`class`等会被解析到 `<section>`标签上面，而`:class`则被解析到`div.wrap`上面，例如：

```html
`<slide :class="size-50" class="bg-primary"></slide>`
```

output 为：

```html
<section class="slide bg-primary"><div class="wrap size-50" wrap="true">// 具体 markdown 渲染的内容</div></section>
```

<slide :class="size-6 aligncenter">


### 背景：图片

`<slide>`的`image` 会被解析成背景大图，常见的支持方式有：

```md
`<slide image="https://source.unsplash.com/UJbHNoVPZW0/">`

# 这是一个普通的背景图

`<slide image="https://source.unsplash.com/UJbHNoVPZW0/ .dark">`

# 这张背景图会在图片上面蒙一层偏黑色的透明层

`<slide image="https://source.unsplash.com/UJbHNoVPZW0/ .light">`

# 这张背景图会在图片上面蒙一层偏白色的透明层

`<slide class="bg-black aligncenter" image="https://source.unsplash.com/n9WPPWiPPJw/ .anim">`

# 这张背景图会缓慢动
```

详见[site/background.md](./site/background.md)和[在线演示](https://js8.in/nodeppt/background.html)


<slide>
### 样式,布局 default style


样式太多，具体详见[classes.md](classes.md)和[在线演示](https://js8.in/nodeppt/classes.html)


nodeppt 这次使用`webslides`的布局，支持丰富的布局，直接看文档[layout.md](./layout.md)和[在线演示](https://js8.in/nodeppt/layout.html)


<slide :class="size-30 content-left" >
### attribute

参考[markdown-it-attrs](https://www.npmjs.com/package/markdown-it-attrs)，支持了`attribute`，修改增加多 class 支持等功能。

其中：`..class`会往上一级节点添加 class，支持`{.class1.class2}`这种多 class 的语法。用法举例：

```markdown
# header {.style-me.class2}

paragraph {data-toggle=modal}
```

Output:

```html
<h1 class="style-me class2">header</h1>
<p data-toggle="modal">paragraph</p>
```

```markdown
Use the css-module green on this paragraph. {.text-intro}
```

Output:

```html
<p class="text-intro">Use the css-module green on this paragraph.</p>
```

```markdown
-   list item **bold** {.red}
```

Output:

```html
<ul>
    <li class="red">list item <strong>bold</strong></li>
</ul>
```

```markdown
-   list item **bold**
    {.red}
```

Output:

```html
<ul class="red">
    <li>list item <strong>bold</strong></li>
</ul>
```

<slide :class="size-20 content-left" >
### image 增强

对于 image ，支持外面包裹一层的写法，具体语法 `!![](图片地址 属性)`，例如：

```markdown
!![](https://webslides.tv/static/images/iphone.png .size-50.alignleft)
```

Output：

```html
<img src="https://webslides.tv/static/images/iphone.png" class="size-50 alignleft" />
```

```markdown
!![figure](https://webslides.tv/static/images/setup.png .aligncenter)
```

Output:

```html
<figure><img src="https://webslides.tv/static/images/setup.png" class="aligncenter" /></figure>
```


<slide :class="size-20 content-left" >
### button

nodeppt 的 button 是类似`link`语法的，支持蓝色、圆角、空心和 icon 版本的 button：

```markdown
[普通按钮](){.button} [圆角普通按钮](){.button.radius}

[空心](){.button.ghost} [:fa-github: 前面带 icon](){.button}
```

<slide :class="size-30 content-left">

### Icon：FontAwesome

nodeppt 的 icon 支持 [FontAwesome](https://fontawesome.com/icons?d=gallery)

语法：
```bash
-   `:fa-xxx:` → `<i class="fa fa-xxx"></i>`
-   `:~fa-xxx:~` → `<span><i class="fa fa-xxx"></i></span>`
-   `::fa-xxx::` → 块级`<i class="fa fa-xxx"></i>`，即不会被`p`包裹

```

<slide :class="size-30 alignleft">

### span

代码修改自[markdown-it-span](https://github.com/pnewell/markdown-it-span/)，支持 `attr`语法，基本用法：

```md
:span:
:span: {.text-span}
```

<slide :class="size-30 content-left">

### 动效

nodeppt 一如既往的支持动效，2.0 版本支持动效主要是页面内的动效。

支持动效包括：

```bash
-   fadeIn
-   zoomIn
-   rollIn
-   moveIn
-   fadeInUp
-   slow
```
在需要支持的动效父节点添加`.build`或者在具体的某个元素上添加`.tobuild+动效 class`即可。

按照惯例，nodeppt 还支持`animate.css`的动效哦~

详细查看文件：[site/animation.md](./site/animation.md)和[在线演示](https://js8.in/nodeppt/animation.html)

<slide :class="size-30 content-left">
### 使用强大的`:::`完成复杂布局

`:::`语法是扩展了 [markdown-it-container](https://www.npmjs.com/package/markdown-it-container) 语法，默认是任意 tag，例如

```markdown
:::div {.content-left}

## title

:::
```
Output：

```html
<div class="content-left"><h2>title</h2></div>
```

还支持，`tag` 嵌套，除此之外，支持的组件包括：

```html
-   card：卡片，一边是图片，一边是内容
-   column：column 多栏布局
-   shadowbox：带阴影的盒子
-   steps：步骤组件
-   cta：
-   gallery：图片
-   flexblock：flex block 布局，支持多个子类型
-   note: 演讲注释
```

<slide :class="size-30 content-left">
### 使用强大的`:::`完成复杂布局 2

基本语法是：

```markdown
:::TYPE {.attrs}

## 第一部分
使用 hr 标签隔开
---
## 第二部分
这里的内容也是哦
:::
```

详细可以看 [component](./site/component.md) 部分的 markdown 文件和[在线演示](https://js8.in/nodeppt/component.html)

<h2 align="center">打印？导出 pdf？</h2>

chrome 浏览器，直接在第一页 `command+P/ctrl+P` 即可


<slide :class="size-30 aligncenter">

### `nodeppt.config.js`

在 nodeppt 执行路径下创建`nodeppt.config.js`文件，可以配置跟`webpack`相关的选项，另外可以支持自研 nodeppt 插件。

默认内置的`config.js`内容如下：

```js
/**
 * @file 默认配置
 */
module.exports = () => ({
    // project deployment base
    baseUrl: '/',

    // where to output built files
    outputDir: 'dist',

    // where to put static assets (js/css/img/font/...)
    assetsDir: '',

    // filename for index.html (relative to outputDir)
    indexPath: 'index.html',
    // 插件，包括 markdown 和 posthtml
    plugins: [],
    // chainWebpack: [],

    // whether filename will contain hash part
    filenameHashing: true,

    // boolean, use full build?
    runtimeCompiler: false,

    // deps to transpile
    transpileDependencies: [
        /* string or regex */
    ],

    // sourceMap for production build?
    productionSourceMap: true,

    // use thread-loader for babel & TS in production build
    // enabled by default if the machine has more than 1 cores
    parallel: () => {
        try {
            return require('os').cpus().length > 1;
        } catch (e) {
            return false;
        }
    },

    // multi-page config
    pages: undefined,

    // <script type="module" crossorigin="use-credentials">
    // #1656, #1867, #2025
    crossorigin: undefined,

    // subresource integrity
    integrity: false,

    css: {
        extract: true
        // modules: false,
        // localIdentName: '[name]_[local]_[hash:base64:5]',
        // sourceMap: false,
        // loaderOptions: {}
    },

    devServer: {
        /*
      host: '0.0.0.0',
      port: 8080,
      https: false,
      proxy: null, // string | Object
      before: app => {}
    */
    }
});
```

<slide :class="size-30 aligncenter">

### parser plugin

解析插件分两类： `markdown-it` 和 `posthtml`，

-   markdown-it：是解析 markdown 文件的，如果是增强 markdown 语法，可以用这类插件
-   posthtml：是处理 html 标签的，如果是修改输出的 html 内容，可以用这类插件

定义一个 plugin ：

```js
module.exports = {
    // 这里的 id 必须以 markdown/posthtml开头
    // 分别对应 markdown-it和 posthtml 插件语法
    id: 'markdown-xxx',
    // 这里的 apply 是插件实际的内容，详细查看 markdown-it和 posthtml 插件开发
    apply: () => {}
};
```

-   [markdown-it docs](https://github.com/markdown-it/markdown-it/tree/master/docs)
-   [posthtml docs](https://github.com/posthtml/posthtml/tree/master/docs)


<slide :class="size-30 aligncenter">


### webslides plugin

WebSlides 插件需要写到一个 js 文件中，然后作为数组放到`window.WSPlugins_`中，然后通过在 md 页面的配置（yaml）添加 js 的方法引入。

```md
js: - webslide_plugins.js
```

```js
// webslide_plugins.js内容
window.WSPlugins_ = [
    {
        id: 'webslide_plugin_name',
        // 下面是对应的插件类
        apply: class Plugin {}
    }
];
```

参考[WebSlides 文档](https://github.com/webslides/WebSlides/wiki/Plugin-development)

<slide :class="size-30 aligncenter">

### Template：自制模板

参考[nodeppt-template-default](https://github.com/ksky521/nodeppt-template-default)。


### End

End

