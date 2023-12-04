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



### 二、多媒体

我们可以用 `img` 元素来把图片放到网页上。它是一个空元素（它不需要包含文本内容或闭合标签），最少只需要一个 `src` （一般读作其全称 *source）*来使其生效。`src` 属性包含了指向我们想要引入的图片的路径，可以是相对路径或绝对 URL，就像 `a` 元素的 `href` 属性一样。

举个例子来看，如果你有一幅文件名为 `dinosaur.jpg` 的图片，且它与你的 HTML 页面存放在相同路径下，那么你可以这样嵌入它：

```html
<img src="dinosaur.jpg" />
```

#### 1. 图片备选文本

下一个我们讨论的属性是 `alt` ，它的值应该是对图片的文字描述，用于在图片无法显示或不能被看到的情况。例如，上面的例子可以做如下改进：

```html
<img
  src="images/dinosaur.jpg"
  alt="The head and torso of a dinosaur skeleton;
          it has a large head with long sharp teeth" />
```

测试`alt` 属性最简单的方式就是故意拼错图片文件名，这样浏览器就无法找到该图片从而显示备选的文本。如果我们将上例的图片文件名改为 `dinosooooor.jpg`，浏览器就不能显示图片，而显示备选文本。

那么，为什么我们需要备选文本呢？它可以派上用场的原因有很多：

- 用户有视力障碍，通过[屏幕阅读器](https://zh.wikipedia.org/wiki/螢幕閱讀器)来浏览网页。事实上，给图片一个备选的描述文本对大多数用户都是很有用的。
- 就像上面所说的，你也许会把图片的路径或文件名拼错。
- 浏览器不支持该图片类型。某些用户仍在使用纯文本的浏览器，例如 [Lynx](https://en.wikipedia.org/wiki/Lynx_(web_browser))，这些浏览器会把图片替换为描述文本。
- 你会想提供一些文字描述来给搜索引擎使用，例如搜索引擎可能会将图片的文字描述和查询条件进行匹配。
- 用户关闭的图片显示以减少数据的传输，这在手机上是十分普遍的，并且在一些国家带宽有限且昂贵。

你到底应该在 `alt` 里写点什么呢？这首先取决于为什么这张图片会在这儿，换句话说，如果这张图片没显示出来，会少了什么：

- **装饰：** 如果图片只是用于装饰，而不是内容的一部分，可以写一个空的`alt=""` 。例如，屏幕阅读器不会浪费时间对用户读出不是核心需要的内容。实际上装饰性图片就不应该放在 HTML 文件里， [CSS 背景图片](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Multimedia_and_embedding/Images_in_HTML#css_背景图片)才应该用于插入装饰图片，但如果这种情况无法避免， `alt=""`会是最佳处理方案。
- **内容：** 如果你的图片提供了重要的信息，就要在`alt`文本中简要的提供相同的信息，甚至更近一步，把这些信息写在主要的文本内容里，这样所有人都能看见。不要写冗余的备选文本（如果在主要文本中将所有的段落都重复两遍，对于没有失明的用户来说多烦啊！），如果在主要文本中已经对图片进行了充分的描述，写`alt=""`就好。
- **链接：** 如果你把图片嵌套在`a`标签里，来把图片变成链接，那你还必须[提供无障碍的链接文本](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Introduction_to_HTML/Creating_hyperlinks#用清晰的链接措辞。)。在这种情况下，你可以写在同一个`<a>`元素里，或者写在图片的`alt`属性里，随你喜欢。
- **文本：** 你不应该将文本放到图像里。例如，如果你的主标题需要有阴影，你可以[用 CSS](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-shadow)来达到这个目的，而不是把文本放到图片里。如果真的必须这么做，那就把文本也放到`alt`里。

本质上，关键在于在图片无法被看见时也提供一个可用的体验，这确保了所有人都不会错失一部分内容。尝试在浏览器中使图片不可见然后看看网页变成什么样了，你会很快意识到在图片无法显示时备选文本能帮上多大忙。

#### 2. 图片标题

类似于[超链接](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Introduction_to_HTML/Creating_hyperlinks#使用_title_属性添加支持信息)，你可以给图片增加 `title` 属性来提供需要更进一步的支持信息。在我们的例子中，可以这样做：

```html
<img
  src="images/dinosaur.jpg"
  alt="一只恐龙头部和躯干的骨架，它有一个巨大的头，长着锋利的牙齿。"
  width="400"
  height="341"
  title="A T-Rex on display in the Manchester University Museum" />
```

图片标题并不必须要包含有意义的信息，通常来说，将这样的支持信息放到主要文本中而不是附着于图片会更好。不过，在有些环境中这样做更有用，比如当没有空间显示提示时，也就是在图片栏中。

然而，这并不是推荐的——title 有很多易访问性问题，主要是基于这样一个事实，即屏幕阅读器的支持是不可预测的，大多数浏览器都不会显示它，除非你在鼠标悬停时

#### 3. 视频

`<video>` 允许你轻松地嵌入一段视频。一个简单的例子如下：

```html
<video src="rabbit320.webm" controls>
  <p>
    你的浏览器不支持 HTML5 视频。可点击<a href="rabbit320.mp4">此链接</a>观看
  </p>
</video>
```

当中的一些属性如下：

- `src`

  同 `img` 标签使用方式相同，`src` 属性指向你想要嵌入网页当中的视频资源，他们的使用方式完全相同。

- `controls`

  用户必须能够控制视频和音频的回放功能。你可以使用 `controls` 来包含浏览器提供的控件界面，同时你也可以使用合适的 [JavaScript API](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLMediaElement) 创建自己的界面。界面中至少要包含开始、停止以及调整音量的功能。

- `video` 标签内的内容

  这个叫做**后备内容** — 当浏览器不支持 `<video>` 标签的时候，就会显示这段内容，这使得我们能够对旧的浏览器提供回退内容。你可以添加任何后备内容，在这个例子中我们提供了一个指向这个视频文件的链接，从而使用户至少可以访问到这个文件，而不会局限于浏览器的支持。

#### 4. 使用多个播放源以提高兼容性

以上的例子中有一个问题，你可能已经注意到了，如果你尝试使用像 Safari 或者 Internet Explorer 这些浏览器来访问上面的链接。视频并不会播放，这是因为不同的浏览器对视频格式的支持不同。幸运的是，你有办法防止这个问题发生。

```html
<video controls>
  <source src="rabbit320.mp4" type="video/mp4" />
  <source src="rabbit320.webm" type="video/webm" />
  <p>
    你的浏览器不支持 HTML5 视频。可点击<a href="rabbit320.mp4">此链接</a>观看
  </p>
</video>
```

现在我们将 `src` 属性从 `<video>` 标签中移除，转而将它放在几个单独的标签 `src`  当中。在这个例子当中，浏览器将会检查 `<source>` 标签，并且播放第一个与其自身 codec 相匹配的媒体。你的视频应当包括 WebM 和 MP4 两种格式，这两种在目前已经足够支持大多数平台和浏览器。

每个 `<source>` 标签页含有一个 `type` 属性，这个属性是可选的，但是建议你添加上这个属性 — 它包含了视频文件的 [MIME types](https://developer.mozilla.org/zh-CN/docs/Glossary/MIME_type) ，同时浏览器也会通过检查这个属性来迅速的跳过那些不支持的格式。如果你没有添加 `type` 属性，浏览器会尝试加载每一个文件，直到找到一个能正确播放的格式，这样会消耗掉大量的时间和资源。

#### 5. 音频

`<audio>` 标签与 `<video>` 标签的使用方式几乎完全相同，有一些细微的差别比如下面的边框不同，一个典型的例子如下：

```html
<audio controls>
  <source src="viper.mp3" type="audio/mp3" />
  <source src="viper.ogg" type="audio/ogg" />
  <p>你的浏览器不支持 HTML5 音频，可点击<a href="viper.mp3">此链接</a>收听。</p>
</audio>
```

音频播放器所占用的空间比视频播放器要小，由于它没有视觉部件 — 你只需要显示出能控制音频播放的控件。一些与 HTML5 `<video>` 的差异如下：

- `audio` 标签不支持 `width`/`height` 属性 — 由于其并没有视觉部件，也就没有可以设置 `width`/`height` 的内容。
- 同时也不支持 `poster` 属性 — 同样，没有视觉部件。

除此之外，`<audio>` 标签支持所有 `<video>` 标签拥有的特性

#### 6. 重新播放

任何时候，你都可以在 Javascript 中调用 [`load()`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLMediaElement/load) 方法来重置媒体。如果有多个由 `source` 标签指定的媒体来源，浏览器会从选择媒体来源开始重新加载媒体。

```js
const mediaElem = document.getElementById("my-media-element");
mediaElem.load();
```

#### 7. 音轨增删事件

你可以监控媒体元素中的音频轨道，当音轨被添加或删除时，你可以通过监听相关事件来侦测到。具体来说，通过监听 [`AudioTrackList` (en-US)](https://developer.mozilla.org/en-US/docs/Web/API/AudioTrackList) 对象的 `addtrack` 事件（即 [`HTMLMediaElement.audioTracks`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLMediaElement/audioTracks) 对象），你可以及时对音轨的增加做出响应。

```js
const mediaElem = document.querySelector("video");
mediaElem.audioTracks.onaddtrack = function (event) {
  audioTrackAdded(event.track);
};

```



### 三、嵌入技术

到目前为止，你应该掌握了将图像、视频和音频嵌入到网页上的诀窍了。此刻，让我们继续深入学习，来看一些能让你在网页中嵌入各种内容类型的元素：[`iframe`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe), [`embed`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/embed) 和 [`object`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/object) 元素。

#### 1. iframe

`<iframe>` 元素旨在允许你将其他 Web 文档嵌入到当前文档中。这很适合将第三方内容嵌入你的网站，你可能无法直接控制，也不希望实现自己的版本——例如来自在线视频提供商的视频，Disqus 等评论系统，在线地图提供商，广告横幅等。你通过本课程使用的实时可编辑示例就是使用 `<iframe>` 实现的。

我们会在后面提到，关于 `<iframe>` 有一些严重的[安全隐患](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Multimedia_and_embedding/Other_embedding_technologies#安全隐患)需要考虑，但这并不意味着你不应该在你的网站上使用它们——它只需要一些知识和仔细地思考。让我们更详细地探索这些代码。假设你想在其中一个网页上加入 MDN 词汇表，你可以尝试以下方式：

```html
<iframe
  src="https://developer.mozilla.org/zh-CN/docs/Glossary"
  width="100%"
  height="500"
  frameborder="0"
  allowfullscreen
  sandbox>
  <p>
    <a href="https://developer.mozilla.org/zh-CN/docs/Glossary">
      Fallback link for browsers that don't support iframes
    </a>
  </p>
</iframe>
```

此示例包括使用以下所需的 `<iframe>` 基本要素：

- [`allowfullscreen`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe#attr-allowfullscreen)

  如果设置，`<iframe>`则可以通过[全屏 API](https://developer.mozilla.org/zh-CN/docs/Web/API/Fullscreen_API) 设置为全屏模式（稍微超出本文的范围）。

- [`frameborder`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe#attr-frameborder)

  如果设置为 1，则会告诉浏览器在此框架和其他框架之间绘制边框，这是默认行为。0 删除边框。不推荐这样设置，因为在 [CSS 中](https://developer.mozilla.org/zh-CN/docs/Glossary/CSS)可以更好地实现相同的效果。[`border`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border)`: none;`

- [`src`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe#attr-src)

  该属性与 [`video`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/video) / 元素表示文档中的图像。[`img`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/img)一样包含指向要嵌入文档的 URL 路径。

- [`width`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe#attr-width) 和 [`height`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe#attr-height)

  这些属性指定你想要的 iframe 的宽度和高度。

- [备选内容](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Multimedia_and_embedding/Other_embedding_technologies#备选内容)

  与 [`video`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/video) 等其他类似元素相同，你可以在 `<iframe></iframe>` 标签之间包含备选内容，如果浏览器不支持 `<iframe>`，将会显示备选内容，这种情况下，我们已经添加了一个到该页面的链接。现在你几乎不可能遇到任何不支持 `<iframe>` 的浏览器。

- [`sandbox`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe#attr-sandbox)

  该属性需要在已经支持其他 `<iframe>` 功能（例如 IE 10 及更高版本）但稍微更现代的浏览器上才能工作，该属性可以提高安全性设置；我们将在下一节中更加详细地谈到。

#### 2. 安全隐患

浏览器制造商和 Web 开发人员了解到网络上的坏人（通常被称为**黑客**，或更准确地说是**破解者**），如果他们试图恶意修改你的网页或欺骗人们进行不想做的事情时常把 iframe 作为共同的攻击目标（官方术语：**攻击向量**），例如显示用户名和密码等敏感信息。因此，规范工程师和浏览器开发人员已经开发了各种安全机制，使`<iframe>`更加安全，这有些最佳方案值得我们考虑——我们将在下面介绍其中的一些。

**备注：** [单击劫持](https://en.wikipedia.org/wiki/Clickjacking)是一种常见的 iframe 攻击，黑客将隐藏的 iframe 嵌入到你的文档中（或将你的文档嵌入到他们自己的恶意网站），并使用它来捕获用户的交互。这是误导用户或窃取敏感数据的常见方式。

一个快速的例子——尝试在浏览器中加载上面的例子——你也可以 [在 Github 上找到它](https://mdn.github.io/learning-area/html/multimedia-and-embedding/other-embedding-technologies/iframe-detail.html)（[参见源代码](https://github.com/mdn/learning-area/blob/gh-pages/html/multimedia-and-embedding/other-embedding-technologies/iframe-detail.html)）。你将不会看到任何内容，但如果你点击[浏览器开发者工具](https://developer.mozilla.org/zh-CN/docs/Learn/Common_questions/Tools_and_setup/What_are_browser_developer_tools)中的*控制台*，你会看到一条消息，告诉你为什么没有显示内容。在 Firefox 中，你会*被告知：“X-Frame-Options 拒绝加载 `https://developer.mozilla.org/zh-CN/docs/Glossary`*。这是因为构建 MDN 的开发人员已经在网站页面的服务器上设置了一个不允许被嵌入到`<iframe>`的设置（请参阅[配置 CSP 指令](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Multimedia_and_embedding/Other_embedding_technologies#配置_csp_指令)）这是有必要的——整个 MDN 页面被嵌入在其他页面中没有多大意义，除非你想要将其嵌入到你的网站上并将其声称为自己的内容，或尝试通过单击劫持来窃取数据，这都是非常糟糕的事情。此外，如果每个人都这样做，所有额外的带宽将花费 Mozilla 很多资金。

##### 只有在必要时嵌入

有时嵌入第三方内容（例如 YouTube 视频和地图）是有意义的，但如果你只在完全需要时嵌入第三方内容，你可以省去很多麻烦。网络安全的一个很好的经验法则是“*你怎么谨慎都不为过，如果你决定要做这件事，多检查一遍；如果是别人做的，在被证明是安全的之前，都假设这是危险的。*”



### 四、SVG

[SVG](https://developer.mozilla.org/zh-CN/docs/Web/SVG) 是用于描述矢量图像的[XML](https://developer.mozilla.org/zh-CN/docs/Glossary/XML)语言。它基本上是像 HTML 一样的标记，只是你有许多不同的元素来定义要显示在图像中的形状，以及要应用于这些形状的效果。SVG 用于标记图形，而不是内容。非常简单，你有一些元素来创建简单图形，如 `circle`和 `rect` 。更高级的 SVG 功能包括 `feColorMatrix`（使用变换矩阵转换颜色）`animate`（矢量图形的动画部分）和 `mask`（在图像顶部应用模板）

作为一个简单的例子，以下代码创建一个圆和一个矩形：

```html
<svg
  version="1.1"
  baseProfile="full"
  width="300"
  height="200"
  xmlns="http://www.w3.org/2000/svg">
  <rect width="100%" height="100%" fill="black" />
  <circle cx="150" cy="100" r="90" fill="blue" />
</svg>
```

这将创建以下输出：

<svg
  version="1.1"
  baseProfile="full"
  width="300"
  height="200"
  xmlns="http://www.w3.org/2000/svg">
  <rect width="100%" height="100%" fill="black" />
  <circle cx="150" cy="100" r="90" fill="blue" />
</svg>



## CSS

































