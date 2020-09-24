# Web Vital Metrics [[doc]](https://web.dev/vitals/)

We can use [`Event Timing API`](https://wicg.github.io/event-timing/) and [`Element Timing API`](https://wicg.github.io/element-timing/) to measuring web vitals in the future, now they're still drafts.

Google might use these mertics as a [search ranking factor](https://support.google.com/webmasters/answer/9205520?hl=en).

## Largest Contentful Paint (LCP) [[doc]](https://web.dev/lcp/)

The time since a page was started that the largest block of meaningful content was loaded, such as a big image, block of text.

## Cumulative Layout Shift (CLS) [[doc]](https://web.dev/cls/)

The sum of all the layout shifts that happen on a page.

Layout Shift happens whenever new elements added to the page move the placement of other elements. When there is a lot of asynchronous content, there are lots of layout shifts and the CLS is high.

### Optimization [[doc]](https://web.dev/optimize-cls/)

The most common causes of a poor CLS are:

#### Images without dimensions

Always include width and height size attributes on your images and video elements. Alternatively, reserve the required space with [CSS aspect ratio boxes](https://css-tricks.com/aspect-ratio-boxes/).

##### Relative CSS Properties

- The [`object-fit`](https://developer.mozilla.org/en-US/docs/Web/CSS/object-fit) CSS property sets how the content of a replaced element, such as an `<img>` or `<video>`, should be resized to fit its container.
- You can alter the alignment of the replaced element's content object within the element's box using the [`object-position`](https://developer.mozilla.org/en-US/docs/Web/CSS/object-position) property.
- Use [`aspect-ratio`](https://developer.mozilla.org/en-US/docs/Web/CSS/aspect-ratio) in the futre.

> Addtional reading: [Responsive Images](https://css-tricks.com/responsive-images-css/)

#### Ads, embeds, and iframes without dimensions

#### Dynamically injected content

#### Web Fonts causing FOIT/FOUT

#### Actions waiting for a network response before updating DOM

## First Input Delay (FID) [[doc]](https://web.dev/fid/)

How long the page is busy when the user tries to interact with the page for the first time.
