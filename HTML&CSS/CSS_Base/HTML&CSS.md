# HTML & CSS

## HTML

### 一、结构

```html
<!doctype html>
<html lang="zh-CN">
  <head>
    <meta charset="utf-8" />
    <title>我的测试页面</title>
  </head>
  <body>
    <p>这是我的页面</p>
  </body>
</html>
```

1. `<!DOCTYPE html>`: 声明文档类型。早期的 HTML（大约 1991-1992 年）文档类型声明类似于链接，规定了 HTML 页面必须遵从的良好规则，能自动检测错误和其他有用的东西。文档类型使用类似于这样：

   ```html
   <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
   ```

   文档类型是一个历史遗留问题，需要包含它才能使其他东西正常工作。现在，只需要知道`<!DOCTYPE html>`是最短的有效文档声明！

2. `<html></html>`:这个元素包裹了页面中所有的内容，有时被称为根元素。

3. `<head></head>`: 这个元素是一个容器，它包含了所有你想包含在 HTML 页面中但**不在 HTML 页面中显示**的内容。这些内容包括你想在搜索结果中出现的关键字和页面描述、CSS 样式、字符集声明等等。以后的章节中会学到更多相关的内容。

4. `<meta charset="utf-8">`: 这个元素代表了不能由其他 HTML 元相关元素表示的元数据，比如`base`、`link` 等。`charset`属性将你的文档的字符集设置为 UTF-8，其中包括绝大多数人类书面语言的大多数字符。有了这个设置，页面现在可以处理它可能包含的任何文本内容。没有理由不对它进行设置，它可以帮助避免以后的一些问题。

5. `<title></title>`: 这设置了页面的标题，也就是出现在该页面加载的浏览器标签中的内容。当页面被加入书签时，页面标题也被用来描述该页面。

6. `<body></body>`: 包含了你访问页面时*所有*显示在页面上的内容，包含文本、图片、视频、游戏、可播放音频轨道等等。

#### 1. head 头

HTML 头部包含 HTML `head`元素的内容，与 `body` 元素内容不同，页面在浏览器加载后它的内容不会在浏览器中显示，它的作用是保存页面的一些[元数据](https://developer.mozilla.org/zh-CN/docs/Glossary/Metadata)。上述示例的头部非常简短：

```html
<head>
  <meta charset="utf-8" />
  <title>我的测试页面</title>
</head>
```

然而，大型页面的头部会相当大。可以试着到一些喜欢的网站上，使用[开发者工具](https://developer.mozilla.org/zh-CN/docs/Learn/Common_questions/Tools_and_setup/What_are_browser_developer_tools)查看网页的头部内容。

##### title 标题

`title` 元素是一项元数据，用于表示整个 HTML 文档的标题（而不是文档内容）。

`<title>` 元素也被以其他的方式使用着。比如说，如果你尝试为某个页面添加书签（在 Firefox 浏览器中，点击*书签 > 将当前标签页添加到书签*，或点击地址栏末尾的星标），你会看到 `<title>` 的内容被作为建议的书签名。

##### meta 元数据

元数据就是描述数据的数据，而 HTML 有一个“官方的”方式来为一个文档添加元数据——[`meta`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/meta) 元素。当然，其他在这篇文章中提到的东西也可以被当作元数据。有很多不同种类的 `<meta>` 元素可以被包含进你的页面的 `<head>` 元素，但是现在我们还不会尝试去解释所有类型，这只会引起混乱。我们会解释一些你常会看到的类型，先让你有个概念。

- 设置字符编码，更改charset

  ```html\
  <meta charset="utf-8" />
  ```

- 添加作者和描述

  许多 `<meta>` 元素包含了 `name` 和 `content` 属性：

  - `name` 指定了 meta 元素的类型；说明该元素包含了什么类型的信息。
  - `content` 指定了实际的元数据内容。

  这两个 meta 元素对于定义你的页面的作者和提供页面的简要描述是很有用的。让我们看一个例子：

  ```html
  <meta name="author" content="Chris Mills" />
  <meta
    name="description"
    content="The MDN Web Docs Learning Area aims to provide
  complete beginners to the Web with all they need to know to get
  started with developing web sites and applications." />
  ```

- 其他类型的元数据

  当你在网站上查看源码时，你也会发现其他类型的元数据。你在网站上看到的许多功能都是专有创作，旨在向某些网站（如社交网站）提供可使用的特定信息。

  例如 QQ 的元数据协议：

  ```html
  <meta itemprop="name" content="标题"> 
  <meta itemprop="Description" content="描述内容"> 
  <meta itemprop="image" content="显示的图片URL">
  ```

  发送自己网页的链接时（不是打开网站后分享给好友，单单指将网页链接以文本的形式发送出去），默认情况下是不会以卡片形式显示的，加上这些元数据就可以显示卡片。

##### 站点自定义图标

页面添加网页图标的方式为：

1. 将其保存在与网站的索引页面相同的目录中，以 `.ico` 格式保存（大多数浏览器支持更通用的格式，如 `.gif` 或 `.png`）

2. 将以下行添加到 HTML 的`<head>`块中以引用它：

   ```html
   <link rel="icon" href="favicon.ico" type="image/x-icon" />
   ```

#### 2. 使用 CSS 和 JS

分别使用 [`link`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/link) 元素以及 [`script`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/script) 元素来引入 CSS 和 JS。

- `<link>`元素经常位于文档的头部，它有 2 个属性，`rel="stylesheet"` 表明这是文档的样式表， `href` 包含了样式表文件的路径：

  ```html
  <link rel="stylesheet" href="my-css-file.css" />
  ```

- `<script>`元素也应当放在文档的头部，并包含 `src` 属性来指向需要加载的 JavaScript 文件路径，同时最好加上 `defer` 以告诉浏览器在解析完成 HTML 后再加载 JavaScript。这样可以确保在加载脚本之前浏览器已经解析了所有的 HTML 内容。这样你就不会因为 JavaScript 试图访问页面上不存在的 HTML 元素而产生错误。实际上有很多方法来处理在你的页面上加载 JavaScript，但对于现代浏览器来说，这是最可靠的方法（对于其他方法，请阅读[脚本加载策略](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/First_steps/What_is_JavaScript#%E8%84%9A%E6%9C%AC%E8%B0%83%E7%94%A8%E7%AD%96%E7%95%A5)

  ```html
  <script src="my-js-file.js" defer></script>
  ```

#### 3. 为文档设定主语言

可以（而且有必要）为站点设定语言，这个可以通过添加 [lang 属性](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Global_attributes/lang)到 HTML 开始的标签中来实现（就像 [meta-example.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/the-html-head/meta-example.html) 那样），如下所示：

```html
<html lang="zh-CN">
  …
</html>
```

这在很多方面都很有用。如果你的 HTML 文档的语言设置好了，那么你的 HTML 文档就会被搜索引擎更有效地索引（例如，允许它在特定于语言的结果中正确显示），对于那些使用屏幕阅读器的视障人士也很有用（例如，法语和英语中都有“six”这个单词，但是发音却完全不同）。

你还可以将文档的分段设置为不同的语言。例如，我们可以把日语部分设置为日语，如下所示：

```html
<p>Japanese example: <span lang="ja">ご飯が熱い。</span>.</p>
```
