# Bootstrap4 安装使用

**CDN**

```js
<!-- jQuery文件。务必在bootstrap.min.js 之前引入 -->
<script src="https://cdn.bootcss.com/jquery/3.2.1/jquery.min.js"></script>
 
<!-- popper.min.js 用于弹窗、提示、下拉菜单 -->
<script src="https://cdn.bootcss.com/popper.js/1.12.5/umd/popper.min.js"></script>
 
<!-- 最新的 Bootstrap4 核心 JavaScript 文件 -->
<script src="https://cdn.bootcss.com/bootstrap/4.1.0/js/bootstrap.min.js"></script>
```

**NPM**

```js
npm install bootstrap@4.0.0-beta.2
gem 'bootstrap', '~> 4.0.0.beta2'
composer require twbs/bootstrap:4.0.0-beta.2
```

# 容器类

```css
容器类

Bootstrap 4 需要一个容器元素来包裹网站的内容。

我们可以使用以下两个容器类：

    .container 类用于固定宽度并支持响应式布局的容器。
    .container-fluid 类用于 100% 宽度，占据全部视口（viewport）的容器。 
```

![img](http://www.runoob.com/wp-content/uploads/2017/10/176B67B9-013C-429C-8FD0-BC2409011545.jpg)

# 网格系统 

- .col- 针对所有设备
- .col-sm- 平板  - 屏幕宽度等于或大于 576px
- .col-md- 桌面显示器  - 屏幕宽度等于或大于 768px)
- .col-lg- 大桌面显示器  - 屏幕宽度等于或大于 992px)
- .col-xl- 超大桌面显示器 - 屏幕宽度等于或大于 1200px)

```html

<!-- 第一个例子：控制列的宽度及在不同的设备上如何显示 -->
<div class="row">
  <div class="col-*-*"></div>
</div>
<div class="row">
  <div class="col-*-*"></div>
  <div class="col-*-*"></div>
  <div class="col-*-*"></div>
</div>
 
<!-- 第二个例子：或让 Bootstrap 者自动处理布局 -->
<div class="row">
  <div class="col"></div>
  <div class="col"></div>
  <div class="col"></div>
</div>


<div class="container-fluid">
  <div class="row">
    <div class="col-sm-3 col-md-6">
      <p>RUNOOB</p>
    </div>
    <div class="col-sm-9 col-md-6">
      <p>菜鸟教程</p>
    </div>
  </div>
</div>


<div class="container-fluid">
  <div class="row">
    <div class="col-sm-3 col-md-6 col-lg-4 col-xl-2">
      <p>RUNOOB</p>
    </div>
    <div class="col-sm-9 col-md-6 col-lg-8 col-xl-10">
      <p>菜鸟教程</p>
    </div>
  </div>
</div>

```

偏移列通过 offset-*-* 类来设置。第一个星号( * )可以是 **sm、md、lg、xl**，表示屏幕设备类型，第二个星号( * )可以是 **1** 到 **11** 的数字。

为了在大屏幕显示器上使用偏移，请使用 **.offset-md-\*** 类。这些类会把一个列的左外边距（margin）增加 ***** 列，其中 ***** 范围是从 **1** 到 **11**。

```html
<div class="row">
  <div class="col-md-4">.col-md-4</div>
  <div class="col-md-4 offset-md-4">.col-md-4 .offset-md-4</div>
</div>
<div class="row">
  <div class="col-md-3 offset-md-3">.col-md-3 .offset-md-3</div>
  <div class="col-md-3 offset-md-3">.col-md-3 .offset-md-3</div>
</div>
<div class="row">
  <div class="col-md-6 offset-md-3">.col-md-6 .offset-md-3</div>
</div>
```

# 文字排版

Bootstrap 4 默认的 font-size 为 16px,  line-height 为 1.5。

默认的 font-family 为 "Helvetica Neue", Helvetica, Arial, sans-serif。

此外，所有的 <p> 元素 margin-top: 0 、 margin-bottom: 1rem (16px)。

```html

<div class="container">
  <h1>h1 Bootstrap 标题 (2.5rem = 40px)</h1>
  <h2>h2 Bootstrap 标题 (2rem = 32px)</h2>
  <h3>h3 Bootstrap 标题 (1.75rem = 28px)</h3>
  <h4>h4 Bootstrap 标题 (1.5rem = 24px)</h4>
  <h5>h5 Bootstrap 标题 (1.25rem = 20px)</h5>
  <h6>h6 Bootstrap 标题 (1rem = 16px)</h6>
</div>


<div class="container">
  <h1>Display 标题</h1>
  <p>Display 标题可以输出更大更粗的字体样式。</p>
  <h1 class="display-1">Display 1</h1>
  <h1 class="display-2">Display 2</h1>
  <h1 class="display-3">Display 3</h1>
  <h1 class="display-4">Display 4</h1>
</div>


<div class="container">
  <h1>更小文本标题</h1>
  <p>small 元素用于字号更小的颜色更浅的文本:</p>       
  <h1>h1 标题 <small>副标题</small></h1>
  <h2>h2 标题 <small>副标题</small></h2>
  <h3>h3 标题 <small>副标题</small></h3>
  <h4>h4 标题 <small>副标题</small></h4>
  <h5>h5 标题 <small>副标题</small></h5>
  <h6>h6 标题 <small>副标题</small></h6>
</div>


<div class="container">
  <h1>高亮文本</h1>    
  <p>使用 mark 元素来 <mark>高亮</mark> 文本。</p>
</div>



<div class="container">
  <h1>Abbreviations</h1>
  <p>The abbr element is used to mark up an abbreviation or acronym:</p>
  <p>The <abbr title="World Health Organization">WHO</abbr> was founded in 1948.</p>
</div>


<div class="container">
  <h1>Blockquotes</h1>
  <p>The blockquote element is used to present content from another source:</p>
  <blockquote class="blockquote">
    <p>For 50 years, WWF has been protecting the future of nature. The world's leading conservation organization, WWF works in 100 countries and is supported by 1.2 million members in the United States and close to 5 million globally.</p>
    <footer class="blockquote-footer">From WWF's website</footer>
  </blockquote>
</div>


<div class="container">
  <h1>Description Lists</h1>    
  <p>The dl element indicates a description list:</p>
  <dl>
    <dt>Coffee</dt>
    <dd>- black hot drink</dd>
    <dt>Milk</dt>
    <dd>- white cold drink</dd>
  </dl>     
</div>


<div class="container">
  <h1>代码片段</h1>
  <p>可以将一些代码元素放到 code 元素里面:</p>
  <p>以下 HTML 元素: <code>span</code>, <code>section</code>, 和 <code>div</code> 用于定义部分文档内容。</p>
</div>


<div class="container">
  <h1>Keyboard Inputs</h1>
  <p>To indicate input that is typically entered via the keyboard, use the kbd element:</p>
  <p>Use <kbd>ctrl + p</kbd> to open the Print dialog box.</p>
</div>


<div class="container">
<h1>Multiple Code Lines</h1>
<p>For multiple lines of code, use the pre element:</p>
<pre>
Text in a pre element
is displayed in a fixed-width
font, and it preserves
both      spaces and
line breaks.
</pre>
</div>

```

| .font-weight-bold   | 加粗文本                                                     | [尝试一下](http://www.runoob.com/try/try.php?filename=trybs4_txt_font-weight) |
| ------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| .font-weight-normal | 普通文本                                                     | [尝试一下](http://www.runoob.com/try/try.php?filename=trybs4_txt_font-weight) |
| .font-weight-light  | 更细的文本                                                   | [尝试一下](http://www.runoob.com/try/try.php?filename=trybs4_txt_font-weight) |
| .font-italic        | 斜体文本                                                     | [尝试一下](http://www.runoob.com/try/try.php?filename=trybs4_txt_font-weight) |
| .lead               | 让段落更突出                                                 | [尝试一下](http://www.runoob.com/try/try.php?filename=trybs4_ref_txt_lea) |
| .small              | 指定更小文本 (为父元素的 85% )                               | [尝试一下](http://www.runoob.com/try/try.php?filename=trybs4_ref_txt_small) |
| .text-left          | 左对齐                                                       | [尝试一下](http://www.runoob.com/try/try.php?filename=trybs4_ref_text-left) |
| .text-center        | 居中                                                         | [尝试一下](http://www.runoob.com/try/try.php?filename=trybs4_ref_text-left) |
| .text-right         | 右对齐                                                       | [尝试一下](http://www.runoob.com/try/try.php?filename=trybs4_ref_text-left) |
| .text-justify       | 设定文本对齐,段落中超出屏幕部分文字自动换行                  | [尝试一下](http://www.runoob.com/try/try.php?filename=trybs4_ref_text-left) |
| .text-nowrap        | 段落中超出屏幕部分不换行                                     | [尝试一下](http://www.runoob.com/try/try.php?filename=trybs4_ref_text-left) |
| .text-lowercase     | 设定文本小写                                                 | [尝试一下](http://www.runoob.com/try/try.php?filename=trybs4_ref_text-lowercase) |
| .text-uppercase     | 设定文本大写                                                 | [尝试一下](http://www.runoob.com/try/try.php?filename=trybs4_ref_text-lowercase) |
| .text-capitalize    | 设定单词首字母大写                                           |                                                              |
| .initialism         | 显示在 <abbr> 元素中的文本以小号字体展示，且可以将小写字母转换为大写字母 |                                                              |
| .list-unstyled      | 移除默认的列表样式，列表项中左对齐 ( <ul> 和 <ol> 中)。 这个类仅适用于直接子列表项     (如果需要移除嵌套的列表项，你需要在嵌套的列表中使用该样式) |                                                              |
| .list-inline        | 将所有列表项放置同一行                                       |                                                              |
| .pre-scrollable     | 使 <pre> 元素可滚动，代码块区域最大高度为340px,一旦超出这个高度,就会在Y轴出现滚动条 |                                                              |

#  颜色 

```html
<!-- text color -->
<div class="container">
  <h2>代表指定意义的文本颜色</h2>
  <p class="text-muted">柔和的文本。</p>
  <p class="text-primary">重要的文本。</p>
  <p class="text-success">执行成功的文本。</p>
  <p class="text-info">代表一些提示信息的文本。</p>
  <p class="text-warning">警告文本。</p>
  <p class="text-danger">危险操作文本。</p>
  <p class="text-secondary">副标题。</p>
  <p class="text-dark">深灰色文字。</p>
  <p class="text-light">浅灰色文本（白色背景上看不清楚）。</p>
  <p class="text-white">白色文本（白色背景上看不清楚）。</p>
</div>

<!-- background color -->

<div class="container">
  <h2>背景颜色</h2>
  <p class="bg-primary text-white">重要的背景颜色。</p>
  <p class="bg-success text-white">执行成功背景颜色。</p>
  <p class="bg-info text-white">信息提示背景颜色。</p>
  <p class="bg-warning text-white">警告背景颜色</p>
  <p class="bg-danger text-white">危险背景颜色。</p>
  <p class="bg-secondary text-white">副标题背景颜色。</p>
  <p class="bg-dark text-white">深灰背景颜色。</p>
  <p class="bg-light text-dark">浅灰背景颜色。</p>
</div>

```

# 表格

```html
<table class="table">
<table class="table table-striped">
<table class="table table-bordered">
<table class="table table-hover">
<table class="table table-dark">
<table class="table table-dark table-striped">
<table class="table table-dark table-hover">   
    

<table class="table">
    <thead>
      <tr>
        <th>Firstname</th>
        <th>Lastname</th>
        <th>Email</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Default</td>
        <td>Defaultson</td>
        <td>def@somemail.com</td>
      </tr>      
      <tr class="table-primary">
        <td>Primary</td>
        <td>Joe</td>
        <td>joe@example.com</td>
      </tr>
      <tr class="table-success">
        <td>Success</td>
        <td>Doe</td>
        <td>john@example.com</td>
      </tr>
      <tr class="table-danger">
        <td>Danger</td>
        <td>Moe</td>
        <td>mary@example.com</td>
      </tr>
      <tr class="table-info">
        <td>Info</td>
        <td>Dooley</td>
        <td>july@example.com</td>
      </tr>
      <tr class="table-warning">
        <td>Warning</td>
        <td>Refs</td>
        <td>bo@example.com</td>
      </tr>
      <tr class="table-active">
        <td>Active</td>
        <td>Activeson</td>
        <td>act@example.com</td>
      </tr>
      <tr class="table-secondary">
        <td>Secondary</td>
        <td>Secondson</td>
        <td>sec@example.com</td>
      </tr>
      <tr class="table-light">
        <td>Light</td>
        <td>Angie</td>
        <td>angie@example.com</td>
      </tr>
      <tr class="table-dark text-dark">
        <td>Dark</td>
        <td>Bo</td>
        <td>bo@example.com</td>
      </tr>
    </tbody>
</table>


 
```

| .table-primary   | 蓝色: 指定这是一个重要的操作     |
| ---------------- | -------------------------------- |
| .table-success   | 绿色: 指定这是一个允许执行的操作 |
| .table-danger    | 红色: 指定这是可以危险的操作     |
| .table-info      | 浅蓝色: 表示内容已变更           |
| .table-warning   | 橘色: 表示需要注意的操作         |
| .table-active    | 灰色: 用于鼠标悬停效果           |
| .table-secondary | 灰色: 表示内容不怎么重要         |
| .table-light     | 浅灰色，可以是表格行的背景       |
| .table-dark      | 深灰色，可以是表格行的背景       |

| .table-responsive-sm | < 576px  |
| -------------------- | -------- |
| .table-responsive-md | < 768px  |
| .table-responsive-lg | < 992px  |
| .table-responsive-xl | < 1200px |

# 表单

## 布局

表单元素 <input>, <textarea>, 和  <select> elements  在使用 .form-control 类的情况下，宽度都是设置为 100%

- 堆叠表单 (全屏宽度)：垂直方向 
- 内联表单：水平方向

```html
<!-- go vertically down -->
<form>
  <div class="form-group">
    <label for="email">Email address:</label>
    <input type="email" class="form-control" id="email">
  </div>
  <div class="form-group">
    <label for="pwd">Password:</label>
    <input type="password" class="form-control" id="pwd">
  </div>
  <div class="form-check">
    <label class="form-check-label">
      <input class="form-check-input" type="checkbox"> Remember me
    </label>
  </div>
  <button type="submit" class="btn btn-primary">Submit</button>
</form>

<!-- go horizontally right -->
<form class="form-inline">
  <label for="email">Email address:</label>
  <input type="email" class="form-control" id="email">
  <label for="pwd">Password:</label>
  <input type="password" class="form-control" id="pwd">
  <div class="form-check">
    <label class="form-check-label">
      <input class="form-check-input" type="checkbox"> Remember me
    </label>
  </div>
  <button type="submit" class="btn btn-primary">Submit</button>
</form>
```

## 控件

Bootstrap4 支持以下表单控件：

- input
- textarea
- checkbox
- radio
- select

```html
<!-- use .form-control for the input -->
<div class="form-group">
  <label for="usr">用户名:</label>
  <input type="text" class="form-control" id="usr">
</div>
<div class="form-group">
  <label for="pwd">密码:</label>
  <input type="password" class="form-control" id="pwd">
</div>
```

```html
<!-- multiple checkbox -->
<!-- 使用 .form-check-inline 类可以让选项显示在同一行 -->
<div class="form-check">
  <label class="form-check-label">
    <input type="checkbox" class="form-check-input" value="">Option 1
  </label>
</div>
<div class="form-check">
  <label class="form-check-label">
    <input type="checkbox" class="form-check-input" value="">Option 2
  </label>
</div>
<div class="form-check disabled">
  <label class="form-check-label">
    <input type="checkbox" class="form-check-input" value="" disabled>Option 3
  </label>
</div>
```

```html
<div class="radio">
  <label><input type="radio" name="optradio">Option 1</label>
</div>
<div class="radio">
  <label><input type="radio" name="optradio">Option 2</label>
</div>
<div class="radio disabled">
  <label><input type="radio" name="optradio" disabled>Option 3</label>
</div>
```

```html
<div class="form-group">
  <label for="sel1">下拉菜单:</label>
  <select class="form-control" id="sel1">
    <option>1</option>
    <option>2</option>
    <option>3</option>
    <option>4</option>
  </select>
</div>
```

## 输入框组

- 使用 .input-group 类来向表单输入框中添加更多的样式，如图标、文本或者按钮
- 使用 .input-group-prepend 类可以在输入框的的前面添加文本信息， .input-group-append 类添加在输入框的后面
- 使用 .input-group-text 类来设置文本的样式
- 使用 .input-group-sm 类来设置小的输入框， .input-group-lg 类设置大的输入框

```html
<form>
  <div class="input-group mb-3">
    <div class="input-group-prepend">
      <span class="input-group-text">@</span>
    </div>
    <input type="text" class="form-control" placeholder="Username">
  </div>
 
  <div class="input-group mb-3">
    <input type="text" class="form-control" placeholder="Your Email">
    <div class="input-group-append">
      <span class="input-group-text">@runoob.com</span>
    </div>
  </div>
</form>
```

# 列表组

要创建列表组，可以在 <ul> 元素上添加 .list-group 类, 在 <li> 元素上添加 .list-group-item 类

```html
<ul class="list-group">
  <li class="list-group-item">First item</li>
  <li class="list-group-item">Second item</li>
  <li class="list-group-item">Third item</li>
</ul>

<!-- link group list -->
<div class="list-group">
  <a href="#" class="list-group-item list-group-item-action">First item</a>
  <a href="#" class="list-group-item list-group-item-action">Second item</a>
  <a href="#" class="list-group-item list-group-item-action">Third item</a>
</div>
```

- 通过添加 .active 类来设置激活状态的列表项
- .disabled 类用于设置禁用的列表项
- 列表项目的颜色可以通过以下列来设置： .list-group-item-success, list-group-item-secondary, list-group-item-info,  list-group-item-warning, .list-group-item-danger, list-group-item-dark 和 list-group-item-light

# 按钮

```html
<button type="button" class="btn">基本按钮</button>
<button type="button" class="btn btn-primary">主要按钮</button>
<button type="button" class="btn btn-secondary">次要按钮</button>
<button type="button" class="btn btn-success">成功</button>
<button type="button" class="btn btn-info">信息</button>
<button type="button" class="btn btn-warning">警告</button>
<button type="button" class="btn btn-danger">危险</button>
<button type="button" class="btn btn-dark">黑色</button>
<button type="button" class="btn btn-light">浅色</button>
<button type="button" class="btn btn-link">链接</button>

<!-- border button -->
<button type="button" class="btn btn-outline-primary">主要按钮</button>
<button type="button" class="btn btn-outline-secondary">次要按钮</button>
<button type="button" class="btn btn-outline-success">成功</button>
<button type="button" class="btn btn-outline-info">信息</button>
<button type="button" class="btn btn-outline-warning">警告</button>
<button type="button" class="btn btn-outline-danger">危险</button>
<button type="button" class="btn btn-outline-dark">黑色</button>
<button type="button" class="btn btn-outline-light text-dark">浅色</button>


<!-- size -->
<button type="button" class="btn btn-primary btn-lg">大号按钮</button>
<button type="button" class="btn btn-primary">默认按钮</button>
<button type="button" class="btn btn-primary btn-sm">小号按钮</button>
```

**Button Group**

```html
<div class="btn-group">
  <button type="button" class="btn btn-primary">Apple</button>
  <button type="button" class="btn btn-primary">Samsung</button>
  <button type="button" class="btn btn-primary">Sony</button>
</div>

使用 .btn-group-lg|sm|xs 类来设置按钮组的大小
使用 .btn-group-vertical 类来创建垂直的按钮组

```



# 多媒体 

## 图片

```html

<img src="cinqueterre.jpg" class="rounded" alt="Cinque Terre">

<img src="cinqueterre.jpg" class="rounded-circle" alt="Cinque Terre">

<img src="cinqueterre.jpg" class="img-thumbnail" alt="Cinque Terre">

<!-- picture align -->
<img src="paris.jpg" class="float-left"> 
<img src="cinqueterre.jpg" class="float-right">

<!-- responsive picture -->
<img class="img-fluid" src="img_chania.jpg" alt="Chania">

```

##  多媒体对象

要创建一个多媒体对象，可以在容器元素上添加 .media 类，然后将多媒体内容放到子容器上，子容器需要添加 .media-body  类，然后添加外边距，内边距等效果

```html
<div class="media border p-3">
  <img src="mobile-icon.png" alt="John Doe" class="mr-3 mt-3 rounded-circle" style="width:60px;">
  <div class="media-body">
    <h4>菜鸟教程</h4>
    <p>学的不仅是技术，更是梦想！！！</p>
  </div>
</div>

<!-- 将头像图片显示在右侧，可以在 .media-body 容器后添加图片 -->
```
# 网页模块

## Jumbotron 

```html
<div class="jumbotron">
  <h1>菜鸟教程</h1> 
  <p>学的不仅是技术，更是梦想！！！</p> 
</div>


<div class="jumbotron jumbotron-fluid">
  <div class="container">
      <h1>菜鸟教程</h1> 
      <p>学的不仅是技术，更是梦想！！！</p>
  </div>
</div>

```

## 徽章（Badges）

将 .badge 类加上带有指定意义的颜色类 (如 .badge-secondary) 添加到 <span>  元素上即可。 徽章可以根据父元素的大小的变化而变化

```html
<span class="badge badge-primary">主要</span>
<span class="badge badge-secondary">次要</span>
<span class="badge badge-success">成功</span>
<span class="badge badge-danger">危险</span>
<span class="badge badge-warning">警告</span>
<span class="badge badge-info">信息</span>
<span class="badge badge-light">浅色</span>
<span class="badge badge-dark">深色</span>

使用 .badge-pill 类来设置药丸形状徽章:
```

## 进度条

创建一个基本的进度条的步骤如下：

- 添加一个带有 **.progress** 类的 <div>。
- 接着，在上面的 <div> 内，添加一个带有 class **.progress-bar** 的空的 <div>。
- 添加一个带有百分比表示的宽度的 style 属性，例如 style="width:70%" 表示进度条在 **70%** 的位置。

```html
<div class="progress">
  <div class="progress-bar" style="width:70%">70%</div>
</div>

<!-- different height -->
<div class="progress" style="height:20px;">
  <div class="progress-bar" style="width:40%;"></div>
</div>

<!-- different color -->
<div class="progress">
  <div class="progress-bar bg-success" style="width:40%"></div>
</div>
 
<div class="progress">
  <div class="progress-bar bg-info" style="width:50%"></div>
</div>
 
<div class="progress">
  <div class="progress-bar bg-warning" style="width:60%"></div>
</div>
 
<div class="progress">
  <div class="progress-bar bg-danger" style="width:70%"></div>
```

可以使用 `.progress-bar-striped` 类来设置条纹进度条：

使用 `.progress-bar-animated` 类可以为进度条添加动画

## 分页

要创建一个基本的分页可以在 <ul> 元素上添加  .pagination 类。然后在  <li> 元素上添加 .page-item 类

.breadcrumb 和 .breadcrumb-item  类用于设置面包屑导航

```html
<!-- normal -->
<ul class="pagination">
  <li class="page-item"><a class="page-link" href="#">Previous</a></li>
  <li class="page-item"><a class="page-link" href="#">1</a></li>
  <li class="page-item"><a class="page-link" href="#">2</a></li>
  <li class="page-item"><a class="page-link" href="#">3</a></li>
  <li class="page-item"><a class="page-link" href="#">Next</a></li>
</ul>

<!-- breadcrumb -->
<ul class="breadcrumb">
  <li class="breadcrumb-item"><a href="#">Photos</a></li>
  <li class="breadcrumb-item"><a href="#">Summer 2017</a></li>
  <li class="breadcrumb-item"><a href="#">Italy</a></li>
  <li class="breadcrumb-item active">Rome</li>
</ul>
```

- 当前页可以使用 .active 类来高亮显示
- .disabled 类可以设置分页链接不可点击
- .pagination-lg 类设置大字体的分页条目，.pagination-sm 类设置小字体的分页条目

## 卡片

```html
<div class="card">
  <div class="card-header">头部</div>
  <div class="card-body">内容</div> 
  <div class="card-footer">底部</div>
</div>


<div class="card">
  <div class="card-body">
    <h4 class="card-title">Card title</h4>
    <p class="card-text">Some example text. Some example text.</p>
    <a href="#" class="card-link">Card link</a>
    <a href="#" class="card-link">Another link</a>
  </div>
</div>
```

- Bootstrap 4 提供了多种卡片的背景颜色类： .bg-primary,  .bg-success, .bg-info, .bg-warning, .bg-danger, .bg-secondary, .bg-dark 和 .bg-light
- 给  <img> 添加 .card-img-top（图片在文字上方） 或 .card-img-bottom（图片在文字下方 来设置图片卡片

```html
<div class="card" style="width:400px">
  <img class="card-img-top" src="img_avatar1.png" alt="Card image">
  <div class="card-body">
    <h4 class="card-title">John Doe</h4>
    <p class="card-text">Some example text.</p>
    <a href="#" class="btn btn-primary">See Profile</a>
  </div>
</div>

<div class="card" style="width:500px">
  <img class="card-img-top" src="img_avatar1.png" alt="Card image">
  <div class="card-img-overlay">
    <h4 class="card-title">John Doe</h4>
    <p class="card-text">Some example text.</p>
    <a href="#" class="btn btn-primary">See Profile</a>
  </div>
</div>
```

## 下拉菜单

```html

<div class="dropdown">
  <button type="button" class="btn btn-primary dropdown-toggle" data-toggle="dropdown">
    Dropdown button
  </button>
  <div class="dropdown-menu">
    <a class="dropdown-item" href="#">Link 1</a>
    <a class="dropdown-item" href="#">Link 2</a>
    <a class="dropdown-item" href="#">Link 3</a>
  </div>
</div>
```

- .dropdown-divider 类用于在下拉菜单中创建一个水平的分割线
- .dropdown-header 类用于在下拉菜单中添加标题：
- 让下拉菜单右对齐，可以在元素上的 .dropdown-menu 类后添加 .dropdown-menu-right 类
- 下拉菜单向上弹出，可以将 <div> 元素的 class="dropdown" 替换为 "dropup"

## 折叠

```html
<button data-toggle="collapse" data-target="#demo">折叠</button>
 
<div id="demo" class="collapse">
Lorem ipsum dolor text....
</div>
```

## 导航

在 <ul> 元素上添加  .nav类，在每个 <li> 选项上添加 .nav-item 类，在每个链接上添加 .nav-link 类:

```html
<ul class="nav">
  <li class="nav-item">
    <a class="nav-link" href="#">Link</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" href="#">Link</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" href="#">Link</a>
  </li>
  <li class="nav-item">
    <a class="nav-link disabled" href="#">Disabled</a>
  </li>
</ul>

<!-- dynamic -->
<!-- 在每个链接上添加 data-toggle="tab" 属性。 然后在每个选项对应的内容的上添加 .tab-pane 类。-->
<!-- 如果你希望有淡入效果可以在 .tab-pane 后添加 .fade类 -->
<!-- Nav tabs -->
<ul class="nav nav-tabs">
  <li class="nav-item">
    <a class="nav-link active" data-toggle="tab" href="#home">Home</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" data-toggle="tab" href="#menu1">Menu 1</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" data-toggle="tab" href="#menu2">Menu 2</a>
  </li>
</ul>
 
<!-- Tab panes -->
<div class="tab-content">
  <div class="tab-pane active container" id="home">...</div>
  <div class="tab-pane container" id="menu1">...</div>
  <div class="tab-pane container" id="menu2">...</div>
</div>

```

- .justify-content-center 类设置导航居中显示， .justify-content-end 类设置导航右对齐
- .flex-column 类用于创建垂直导航
- 使用 .nav-tabs 类可以将导航转化为选项卡。然后对于选中的选项使用 .active 类来标记
- .nav-pills 类可以将导航项设置成胶囊形状
- .nav-justified 类可以设置导航项齐行等宽显示

## 导航栏

使用 .navbar 类来创建一个标准的导航栏，后面紧跟: .navbar-expand-xl|lg|md|sm 类来创建响应式的导航栏 (大屏幕水平铺开，小屏幕垂直堆叠)。 

导航栏上的选项可以使用 <ul> 元素并添加 class="navbar-nav" 类。 然后在 <li> 元素上添加 .nav-item 类， <a> 元素上使用 .nav-link 类

```html

<!-- 小屏幕上水平导航栏会切换为垂直的 -->
<nav class="navbar navbar-expand-sm bg-light">
 
  <!-- Links -->
  <ul class="navbar-nav">
    <li class="nav-item">
      <a class="nav-link" href="#">Link 1</a>
    </li>
    <li class="nav-item">
      <a class="nav-link" href="#">Link 2</a>
    </li>
    <li class="nav-item">
      <a class="nav-link" href="#">Link 3</a>
    </li>
  </ul>
 
</nav>

```

- 删除 .navbar-expand-xl|lg|md|sm 类来创建垂直导航栏
- 使用以下类来创建不同颜色导航栏：.bg-primary,  .bg-success, .bg-info, .bg-warning, .bg-danger, .bg-secondary, .bg-dark 和 .bg-light
- .navbar-brand 类用于高亮显示品牌/Logo
- 使用 .navbar-text 类来设置导航栏上非链接文本，可以保证水平对齐，颜色与内边距一样
- 使用 .fixed-top 类来实现导航栏的固定



通常，小屏幕上我们都会折叠导航栏，通过点击来显示导航选项。

要创建折叠导航栏，可以在按钮上添加 class="navbar-toggle",  data-toggle="collapse" 与 data-target="#*thetarget*" 类。然后在设置了  class="collapse navbar-collapse" 类的 div 上包裹导航内容（链接）, div 元素上的 id 匹配按钮 data-target 的上指定的 id

```html
<nav class="navbar navbar-expand-md bg-dark navbar-dark">
  <!-- Brand -->
  <a class="navbar-brand" href="#">Navbar</a>
 
  <!-- Toggler/collapsibe Button -->
  <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#collapsibleNavbar">
    <span class="navbar-toggler-icon"></span>
  </button>
 
  <!-- Navbar links -->
  <div class="collapse navbar-collapse" id="collapsibleNavbar">
    <ul class="navbar-nav">
      <li class="nav-item">
        <a class="nav-link" href="#">Link</a>
      </li>
      <li class="nav-item">
        <a class="nav-link" href="#">Link</a>
      </li>
      <li class="nav-item">
        <a class="nav-link" href="#">Link</a>
      </li> 
    </ul>
  </div> 
</nav>
```

## 轮播

```html
<div id="demo" class="carousel slide" data-ride="carousel">
 
  <!-- 指示符 -->
  <ul class="carousel-indicators">
    <li data-target="#demo" data-slide-to="0" class="active"></li>
    <li data-target="#demo" data-slide-to="1"></li>
    <li data-target="#demo" data-slide-to="2"></li>
  </ul>
 
  <!-- 轮播图片 -->
  <div class="carousel-inner">
    <div class="carousel-item active">
      <img src="https://static.runoob.com/images/mix/img_fjords_wide.jpg">
    </div>
    <div class="carousel-item">
      <img src="https://static.runoob.com/images/mix/img_nature_wide.jpg">
    </div>
    <div class="carousel-item">
      <img src="https://static.runoob.com/images/mix/img_mountains_wide.jpg">
    </div>
  </div>
 
  <!-- 左右切换按钮 -->
  <a class="carousel-control-prev" href="#demo" data-slide="prev">
    <span class="carousel-control-prev-icon"></span>
  </a>
  <a class="carousel-control-next" href="#demo" data-slide="next">
    <span class="carousel-control-next-icon"></span>
  </a>
 
</div>

<!-- 在 <div class="carousel-item"> 内添加 <div class="carousel-caption"> 来设置轮播图片的描述 -->
<div class="carousel-item">
  <img src="https://static.runoob.com/images/mix/img_fjords_wide.jpg">
  <div class="carousel-caption">
    <h3>第一张图片描述标题</h3>
    <p>描述文字!</p>
  </div>
</div>

```

| `.carousel`                   | 创建一个轮播                                                 |
| ----------------------------- | ------------------------------------------------------------ |
| `.carousel-indicators`        | 为轮播添加一个指示符，就是轮播图底下的一个个小点，轮播的过程可以显示目前是第几张图。 |
| `.carousel-inner`             | 添加要切换的图片                                             |
| `.carousel-item`              | 指定每个图片的内容                                           |
| `.carousel-control-prev`      | 添加左侧的按钮，点击会返回上一张。                           |
| `.carousel-control-next`      | 添加右侧按钮，点击会切换到下一张。                           |
| `.carousel-control-prev-icon` | 与 .carousel-control-prev 一起使用，设置左侧的按钮           |
| `.carousel-control-next-icon` | 与 .carousel-control-next 一起使用，设置右侧的按钮           |
| `.slide`                      | 切换图片的过渡和动画效果，如果你不需要这样的效果，可以删除这个类。 |

# 框

## 信息提示框

提示框可以使用 .alert 类, 后面加上 .alert-success, .alert-info, .alert-warning,  .alert-danger, .alert-primary, .alert-secondary, .alert-light 或 .alert-dark 类来实现:

```html
<div class="alert alert-success">
 
 <!-- link match the alert color -->
<div class="alert alert-success">
  <strong>成功!</strong> 你应该认真阅读 <a href="#" class="alert-link">这条信息</a>。
</div>

<!-- bind 'class' and 'data-dismiss‘ to button for closing the alert -->
<div class="alert alert-success alert-dismissable">
  <button type="button" class="close" data-dismiss="alert">&times;</button>
  <strong>成功!</strong> 指定操作成功提示信息。
</div>
    
<!-- animation -->
<div class="alert alert-danger alert-dismissable fade show">
```

## 模态框

模态框（Modal）是覆盖在父窗体上的子窗体。通常，目的是显示来自一个单独的源的内容，可以在不离开父窗体的情况下有一些互动。子窗体可提供信息交互等

- 添加 .modal-sm  类来创建一个小模态框，.modal-lg 类可以创建一个大模态框。
- 尺寸类放在 <div>元素的 .modal-dialog 类后

```html
<!-- 按钮：用于打开模态框 -->
<button type="button" class="btn btn-primary" data-toggle="modal" data-target="#myModal">
  打开模态框
</button>
 
<!-- 模态框 -->
<div class="modal fade" id="myModal">
  <div class="modal-dialog">
    <div class="modal-content">
 
      <!-- 模态框头部 -->
      <div class="modal-header">
        <h4 class="modal-title">模态框头部</h4>
        <button type="button" class="close" data-dismiss="modal">&times;</button>
      </div>
 
      <!-- 模态框主体 -->
      <div class="modal-body">
        模态框内容..
      </div>
 
      <!-- 模态框底部 -->
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-dismiss="modal">关闭</button>
      </div>
 
    </div>
  </div>
</div>
```

## 提示框

提示框是一个小小的弹窗，在鼠标移动到元素上显示，鼠标移到元素外就消失

通过向元素添加 data-toggle="tooltip"  来来创建提示框。 title 属性的内容为提示框显示的内容

```html
<a href="#" data-toggle="tooltip" title="我是提示内容!">鼠标移动到我这</a>
```

- 使用 data-placement 属性来设定提示框显示的方向: top, bottom, left 或 right

## 弹出框

弹出框控件类似于提示框，它在鼠标点击到元素后显示，与提示框不同的是它可以显示更多的内容

通过向元素添加 data-toggle="popover"  来来创建弹出框。 title 属性的内容为弹出框的标题，data-content 属性显示了弹出框的文本内容

```html
<a href="#" data-toggle="popover" title="弹出框标题" data-content="弹出框内容">多次点我</a>
```

- 使用 data-placement 属性来设定弹出框显示的方向: top, bottom, left 或 right
- 使用 data-trigger="focus" 属性来设置在鼠标点击元素外部区域来关闭弹出框
- 使用 data-trigger 属性，并设置值为 "hover"实现在鼠标移动到元素上显示，移除后消失的效果

# 滚动监听(Scrollspy)

滚动监听（Scrollspy）插件，即自动更新导航插件，会根据滚动条的位置自动更新对应的导航目标。其基本的实现是随着您的滚动

向想要监听的元素（通常是 body）添加 data-spy="scroll" 。

然后添加 data-target 属性，它的值为导航栏的 id 或 class (.navbar)。这样就可以联系上可滚动区域。

注意可滚动项元素上的 id  （<div id="section1">） 必须匹配导航栏上的链接选项 （<a href="#section1">)。

可选项data-offset 属性用于计算滚动位置时，距离顶部的偏移像素。 默认为 10 px。

**设置相对定位:** 使用 data-spy="scroll"  的元素需要将其 CSS **position** 属性设置为 "relative" 才能起作用。

```html
<!-- 可滚动区域 -->
<body data-spy="scroll" data-target=".navbar" data-offset="50">
 
<!-- The navbar - The <a> elements are used to jump to a section in the scrollable area -->
<nav class="navbar navbar-expand-sm bg-dark navbar-dark fixed-top">
...
  <ul class="navbar-nav">
    <li><a href="#section1">Section 1</a></li>
    ...
</nav>
 
<!-- 第一部分内容 -->
<div id="section1">
  <h1>Section 1</h1>
  <p>Try to scroll this page and look at the navigation bar while scrolling!</p>
</div>
...
 
</body>
```

# 小工具

## 边框

使用 border 类可以添加或移除边框:

```html
<span class="border"></span>
<span class="border border-0"></span>
<span class="border border-top-0"></span>
<span class="border border-right-0"></span>
<span class="border border-bottom-0"></span>
<span class="border border-left-0"></span>
```


## 边框颜色

Bootstrap4 提供了一些类来设置边框颜色:
```html
<span class="border border-primary"></span>
<span class="border border-secondary"></span> 
<span class="border border-success"></span> 
<span class="border border-danger"></span> 
<span class="border border-warning"></span> 
<span class="border border-info"></span> 
<span class="border border-light"></span> 
<span class="border border-dark"></span> 
<span class="border border-white"></span>
```

## 边框圆角设置

使用rounded 类可以添加圆角边框:
```html
<span class="rounded"></span> 
<span class="rounded-top"></span>
<span class="rounded-right"></span>
<span class="rounded-bottom"></span>
<span class="rounded-left"></span> 
<span class="rounded-circle"></span>
<span class="rounded-0"></span>
```
## 浮动

.float-right 类用于设置元素右浮动， .float-left 设置元素左浮动, .clearfix 类用于清除浮动:
```html
<div class="clearfix">   
    <span class="float-left">左浮动</span>   
    <span class="float-right">右浮动</span> 
</div>
```
## 响应式浮动
我们看可以设置浮动 (.float-*-left|right -  * 为 sm, md, lg 或 xl)的方向依赖于屏幕的大小:
```html
<div class="float-sm-right">在大于小屏幕尺寸上右浮动</div>
<br> 
div class="float-md-right">在大于中等屏幕尺寸上右浮动</div>
<br> 
<div class="float-lg-right">在大于大屏幕尺寸上右浮动</div>
<br> 
<div class="float-xl-right">在大于超大屏幕尺寸上右浮动</div>
<br> 
<div class="float-none">没有浮动</div>
```
## 居中对齐
使用 .mx-auto 类来设置居中对齐:
```html
<div class="mx-auto bg-warning" style="width:150px">居中显示</div>
```
## 宽度
元素上使用 w-* 类 (.w-25, .w-50, .w-75, .w-100, .mw-100) 来设置宽度:
```html
<div class="w-25 bg-warning">宽度 25%</div> 
<div class="w-50 bg-warning">宽度 50%</div> 
<div class="w-75 bg-warning">宽度 75%</div> 
<div class="w-100 bg-warning">宽度 100%</div> 
<div class="mw-100 bg-warning">最大宽度 100%</div>
```
## 高度
元素上使用 h-* 类 (.h-25, .h-50, .h-75, .h-100, .mh-100) 来设置高度:
```html
<div style="height:200px;background-color:#ddd"> 
    <div class="h-25 bg-warning">高度 25%</div> 
    <div class="h-50 bg-warning">高度 50%</div> 
    <div class="h-75 bg-warning">高度 75%</div> 
    <div class="h-100 bg-warning">高度 100%</div> 
    <div class="mh-100 bg-warning" style="height:500px">最大高度 100%</div>
</div>
```