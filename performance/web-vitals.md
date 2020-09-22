# Web Vital Metrics [[doc]](https://davidwalsh.name/vital-web-performance)

We can use [`Event Timing API`](https://wicg.github.io/event-timing/) and [`Element Timing API`](https://wicg.github.io/element-timing/) to measuring web vitals in the future, now they're still drafts.

Google might use these mertics as a [search ranking factor](https://support.google.com/webmasters/answer/9205520?hl=en).

## Largest Contentful Paint (LCP) [[doc]](https://web.dev/lcp/)

The time since a page was started that the largest block of meaningful content was loaded, such as a big image, block of text.

## Cumulative Layout Shift (CLS) [[doc]](https://web.dev/cls/)

The sum of all the layout shifts that happen on a page.

Layout Shift happens whenever new elements added to the page move the placement of other elements. When there is a lot of asynchronous content, there are lots of layout shifts and the CLS is high.

## First Input Delay (FID) [[doc]](https://web.dev/fid/)

How long the page is busy when the user tries to interact with the page for the first time.
