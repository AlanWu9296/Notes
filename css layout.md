# Flexbox Layout

## Flexbox Container & Flexbox Item

1. `container{display:flex;}`

## Main Axis & Cross Axis

1. main axis is default horizontal and the cross axis is vertical which can be changed
2. `flex-direction:row / column`(default row)

## Justify Content

1. along the main axis
2. `justify-content:` flex-start(default), flex-end, center, space-around, space-between, space-evenly

## Positioning Items

1. along the main axis
2. for specific item
3. `mergin-left:auto` / `mergin-right:auto`

## Flex Property(responsive)

1. for flex item
2. `flex:<num>` 1 which indicates how important of the item is which will take how many space
3. `flex-grow:<>`  1 which indicates how the extra space of the container should be distributed among items. the larger number, the more the space of the item will grow
4. `flex-shrink`1 which indicates how the space will shrink when the container doesn't have enough space for the items, the larger number the more the item will shrink. 0 means the item won't shrink
5. `flex-basis`0 which indicates the basis width of the item (at least they should be the width if the container has enough space for them)
6. `flex: 1 1 0`is the default

## Align Items

1. along the cross axis
2. `align-items` stretch(default), flex-start, flex-end, center
3. for flex item, `align-self`

## Wrap 

1. `flex-wrap` no-wrap(default), wrap

## Order of Items

1. for flex item along the main axis
2. `order:` 0(default), the smaller the earlier, can be negative



# Grid Layout

## Container & Item

.container{}

`display:grid;`

`grid-template-columns: 100px auto 100px`

`grid-template-rows: 50px 50px` 

`grid-template: <vertical> / <horizontal>`

`grid-gap: 3px`

## Franction Units and Repeat

1. <num>fr
2. `repeat(<number>,<fr>)` eg. repeat(3, 1fr)

## Implicit Rows

1. like default row (automatically creating new row)
2. `grid-auto-rows: 100px`

## Template Area

1. ideal for prototype

2. ```css
   .container{
       grid-template-areas:"h h h h h h h h h h" "m c c c c c c c c" "f f f f f f f f";
   }
   .item1{
       grid-area: h;
   }
   .item2{
       grid-area: m;
   }
   ```

3. `.` for blank in this area

4. the shape must be rectangle

## Named Line

```css
.container{
    grid-template-columns:[main-start] 1fr [content-start] 5fr [content-end main-end];
}
.header{
    grid-column: main-start / main-end;
}
.contend{
    grid-column: content;
}
```

1. if <name>-start and <name>-end is the same <name> then in the `grid-column` or `grid-row` can only use the <name> to indicate the area
2. if both the column and the row are defined with the same <name> then can use `grid-area:<name>`to indicate the area.

## Position Items

1. for item
2. `grid-column-start:`  & `grid-column-end`=> `grid-column: <start> / <end>` 1/3 or 1/span2 or 1/-1
3. `grid-row`
4. using `grid-column: auto / span 2` to give free starting position (comparing to other specific starting position) or just using `grid-column: span 2`
5. `grid-auto-flow:dense` will try to fit the grid without any blank causing by different sizes

## Justify & Align Items

1. `justify-content` for vertical gap among items
2. `align-content` for horizontal gap among items
3. options: start, end, center, space-between, space-evenly, space-around
4. `justify-items` & `align-items`
5. options: stretch, center, start, end
6. for specific item, use `justify-self` & `align-self`

## Auto-fit / Auto-fill & Minmax

1. `repeat(auto-fit, 100px)`
2. `repeat(auto-fit, minmax(100px, 1fr))`
3. auto-fill will add blank column to fit the extra spaces while auto-fit will increase the size of items to fit

# Floats

`float` : left, right, none

# Position

`position:`  usually with `top:` `left:` `right:` and `bottom:`

- **Static positioning** is the default that every  element gets — it just means "put the element into its normal position  in the document layout flow — nothing special to see here".
- **Relative positioning** allows you to modify an  element's position on the page, moving it relative to its position in  normal flow — including making it overlap other elements on the page.  This is useful for minor layout tweaks and design pinpointing.
- **Absolute positioning** moves an element completely  out of the page's normal layout flow, like it is sitting on its own  separate layer. From there, you can fix it in a position relative to the  edges of the page's `<html>` element (or its nearest  positioned ancestor element). This is useful for creating complex layout  effects such as tabbed boxes where different content panels sit on top  of one another and are shown and hidden as desired, or information  panels that sit off screen by default, but can be made to slide on  screen using a control button.
- **Fixed positioning** is very similar to absolute  positioning, except that it fixes an element relative to the browser  viewport, not another element. This is useful for creating effects such  as a persistent navigation menu that always stays in the same place on  the screen as the rest of the content scrolls.