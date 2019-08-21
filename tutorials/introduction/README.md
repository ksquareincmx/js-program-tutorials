# Introduction to Responsive Web Desing (RWD)

Responsive web design is a technique used to build web sites that can "response" to different devices (often called media). RWD has three pillars:

- Flexible Grid Based Layout
- Flexible Media
- Media Queires

> Note:
>
> If you're interested in know more about RWD you can find [here](https://alistapart.com/article/responsive-web-design/) the article that started the RWD revolution.

We're gonna go deep dive into each one of them, but first we must talk about viewports.

## Viewport

[According to MDN docs](https://developer.mozilla.org/en-US/docs/Mozilla/Mobile/Viewport_meta_tag) the viewport is "the area of the window in which web content can be seen".

For example, if I open a browser on a 2015 Macbook Pro (with the browser maximized) the viewport would be of 1425x5431 pixels. The viewport depends on the device and browser.

This is generally not an issue, except for one little details: small devices lie. According to MDN: "[Mobile devices] render pages in a virtual window or viewport [...] and then shrink the rendered result down."

This forces the user to do a zoom in to see the content. So, even when we use a fixed width for a specific device (like an iphone) the page is not gonna be rendered properly.

To combat this issue apple introduced the viewport meta tag, even though this is not standard almost every modern browser supports this property.

> Note:
>
> If you wanna read more about the meta viewport tag you can checkout the [MDN documentation](https://developer.mozilla.org/en-US/docs/Mozilla/Mobile/Viewport_meta_tag)

## Opnening a page using a mobile device

We have an example under [examples directory](examples/001-no-vieport.html), if we open it using a mobile device [we can see](screenshots/001-no-viewport.png) that the size of the viewport is actually bigger.

## Meta Viewport Property

The most common "form" of the meta viewport tag is the following:

```html
<meta name="viewport" content="width=device-width, initial-scale=1" />
```

If we add this piece of code to our html then it should work properly, but before doing it we're gonna explain a little bit how it works. So, let's dissect the attribute a little bit.

The `meta` tag is used to represents data about our html page (metadata).

The `name` attribute is set to `viewport` to indicate browsers to apply some values when we're visiting the page using a narrow device (i.e. mobile).

The attribute `content` is where all the magic happens. `content` can a comma-separated string value. In this case we're passing to parameters: `width` and `initial-scale`.

When we set `width` to `device-width` we tell the browser to stop rendering the page using a virtual viewport, and instead use the actual dimensions of the device.

And finally we use the `initial-scale` to tell the browser to use a ratio of `1.0` between the `device-wdith` and `device-height`. We could think of this as: just don't zoom in or zoom out when you load the page.

> Note:
>
> You can read more about the meta property on [MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta)

## Adding the property

To fix the previous example we can add the meta tag under the `head` of our html page like this:

```html
<head>
    <meta charset="UTF-8" />
    <title>With Viewport</title>
    <meta name="viewport" content="width=device-width, initial-scale=1" />
</head>
```

You can find the example [here](examples/002-with-viewport.html).

If we open our page using a mobile browser now it should look like [this](screenshots/002-with-viewport.png).

## Other configurations

We can actually use other configurations: like a specific width, a different scale ratio or even a max/min scale ratio. Even though this configurations are valid/permitted it is considered by many a bad practice.
