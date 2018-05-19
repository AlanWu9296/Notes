# Styling Boxes

## Size Constraints

[`min-width`](https://developer.mozilla.org/en-US/docs/Web/CSS/min-width), [`max-width`](https://developer.mozilla.org/en-US/docs/Web/CSS/max-width), [`min-height`](https://developer.mozilla.org/en-US/docs/Web/CSS/min-height), and [`max-height`](https://developer.mozilla.org/en-US/docs/Web/CSS/max-height).

## Background

- [`background-color`](https://developer.mozilla.org/en-US/docs/Web/CSS/background-color): Sets a solid color for the background.

- [`background-image`](https://developer.mozilla.org/en-US/docs/Web/CSS/background-image): Specifies a background image to appear in the background of the element. This can be a static file, or a generated gradient.

  - ```css
    background-image: linear-gradient(to bottom, yellow, orange 40%, yellow);
    ```

- [`background-position`](https://developer.mozilla.org/en-US/docs/Web/CSS/background-position): Specifies the position that the background should appear inside the element background.

  - Absolute values like pixels — for example `background-position: 200px 25px`.
  - Relative values like rems — for example `background-position: 20rem 2.5rem`.
  - Percentages — for example `background-position: 90% 25%`.
  - Keywords — for example `background-position: right center`. These two values are intuitive, and can take values of `left`, `center`, `right`, and `top`, `center`, `bottom`, respectively.

- [`background-repeat`](https://developer.mozilla.org/en-US/docs/Web/CSS/background-repeat): Specifies whether the background should be repeated (tiled) or not.

  - `no-repeat`: The image will not repeat at all: it will only be shown once.
  - `repeat-x`: The image will repeat horizontally all the way across the background.
  - `repeat-y`: The image will repeat vertically all the way down the background.
  - `repeat`: The image will repeat both vertically and horizontally.

- [`background-attachment`](https://developer.mozilla.org/en-US/docs/Web/CSS/background-attachment):  Specifies the behaviour of an element's background when its content  scrolls, e.g. does it scroll with the content, or is it fixed?

  - `scroll`: Causes the element's background to scroll when  the page is scrolled. If the element content is scrolled, the background  does not move. In effect, the background is fixed to the same position  on the page, so it scrolls as the page scrolls.

    `fixed`: Causes an element's background to be fixed to  the viewport, so that it doesn't scroll when the page or element content  is scrolled. It will always remain in the same position on the screen.

    `local`: This value was added later on (it is only  supported in Internet Explorer 9+, whereas the others are supported in  IE4+) because the `scroll` value is rather confusing and doesn't really do what you want in many cases. The `local` value fixes the background to the element it is set on, so when you scroll the element, the 

- [`background`](https://developer.mozilla.org/en-US/docs/Web/CSS/background): Shorthand for specifying the above five properties on one line.

- [`background-size`](https://developer.mozilla.org/en-US/docs/Web/CSS/background-size): Allows a background image to be resized dynamically

##  Advanced box effects

### Box Shadows

1. `box-shadow` property value:
   1. The first length value is the **horizontal offset** — the distance to the right the shadow is offset from the original box (or left, if the value is negative).
   2. The second length value is the **vertical offset** — the distance downwards that the shadow is offset from the original box (or upwards, if the value is negative).
   3. The third length value is the **blur radius** — the amount of blurring applied to the shadow.
   4. The color value is the **base color** of the shadow.
2. box shadow can be multiple
3. box-shadow has a keyword `inset` to make inner shadow rather than outer shadow

### Filters

`filter: <func1> <func2>`

1. blur()
2. brightness()
3. contrast()
4. drop-shadow() *it apply to all contents while box-shadow only affects the container*
5. grayscale()
6. hue-rotate()
7. invert()
8. opacity()
9. saturate()
10. sepia()

### Blend Mode

- [`background-blend-mode`](https://developer.mozilla.org/en-US/docs/Web/CSS/background-blend-mode), which blends together multiple background images and colors set on a single element.
- [`mix-blend-mode`](https://developer.mozilla.org/en-US/docs/Web/CSS/mix-blend-mode), which blends together the element it is set on with elements it is overlapping — both background and content.

```css
<blend-mode> = normal | multiply | screen | overlay | darken | lighten | color-dodge | color-burn | hard-light | soft-light | difference | exclusion | hue | saturation | color | luminosity
```

# Transition

CSS transitions let you decide which properties to animate (by *listing them explicitly*), when the animation will start (by setting a *delay)*, how long the transition will last (by setting a *duration*), and how the transition will run (by defining a *timing function*, e.g. linearly or quick at the beginning, slow at the end).

## Defining transitions

```css
div {
    transition: <property> <duration> <timing-function> <delay>;
}
```

1. [`transition-property`](https://developer.mozilla.org/en-US/docs/Web/CSS/transition-property)Specifies the name or names of the CSS properties to which  transitions should be applied. Only properties listed here are animated  during transitions; changes to all other properties occur  instantaneously as usual.
2. [`transition-duration`](https://developer.mozilla.org/en-US/docs/Web/CSS/transition-duration)Specifies the duration over which transitions should occur. You can  specify a single duration that applies to all properties during the  transition, or multiple values to allow each property to transition over  a different period of time.
3. [`transition-timing-function`](https://developer.mozilla.org/en-US/docs/Web/CSS/transition-timing-function) ease, linear, step-end, steps(<num>,end)
4. [`transition-delay`](https://developer.mozilla.org/en-US/docs/Web/CSS/transition-delay)Defines how long to wait between the time a property is changed and the transition actually begins.  

# Animation

## Configuring the animation

The sub-properties of the [`animation`](https://developer.mozilla.org/en-US/docs/Web/CSS/animation) property are:

- [`animation-delay`](https://developer.mozilla.org/en-US/docs/Web/CSS/animation-delay)

  Configures the delay between the time the element is loaded and the beginning of the animation sequence.

- [`animation-direction`](https://developer.mozilla.org/en-US/docs/Web/CSS/animation-direction)

  Configures whether or not the animation should alternate direction  on each run through the sequence or reset to the start point and repeat  itself.

- [`animation-duration`](https://developer.mozilla.org/en-US/docs/Web/CSS/animation-duration)

  Configures the length of time that an animation should take to complete one cycle.

- [`animation-iteration-count`](https://developer.mozilla.org/en-US/docs/Web/CSS/animation-iteration-count)

  Configures the number of times the animation should repeat; you can specify `infinite` to repeat the animation indefinitely.

- [`animation-name`](https://developer.mozilla.org/en-US/docs/Web/CSS/animation-name)

  Specifies the name of the [`@keyframes`](https://developer.mozilla.org/en-US/docs/Web/CSS/@keyframes) at-rule describing the animation’s keyframes.

- [`animation-play-state`](https://developer.mozilla.org/en-US/docs/Web/CSS/animation-play-state)

  Lets you pause and resume the animation sequence.

- [`animation-timing-function`](https://developer.mozilla.org/en-US/docs/Web/CSS/animation-timing-function)

  Configures the timing of the animation; that is, how the animation  transitions through keyframes, by establishing acceleration curves.

- [`animation-fill-mode`](https://developer.mozilla.org/en-US/docs/Web/CSS/animation-fill-mode)

  Configures what values are applied by the animation before and after it is executing.

## Defining the animation

```css
@keyframes <animation name>{
    start{}
    50%{}
    to{}
}
```

Since the timing of the animation is defined in the CSS style that configures the animation, keyframes use a [``](https://developer.mozilla.org/en-US/docs/Web/CSS/percentage) to indicate the time during the animation sequence at which they take  place. 0% indicates the first moment of the animation sequence, while  100% indicates the final state of the animation. Because these two times are so important, they have special aliases: `from` and `to`. Both are optional. If `from`/`0%` or `to`/`100%` is not specified, the browser starts or finishes the animation using the computed values of all attributes.