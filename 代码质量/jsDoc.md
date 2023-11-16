## jsDoc

我们使用vscode 编写函数过程中，函数的形参是一个字符串，我们在写代码的时候vscode 并不知道形参的类型，导致我们在写代码的时候并不会得到很好的代码提示，就像下面这样：

<img src=".\image\img1.jpg" alt="img1" style="zoom: 67%;" />

因为 JavaScript 弱类型，所以无法进行类型判断，往往要编译以后才发现错误。可以通过 jsDoc 在代码编写阶段就能告诉开发者数据类型。

### 一、概念

 jsDoc，顾名思义，jsDoc是一个用于JavaScript的API文档生成器，类似于Javadoc或phpDocumentor。他可以将文档注释直接添加到源代码中，就在代码本身旁边。jsDoc工具可以将扫描源代码并为您成一个HTML文档网站。

jsDoc注释通常应该放在代码被记录之前。为了被jsDoc解析器识别，每个注释必须以`/**`序列开头！

**vscode内置了jsDoc**，我们只需要在函数上面输入/** 然后就会提示，然后直接按回车就好！

对上面的函数使用 jsDoc：

<img src="E:\Note\fromTypora\Frontend\代码质量\image\img2.jpg" alt="img2" style="zoom:67%;" />

可以看到 vscode 的提醒都符合参数的数据类型 `string` ，并且鼠标悬停到函数上还能看到对函数的描述：

<img src="E:\Note\fromTypora\Frontend\代码质量\image\img3.jpg" alt="img3" style="zoom:67%;" />

这就是 jsDoc 的魅力之处。



### 二、简单使用

#### 1. 向代码中添加文档注释

JSDoc 的目的是记录 JavaScript 应用程序或库的 API。假设您想要记录诸如模块、名称空间、类、方法、方法参数等内容。

JSDoc注释通常应该放在记录代码之前。为了被 JSDoc 解析器识别，每个注释必须以 `/**` 序列开头。以 `/*`、`/***`开头或超过3颗星的注释将被忽略。这个特性用于控制解析注释块的功能。

最简单的文档示例就是描述：

```js
/** This is a description of the foo function. */
function foo() {
}
```

添加一个描述很简单--只需在文档注释中键入所需的说明。

可以使用特殊的 `JSDoc标签` 来提供更多信息。例如，如果函数是类的构造函数，则可以通过添加 [@constructor](https://www.jsdoc.com.cn/tags-class)标记来表示。

```js
/**
 * Represents a book.
 * @constructor
 */
function Book(title, author) {
}
```

使用标签添加更多的信息。

```jstext
/**
 * Represents a book.
 * @constructor
 * @param {string} title - The title of the book.
 * @param {string} author - The author of the book.
 */
function Book(title, author) {
}
```

#### 2. 生成网站

一旦你的代码是已注释的，你可以是用 JSDoc 3的工具从源文件中生成一个 HTML 网站。

默认情况下，JSDoc 使用内置的“默认”模板将文档转换为 HTML。您可以根据自己的需要编辑此模板，或者创建一个全新的模板（如果您喜欢的话）。

在命令行上运行文档生成器：

```shell
jsdoc book.js
```

此命令将在当前工作目录中创建名为 `out/` 的目录。在该目录中，您将找到生成的 HTML 页面。



###

[JSDoc 3中使用名称路径 | JSDoc中文文档 | JSDoc中文网](https://www.jsdoc.com.cn/about-namepaths)