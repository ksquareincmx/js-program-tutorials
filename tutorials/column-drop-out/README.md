# Column Drop Out

It's a pattern that consits of a multi-column layout that ends up with a single column layout. This pattern can be mixed with mostly fluid layout (for column size).

## What we're gonna do

We already have a portfolio project with a skills section. But right now it doesn't support RWD. We're gonna add column drop out. The portfolio is a simple page: just html and css (nothing fancy).

And this is how it should look like by the end of the tutorial:

- [Portfolio page on desktop](https://github.com/ksquareincmx/js-program-tutorials/blob/master/tutorials/column-drop-out/screenshots/001-desktop.png)
- [Portfolio page on tablet](https://github.com/ksquareincmx/js-program-tutorials/blob/master/tutorials/column-drop-out/screenshots/002-tablet.png)
- [First half of portfolio page on mobile](https://github.com/ksquareincmx/js-program-tutorials/blob/master/tutorials/column-drop-out/screenshots/003-first-half-mobile.png)
- [Second half of portfolio page on mobile](https://github.com/ksquareincmx/js-program-tutorials/blob/master/tutorials/column-drop-out/screenshots/004-second-half-mobile.png)

## Using the example

You can follow along using the example that we have [here](https://github.com/ksquareincmx/js-program-tutorials/blob/master/examples/column-drop-out/). If you need help running the example you can check out the `README` [file](https://github.com/ksquareincmx/js-program-tutorials/blob/master/README.md).

## Solution

You can see the solution [here](https://github.com/ksquareincmx/js-program-tutorials/blob/master/examples/column-drop-out/solution/)

## Current state

By default the example already comes with some css for colors and sizes. It should looke like [this](https://github.com/ksquareincmx/js-program-tutorials/blob/master/tutorials/column-drop-out/screenshots/005-current-state.png). We're not interested on the styling of the page (that's why we're not gonna stop explaining it), but rather focus on the column drop out layout for the skills section.

If you really need the explanation of the styles you can check out this [link](#todo).

## Let the fun begin

There are multiple ways to create layouts using css, but we're gonna focus on just three: floats, inline-block elements and flexbox.

### floats

Floats are a pretty simple (but effective) mechanism. Every page has a layout flow. This means that every element must follow in concordance with the other elements of the page. For example: block elements cannot ocupy the same row. If the page encounters two block elements the browser renders the first that encounter on top and the second one below the first one.

When an element breaks the flow it no longer works with in concordance with the rest but independently. Floats are used precisely to break this flow. Either to the left or either to the right.

> Note:
> You can read more about floats and page layout here:
>
> - [Flow Layout and Overflow](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flow_Layout/Flow_Layout_and_Overflow)
> - [CSS Flow Layout](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flow_Layout)
> - [All about floats](https://css-tricks.com/all-about-floats/)

We have every skill card wrapped inside a div with the class column like this:

```html
<div class="column">
  <div class="card">
    <!-- rest of the code -->
  </div>
</div>
```

First we must tell column to float to the left:

```css
.column {
  float: left;
}
```

This is force our cards to have semi-fixed width. This width is determined by the size of the content inside the cards. The larger the content the larger the card. Like [this](https://github.com/ksquareincmx/js-program-tutorials/blob/master/tutorials/column-drop-out/screenshots/006-float-left.png).

But what we actually want the card to have a width of 33% aprox. So we add the width property to our css.

```diff
.column {
  float: left;
+ width: 33%;
}
```

With this no matter the size of the screen the cards will always occupy 33% of the screen's size.

Remember: floats break the flow layout (don't confuse it with column drop out layout). So even though our example works we have a little problem; skills container doesn't have a height.

This is because now the columns work independently and skills has no content. And becouse it doesn't have content it has nothing to render therefore it has a height of zero. There are multiple ways to fix this issue: one of those is to give skills container the property overflow auto like this:

```diff
.skills {
  margin-bottom: 1rem;
+ overflow: auto;
}
```

> Note:
>
> [Here](https://stackoverflow.com/questions/9446988/expanding-the-parent-container-with-100-height-to-account-for-floated-content/9447057#9447057)'s an awesome explanation of why this happens and other ways to fix it.

If we take a closer look to our [design](https://github.com/ksquareincmx/js-program-tutorials/blob/master/tutorials/column-drop-out/screenshots/001-desktop.png) on desktop it appears to have some spacing between the elements.

We can fix this issue giving the columns a padding of 16 pixels (left and right).

```diff
.column {
  float: left;
+ padding: 0 1rem;
  width: 33.33%;
}
```

Now we have some space but [our cards are not aligned anymoar](https://github.com/ksquareincmx/js-program-tutorials/blob/master/tutorials/column-drop-out/screenshots/008-columns-padding-issue.png). Our cards are supposed to have a width of 33% but if we [inspect them we can see that they're actually larger](https://github.com/ksquareincmx/js-program-tutorials/blob/master/tutorials/column-drop-out/screenshots/009-inspect-column-cards.png).

This is caused by the box model. The size of an element is not only determined by the width of the element, but by sum of: content size, width, padding and margin.

We specified the width of our card to be 33% and a padding of 16px. This results on the card not fitting in the same row.

To fix this we can use a property of `box-sizing` called `border-box`.

```diff
.column {
+ box-sizing: border-box;
  float: left;
  padding: 0 1rem;
  width: 33.33%;
}
```

`box-sizing` indicates to the browser to calculate the size of the element subtracting the border size and padding from the width. The default behavior (`context-box`) is the opposite: it calculates the size of the element based on the width plus the border size and the padding.

Now our cards adjust (again) not matter the size of the screen.

> Notes:
>
> - [Introduction to the CSS basic box model](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Box_Model/Introduction_to_the_CSS_box_model)
> - [box-sizing - MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/box-sizing)

If we resize our window (or use the dev tools) to simulate a mobile device we can see that our [cards have an overflow](https://github.com/ksquareincmx/js-program-tutorials/blob/master/tutorials/column-drop-out/screenshots/010-no-mobile-styles.png). This is where the column drop out comes handy.

When we reach certain point we're gonna move from thre columsn to two and then one. But, how do we decide that (break) point? Depends on three factors: design, device and whatever looks good.

Design: some designers like to add specific breakpoints for the design (this is not the case).
Device: other designers preffer to follow standard breakpoints based on the most common devices (this is not the case neither).
Whatever looks good: this approach is more common between developers than designers. the idea is simple: if the design breaks, then change the layout/styles.

We're gonna follow whatever looks good (WLG). With this approach we don't have to worry about specific devices nor platforms (mobile/web).

Let's resize the screen and let's try to find a breakpoint (the size in which the design breaks or doesn't look good anymoar).

[The first breakpoint](https://github.com/ksquareincmx/js-program-tutorials/blob/master/tutorials/column-drop-out/screenshots/011-first-breakpoint.png) is at `968` pixels of width. At this breakpoint we're gonna change the layout from three columns to two columns. To do that we must add a media query with our styles.

A media query is just a condition: if this condition is true then apply this styles, else do nothing. To change the layout from three to two columns we just change the width of the columns to 50 percent, like this:

```diff
.column {
  /* ... rest of the code */
}

+@media all and (max-width: 968px) {
+  .column {
+    width: 50%;
+  }
+}
```

> Note:
>
> [Here](https://css-tricks.com/css-media-queries/)'s an introduction to media queries.

So we're telling the browser: if the size of the screen is smaller than `968` pixels then set the width of every element with class column to `50%`.

Now it should looks like [this](https://github.com/ksquareincmx/js-program-tutorials/blob/master/tutorials/column-drop-out/screenshots/012-two-columns.png).

But now we have another problem: there's no separation between the first and the third column. To fix that we only need to add a margin at the bottom:

```diff
.column {
  /* ... rest of the code */
}

@media all and (max-width: 968px) {
  .column {
+   margin-bottom: 16px;
    width: 50%;
  }
}
```

There are a few variations of this pattern. The first most common is to apply to the column that's dropping a 100% width. The second most common is to center the third column. And finally (the one we're using) let the third column stay below the first column. We're not gonna implement every single one of them but if you are interested you can find examples [here](https://www.lukew.com/ff/entry.asp?1514).

If we keep resizing we'll find our next breakpoint at 645 of width.

To change from two columns to one we simply add another media query width the width of the column equal to 100.

```diff
.column {
  /* ... rest of the code */
}

@media all and (max-width: 968px) {
  /* ... rest of the code */
}

+@media all and (max-width: 645px) {
+  .column {
+    margin-bottom: 16px;
+    width: 100%;
+  }
+}
```

And there we have it. Now it looks awesome on mobile devices.

> Note:
>
> Different designs require different amount and types of breakpoints. See [tiny tweaks](#todo)

### inline-block elements

Now let's comment (or remove?) all the code that we have written for the floating version.

Every html element has a display property: `none`, `inline`, `inline-block`, `block` and flex. By default containers (div, nav, header, main, etc) have the property `display` equals to `block`. This means that two elements cannot be in the same row. So we must tell the browser to change the columns from `block` to `inline-block`.

```css
.column {
  display: inline-block;
}
```

> Note:
>
> [Here](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Display)'s a list of tutorials on how to use the display property.

But [now we have the same problem](https://github.com/ksquareincmx/js-program-tutorials/blob/master/tutorials/column-drop-out/screenshots/013-wrong-card-size.png) that we had with float: the size of the column depends on the size of the content. So we just need to the the browser to apply a fixed width of `33%` to every column. And as we said earlier we need to add some spacing (padding) between the columns and the card and set `box-sizing` equal to `border-box` to subtract the `padding` from the `width`.

```diff
.column {
+ box-sizing: border-box;
  display: inline-block;
+ padding: 0 16px;
+ width: 33%;
}
```

We did exactly as we did on our previous example, but it's not working, why?

By default `inline-block` add a little space (that has a size) between elements. There are multiple ways to fight this issue: some more elegant than others and some that involve black magic.

The easiest one is to set the `font-size` of the container (parent) equals to zero, like this:

```diff
.column {
  /* ... rest of the code */
}

+.skills {
+  font-size: 0;
+}
```

Just make sure you explicitly add a font-size to any text/heading element that you have inside `.skills` or [it will dissapear](https://github.com/ksquareincmx/js-program-tutorials/blob/master/tutorials/column-drop-out/screenshots/014-no-font-size.png).

> Note:
>
> [Here](https://css-tricks.com/fighting-the-space-between-inline-block-elements/)'s a guide with multiple ways to eliminate the spacing between inline-block elements

The only thing missing now are the breakpoints. We do as before: we drop one column at `969` pixels.

```diff
.column {
  /* ... rest of the code */
}

.skills {
  /* ... rest of the code */
}

+@media all and (max-width: 969px) {
+  .column {
+    margin-bottom: 1rem;
+    width: 50%;
+  }
+}
```

And we drop again (from two to one) at max `646`:

```diff
.column {
  /* ... rest of the code */
}

.skills {
  /* ... rest of the code */
}

@media all and (min-width: 659px) and (max-width: 969px) {
 /* ... rest of the code */
}

+@media all and (max-width: 645px) {
+  .column {
+    margin-bottom: 1rem;
+    width: 100%;
+  }
+}
```

Here we must be careful: we must place our css in the right order. If we put the "mobile" query before the "tablet" query is not gonna work due to the cascading nature of css.

> Note:
>
> [Here](https://developer.mozilla.org/en-US/docs/Web/CSS/Cascade#Cascading_order)'s a link on how the cascading nature of css works.

And that's it.

### flexbox

## Solutions

You can check out the solution [here](https://github.com/ksquareincmx/js-program-tutorials/tree/master/examples/column-drop-out/solution). There are three secionts: floats, inline-block elements and flexbox. These sections are commented on purpose so you can test the one that you're interested in.

## Final Thoughts

So which approach should you follow? Short answer: whatever fits you. Long answer: depends on the project, team, your personal knowledge of css and preferences.
