# Layout

## Grid

Create a fully responsive, fluid and nestable grid layout.

The Grid system of UIkit allows you to arrange block elements in columns. It works closely together with the [Width component](https://getuikit.com/docs/width) to determine how much space of the container each item will take up, and it can also be combined with the [Flex component](https://getuikit.com/docs/flex) to align and order grid items.

```html
<div uk-grid>
    <div></div>
    <div></div>
</div>
```

### Gutter Modifiers

## 

The  Grid component comes with a default gutter that is decreased  automatically from a certain breakpoint usually on a smaller desktop  viewport width. To apply a different gutter, add one of the following  classes.

| Class               | Description                                                  |
| ------------------- | ------------------------------------------------------------ |
| `.uk-grid-small`    | Add this class to apply a small gutter.                      |
| `.uk-grid-medium`   | Add this class to apply a medium gutter like the default one, but without a breakpoint. |
| `.uk-grid-large`    | Add this class to apply a large gutter with breakpoints.     |
| `.uk-grid-collapse` | Add this class to remove the grid gutter entirely.           |

Add the `.uk-grid-divider` class to separate grid cells  with lines. This class can be combined with the gutter modifiers. As  soon as the grid stacks, the divider lines will become horizontal.

```html
<div class="uk-grid-divider" uk-grid>...</div>
```

###[Component options](https://getuikit.com/docs/grid#component-options)

Any of these options can be applied to the component attribute. Separate multiple options with a semicolon. [Learn more](https://getuikit.com/docs/javascript#component-configuration)

| Option         | Value   | Default         | Description                                                  |
| -------------- | ------- | --------------- | ------------------------------------------------------------ |
| `margin`       | String  | uk-grid-margin  | This class is added to items that break into the next row, typically to create margin to the previous row. |
| `first-column` | String  | uk-first-column | This class is added to the first element in each row.        |
| `masonry`      | Boolean | false           | Enables masonry layout for this grid.                        |
| `parallax`     | Number  | 0               | Parallax translation value. The value must be a positive integer. Falsy disables the parallax effect (default). |

## Width

`.uk-width-*` 

`.uk-child-width-*`

`.uk-width-auto`

`.uk-width-expand`

###[Responsive width](https://getuikit.com/docs/width#responsive-width)

UIkit  provides a number of responsive widths classes. Basically they work  just like the usual width classes, except that they have suffixes that  represent the breakpoint from which they come to effect. These classes  can be combined with the [Visibility component](https://getuikit.com/docs/visibility). This is great to adjust your layout and content for different device sizes.

| Class                                    | Description                                                  |
| ---------------------------------------- | ------------------------------------------------------------ |
| `.uk-width-*`  `.uk-child-width-*`       | Affects all device widths, grid columns stay side by side.   |
| `.uk-width-*@s`  `.uk-child-width-*@s`   | Affects device widths of *640px* and larger. Grid columns will stack on smaller sizes. |
| `.uk-width-*@m`  `.uk-child-width-*@m`   | Affects device widths of *960px* and larger. Grid columns will stack on smaller sizes. |
| `.uk-width-*@l`  `.uk-child-width-*@l`   | Affects device widths of *1200px* and larger. Grid columns will stack on smaller sizes. |
| `.uk-width-*@xl`  `.uk-child-width-*@xl` | Affects device widths of *1600px* and larger. Grid columns will stack on smaller sizes. |

## Flex

| `.uk-flex`        | Create the flex container and behave like a block element.   |
| ----------------- | ------------------------------------------------------------ |
| `.uk-flex-inline` | Create the flex container and behave like an inline element. |

| `.uk-flex-left`    | Add this class to align flex items to the left.              |
| ------------------ | ------------------------------------------------------------ |
| `.uk-flex-center`  | Add this class to center flex items along the main axis.     |
| `.uk-flex-right`   | Add this class to align flex items to the right.             |
| `.uk-flex-between` | Add this class to distribute items evenly, with equal space between the items along the main axis. |
| `.uk-flex-around`  | Add this class to distribute items evenly with equal space on both sides of each item. |

| `.uk-flex-stretch` | Add this class to expand flex items to fill the height of their parent. |
| ------------------ | ------------------------------------------------------------ |
| `.uk-flex-top`     | Add this class to align flex items to the top.               |
| `.uk-flex-middle`  | Add this class to center flex items along the cross axis.    |
| `.uk-flex-bottom`  | Add this class to align flex items to the bottom.            |

| `.uk-flex-row`            | Add this class to lay out flex items as horizontal rows.  |
| ------------------------- | --------------------------------------------------------- |
| `.uk-flex-row-reverse`    | Add this class to lay out flex items from right to left.  |
| `.uk-flex-column`         | Add this class to lay out flex items as vertical columns. |
| `.uk-flex-column-reverse` | Add this class to lay out flex items from bottom to top.  |

| `.uk-flex-wrap`         | Add this class to make flex items wrap into another line when they no longer fit their container. |
| ----------------------- | ------------------------------------------------------------ |
| `.uk-flex-wrap-reverse` | Add this class to change the items' direction so that they run from right to left. |
| `.uk-flex-nowrap`       | Add this class to force the flex items into one line. This is the default behavior. |

| `.uk-flex-none` | The box's size is determined by its content.           |
| --------------- | ------------------------------------------------------ |
| `.uk-flex-auto` | The space is allocated considering the item's content. |
| `.uk-flex-1`    | The space is allocated solely based on flex.           |

## [Cover](https://getuikit.com/docs/cover#cover)

Expand images, videos or iframes to cover their entire container and place your own content on top.

[Usage](https://getuikit.com/docs/cover#usage)

To have an image cover its parent element, add the `.uk-cover-container` class to the parent and the `uk-cover` attribute to the image.

```html
<div class="uk-cover-container">
    <img src="" alt="" uk-cover>
</div>
```

## [Article](https://getuikit.com/docs/article#article)

Create articles within your page.

[Usage](https://getuikit.com/docs/article#usage)

The article component consists of the article itself, a title and meta data.

| Class               | Description                                                  |
| ------------------- | ------------------------------------------------------------ |
| `.uk-article`       | Add this class to define the Article component. Typically you would use an `<article>` element for this. |
| `.uk-article-title` | Add this class to a heading to create an article title. Typically you would use a `<h1>` element for this. |
| `.uk-article-meta`  | Add this class to a paragraph which contains meta data about your article. |