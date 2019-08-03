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

## Solution

You can find the solution [here](example/solution/).

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
