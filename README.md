# Toc

为页面中的标题元素建立目录索引，这是一个不起眼却很实用的功能，特别是对于那些含有篇幅很长的文章的页面，拥有一个目录可以让读者对文章的结构一目了然，更重要的是，读者可以很轻松地在文章的各个章节之间来回快速跳转，极大地提高了用户体验。

使用 toc.js 可以快速为你的页面生成目录的 HTML 结构，toc.js 拥有较强的可定制性，并且 toc.js 并不会擅自为你做太多的事情，以便你更容易地把 toc.js 应用到你的项目中。

## 预览

![screenshot](https://github.com/totravel/toc/blob/master/screenshot.png?raw=true)

## 用法

在你的页面中引入 toc.js：

```html
<script type="text/javascript" src="/js/toc.js"></script>
```

请根据项目实际情况自行修改 `src` 属性。

toc.js 基于 jQuery，请确保在使用 Toc 之前引入 jQuery。

### 快速上手

实例化 Toc：

```javascript
var toc = new Toc();
```

然后调用 `make()` 方法：

```javascript
make(content, toc[, callback])
```

该方法接受两个必选参数：第一个参数 `content` 是页面中要为其中的标题元素建立目录索引的 jQuery 元素对象，通常就是 article 标签，第二个参数 `toc` 是用于存放目录索引的 jQuery 元素对象。

比如你的页面中有下面元素：

```html
<article>something...</article>
```

你要在下面的元素中为上面元素内的标题元素建立目录索引：

```html
<div class="toc"></div>
```

你应该这样调用 `make()` 方法：

```javascript
var content = $('article');
var wrapper = $('.toc');

toc.make(content, wrapper);
```

`make()` 方法将为 `content` 内的标题标签添加锚点，并在 `toc` 内生成对应的目录索引，生成的目录结构大致如下：

```html
<ul>
    <li>
        <a href="#toc-0">1 第一章</a>
        <ul>
            <li><a href="#toc-1">1.1 第一节</a>
                <ul>
                    <li><a href="#toc-2">1.1.1 第一点</a></li>
                    <li><a href="#toc-3">1.1.2 第二点</a></li>
                    <li><a href="#toc-4">1.1.3 第三点</a></li>
                </ul>
            </li>
            <li><a href="#toc-5">1.2 第二节</a></li>
        </ul>
    </li>
    <li><a href="#toc-6">2 第二章</a>
        <ul>
            <li><a href="#toc-7">3.1 第三节</a></li>
            <li><a href="#toc-8">2.1 第四节</a></li>
            <li><a href="#toc-9">2.2 第五节</a></li>
        </ul>
    </li>
    <li><a href="#toc-10">3 第三章</a>
    <li><a href="#toc-11">3 第四章</a>
        <ul>
            <li><a href="#toc-12">2.3 第六节</a></li>
        </ul>
    </li>
</ul>
```

如果你需要在目录索引生成后进行一些操作，可以给 `make()` 方法传入第三个参数：

```javascript
var content = $('article');
var wrapper = $('.toc');

var callback = function () {
    console.log('well done!');
};

toc.make(content, wrapper, callback);
```

### 更多

如果默认生成的目录索引不能满足你的需求，你可以在调用  `make()` 方法之前链式调用 `setting()` 方法来设定一些参数，甚至定义如何生成目录的 HTML 结构，`setting()` 方法接受一个对象作为唯一的必选参数：

```javascript
setting(config)
```

你可以在给传给 `setting()` 方法的对象定义以下属性或方法：

配置项 | 说明 | 默认值
---- | ---- | ----
headingSelector | 决定哪些级别的标题会出现在目录中 | `h2, h3, h4`
anchorPrefix | 每个标题锚点名的前缀 | `toc-`
makeItem() | 如何生成目录的每个条目 | 默认生成被 li 标签包含的 a 标签
makeWrapper() | 如何生成每个目录层级的容器 | 默认使用 ul 标签

#### headingSelector

`headingSelector` 属性是一个 jQuery 选择器，你可以通过定义它来决定哪些标题元素将会出现在目录中：

```javascript
var config = {
    headingSelector: 'h1, h2, h3',
};

toc.setting(config).make(content, wrapper, callback);
```

以上定义了只有一级标题、二级标题和三级标题会出现在目录中。

#### anchorPrefix

你可以定义 `anchorPrefix` 属性来决定每个标题锚点名的前缀，默认为 `toc-`。

#### makeItem()

你可以定义 `makeItem()` 方法来告诉 Toc 如何生成目录的每个条目：

```javascript
makeItem(anchor, text, level)
```

该方法接受三个参数：第一个参数 `anchor` 是对应标题的锚点；第二个参数 `text` 是对应标题的文本内容；第三个参数 `level` 是该标题的级别，你需要在此方法中返回目录条目的 HTML字符串、DOM 元素或者 jQuery 对象，默认的定义如下：

```javascript
function (anchor, text, level) {

    return '<li><a href="#' + anchor + '">' + text + '</a></li>';
}
```

#### makeWrapper()

你还可以定义 `makeItem()` 方法来告诉 Toc 如何生成每个目录层级的容器：

```javascript
makeWrapper(level)
```

默认的定义如下：

```javascript
function (level) {

    return '<ul></ul>';
}
```

如果你想单独设定最顶级目录层级的容器的生成方式，你可以这样定义 `makeWrap()` 方法：

```javascript
function (level) {
    // 其他容器的生成方式
    this.makeWrapper = function (level) {
        return '<ul></ul>';
    };

    return '<ul class="list-unstyled"></ul>';
}
```
