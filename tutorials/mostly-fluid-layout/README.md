# Mostly Fluid Layout

It's a pattern that consists of a multi-column layout that adjusts its proportions. Only in the smallest resolutions (mobile devices) it will changes to a single column layout. This pattern it's built on top of fluid layout.

## What we're gonna do

We already have a page with a blog section and a "new book" section. But it only has the skeleton implemented. We need to add the support for mostly fluid layout pattern.

This is how it should look like by the end of the tutorial:

- [Page on desktop](screenshots/001-desktop.png)
- [Page on tablet](screenshots/002-tablet.png)
- [Page on mobile](screenshots/003-mobile.png)

## Using the example

You can follow along using the example that we have [here](example/). If you need help running the example you can check out the [README](https://github.com/ksquareincmx/js-program-tutorials/blob/master/README.md) file.

## Current State

By default the example already comes with some css for colors and size. It should look like [this](screenshots/004-current-state.png). We're not interested on the styling of teh page (that's why we're not gonna stop explaining it), but rather focus on the column drop out layout for the skills section.

## Let the fun begin

As with [column drop out pattern](https://github.com/ksquareincmx/js-program-tutorials/blob/master/tutorials/column-drop-out/README.md) the mostly fluid layout can be solved in different ways. We're just gonna focus on three: floats, inline-block elements and flexbox.

### floats

Inside our markup we two divs with the classes `left` and `right` inside our main `wrapper`. like this:

```html
<div clas="wrapper">
  <div class="left">
    <!-- rest of the code -->
  </div>
  <div class="right">
    <!-- rest of the code -->
  </div>
</div>
```

By default every div element has a display property set to block. That tells the divs to use all available space and don't allow other elements to be part of the same "row". First we're gonna override that behavior and set the width property of the left column to `60%` and the width property of the right column to `40%`.

```diff
+.left {
+    width: 60%;
+}
+.right {
+    width: 40%;
+}
```

Now we can observe that the columns have a fixed width but they're not aligned. This is because by default every div element has the property `display` set to `block`. Block elements don't "share space" with other elements in the same "row". Fortunately floats can help us fix that issue. The only thing we need to do is float elements to the `left`.

> Note:
> We could actually float one to the left and the other to the right and we would obtain the same result.

```diff
.left {
+   float: left;
    width: 60%;
}
.right {
+   float: left;
    width: 40%;
}
```

[As we can see now](screnshots/006-float-left.png) our columns now are aligned. The only issue we have is we don't have enough "space" between columns. We can fix this by adding some padding.

```diff
.left {
+   box-sizing: border-box;
    float: left;
+   padding: 0 16px;
    width: 60%;
}
.right {
+   box-sizing: border-box;
    float: left;
+   padding: 0 16px;
    width: 40%;
}
```

> Note:
> If you want an explanation of how box sizing works check the column [drop out tutorial](https://github.com/ksquareincmx/js-program-tutorials/blob/master/tutorials/column-drop-out/README.md).

And with that now we have our spacing issue [fixed](screenshots/007-padding-fixed.png).

We're done with desktop and tablet, we only need to drop one of the columns (the right one) when we are on small devices (mobile).

To do that we need to add a media query like this:

```diff
.left {
  /* ... rest of the code */
}
.right {
  /* ... rest of the code */
}
+@media all and (max-width: 425px) {
+ .left,
+ .right {
+   float: none:
+   width: 100%;
+ }
+}
+
```

It should look like [this](screenshots/008-one-column-mobile.png). This media queries tells the browser to stop floating elements when the device is smaller than `426px` and to set the width of the columns to `100%` (all available space).

And tha's it.

### inline-block elements

Now let's comment (or remove?) all the code that we have written for the floating version.

By default every div element has a display property set to block. That tells the divs to use all available space and don't allow other elements to be part of the same "row". First we're gonna override that behavior and set the display property to `inline-block`.

```diff
+.left {
+  display: inline-block;
+}
+.right {
+  display: inline-block;
+}
```

This code is just gonna make sure two elements can be in the same row, but by default divs have a width of `100%`, so even though the two columns can technically be in the same row they take all the space they have available provoking them to stack one on top of the other. To fix that we only need to give them a fixed with that added it's not bigger to `100%`.

```diff
.right {
  display: inline-block;
+ width: 60%;
}
.left {
  display: inline-block;
+ width: 40%;
}
```

[The columns keep stacking](screenshots/009-stacked-columns.png). This caused by `inline-block`. By default browsers add a little spacing between `inline-block` elements causing the elements to take more space adding more than `100%`. To fix this wen do two things: subtract 1 percent to any of the columns or set the container `font-size` to `0`.

```diff
.right {
  display: inline-block;
  width: 40%;
}
.left {
  display: inline-block;
  width: 60%;
}
+.wrapper {
+ font-size: 0;
+}
```

[That fixes the issue](screenshots/010-no-font-size.png), but be careful: if have some element inside `wrapper` that relies on wrapper's `font-size` thru inheritance then it's gonna disappear.

> Note:
> For a deeper explanation on why this happens checkout the [column drop out tutorial](https://github.com/ksquareincmx/js-program-tutorials/blob/master/tutorials/column-drop-out/README.md).

Now let's just add some padding between columns:

```diff
.right {
+ box-sizing: border-box;
  display: inline-block;
+ padding: 0 16px;
  width: 40%;
}
.left {
+ box-sizing: border-box;
  display: inline-block;
+ padding: 0 16px;
  width: 60%;
}
.wrapper {
 /* rest of the code */
}
```

[Done, space between columns added](screenshots/007-padding-fixed.png). Now if we resize our browser a little bit [we'll notice something strange](screenshots/011-no-vertical-align.png).

By default every `inline` element (that includes `inline-block` elements) has a vertical alignment. In this case we need to set it to `top`.

```diff
.right {
  box-sizing: border-box;
  display: inline-block;
  padding: 0 16px;
+ vertical-align: top;
  width: 40%;
}
.left {
  box-sizing: border-box;
  display: inline-block;
  padding: 0 16px;
+ vertical-align: top;
  width: 60%;
}
.wrapper {
 /* rest of the code */
}
```

> Note:
> To learn more about vertical align check out [this](https://developer.mozilla.org/en-US/docs/Web/CSS/vertical-align) page from mdn.

[That fixes our alignment issue](screenshots/012-vertical-align-top.png). Now we only need to drop the right column when we're on a small device (mobile).

```diff
.right {
 /* rest of the code */
}
.left {
 /* rest of the code */
}
.wrapper {
 /* rest of the code */
}
+@media all and (max-width: 425px) {
+  .left .right {
+    width: 100%;
+  }
+  .right {
+    width: 100%;
+  }
+}
```

And we're done.

## flexbox

Let's add the basic things padding and fixed width to the columns.

```diff
+.right {
+  box-sizing: border-box;
+  padding: 0 16px;
+  width: 40%;
+}
+.left {
+  box-sizing: border-box;
+  padding: 0 16px;
+  width: 60%;
+}
```

and let's set the `display` property to `flex` on the `wrapper` parent:

```diff
.left {
    /* ... rest of the code */
}
.right {
    /* ... rest of the code */
}
+.wrapper {
+    display: flex;
+}
```

And that's pretty much it. Flexbox is gonna align them vertically to the top and will try to make them fit in the same row.

> Note:
> If you need a deeper explanation on how flexbox works checkout the [column drop out pattern](https://github.com/ksquareincmx/js-program-tutorials/blob/master/tutorials/column-drop-out/README.md)

The only thing missing is to drop the column on mobile devices. To do that we only add the same old media query and give a fixed width of `100%` to left and right column:

```diff
.left {
    /* ... rest of the code */
}
.right {
    /* ... rest of the code */
}
.wrapper {
    /* ... rest of the code */
}
+@media all and (max-width: 425px) {
+  .left {
+    width: 100%;
+  }
+  .right {
+    width: 100%;
+  }
+}
```

> Note:
> If you need a deeper explanation on how media queries works checkout the [column drop out pattern](https://github.com/ksquareincmx/js-program-tutorials/blob/master/tutorials/column-drop-out/README.md)

And that should do the trick.

## Fixed max width

In some really large devices like 2/3/4k monitors this patterns tend to look ["stretch out"](screenshots/013-larger-devices.png), to combat that some designers/front-end-devs tend to have a fixed max width. After certain size the `wrapper` just stops resizing (growing). To implement that we only need to add a pretty simple media query.

```diff
.left {
    /* ... rest of the code */
}
.right {
    /* ... rest of the code */
}
.wrapper {
    /* ... rest of the code */
}
@media all and (max-width: 425px) {
  /* ... rest of the code */
}
+@media all and (min-width: 1200px) {
+  .wrapper {
+    margin: 0 auto;
+    width: 1200px;
+  }
+}
```

An we're done

## Solutions

You can check out the solutions [here](example/solution). There are three secionts: floats, inline-block elements and flexbox. These sections are commented on purpose so you can test the one that you're interested in.

## Final Thoughts

So which approach should you follow? Short answer: whatever fits you. Long answer: depends on the project, team, your personal knowledge of css and preferences.
