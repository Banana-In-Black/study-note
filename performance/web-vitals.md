# Web Vital Metrics

[[doc]](https://web.dev/learn-web-vitals/)

We can use [`Event Timing API`](https://wicg.github.io/event-timing/) and [`Element Timing API`](https://wicg.github.io/element-timing/) to measuring web vitals in the future, now they're still drafts.

Google might use these mertics as a [search ranking factor](https://support.google.com/webmasters/answer/9205520?hl=en).

## Largest Contentful Paint (LCP) [[doc]](https://web.dev/lcp/)

The time since a page was started that the largest block of meaningful content was loaded, such as a big image, block of text.

### Optimization [[doc]](https://web.dev/optimize-lcp/)

The most common causes of a poor LCP are:

#### Slow server response times

- Optimize your server
- Route users to a nearby CDN
- Cache assets
- Serve HTML pages cache-first
- [Establish third-party connections early](https://web.dev/preconnect-and-dns-prefetch/)

#### Render-blocking JavaScript and CSS

- Minify / compress CSS & JavaScript files
- [Defer non-critical CSS](https://web.dev/defer-non-critical-css/) ([load CSS files asynchronously](https://github.com/filamentgroup/loadCSS/blob/master/README.md))

```html
<link rel="preload" href="path/to/styles.css" as="style" onload="this.onload=null;this.rel='stylesheet'">
```

- Inline critical CSS ([critters-webpack-plugin](https://github.com/GoogleChromeLabs/critters))
- [Defer unused JavaScript](https://web.dev/reduce-javascript-payloads-with-code-splitting/) (code splitting / dynamically import)
- [Minimize unused polyfills](https://web.dev/serve-modern-code-to-modern-browsers/) (tree shaking)

#### Slow resource load times

- Optimize and compress images
- [Preload important resources](https://web.dev/preload-critical-assets/)
- Compress text files
- [Deliver different assets based on network connection](https://web.dev/adaptive-serving-based-on-network-quality/) (adaptive serving)
- Cache assets using a service worker

#### Client-side [rendering](https://developers.google.com/web/updates/2019/02/rendering-on-the-web)

- Minimize critical JavaScript
- Use server-side rendering
- Use pre-rendering (webpack: [prerender-loader](https://github.com/GoogleChromeLabs/prerender-loader))

## Cumulative Layout Shift (CLS) [[doc]](https://web.dev/cls/)

The sum of all the layout shifts that happen on a page.

Layout Shift happens whenever new elements added to the page move the placement of other elements. When there is a lot of asynchronous content, there are lots of layout shifts and the CLS is high.

### Optimization [[doc]](https://web.dev/optimize-cls/)

The most common causes of a poor CLS are:

#### Images without dimensions

Content had to be reflowed as each image started to appear. The text you were reading could suddenly move or disappear off-screen, especially on mobile devices.

- Always include `width` and `height` size attributes on your `<img>` elements because modern browsers now set the default `aspect-ratio` based on those attributes. (Currently only `<img>` has support)
- Alternatively, reserve the required space with [CSS aspect ratio boxes](https://css-tricks.com/aspect-ratio-boxes/). (Consider use this on `<video>` and `<iframe>` element)

##### Relative CSS Properties

- The [`object-fit`](https://developer.mozilla.org/en-US/docs/Web/CSS/object-fit) CSS property sets how the content of a replaced element, such as an `<img>` or `<video>`, should be resized to fit its container (stretching and cropping).
- You can alter the alignment of the replaced element's content object within the element's box using the [`object-position`](https://developer.mozilla.org/en-US/docs/Web/CSS/object-position) property.
- Use [`aspect-ratio`](https://developer.mozilla.org/en-US/docs/Web/CSS/aspect-ratio) in the futre.

##### Responsive Images

- [Responsive Images CSS](https://css-tricks.com/responsive-images-css/)
- [Serve Responsive Images](https://web.dev/serve-responsive-images/)
- [Art Direction (`<picture>` element)](https://developer.mozilla.org/en-US/docs/Learn/HTML/Multimedia_and_embedding/Responsive_images#Art_direction)

#### Actions waiting for a network response before updating DOM

#### Ads, embeds, and iframes without dimensions

- Statically reserve space for them.
- Avoid placing them near the top of the viewport. (Cause more content to move)

#### Dynamically injected content

Avoid inserting new content above existing content, unless in response to a user interaction. This ensures any layout shifts that occur are expected.

#### Web Fonts causing FOIT/FOUT

Downloading and rendering web fonts can cause layout shifts in two ways:

- The fallback font is swapped with a new font __(FOUT - flash of unstyled text)__
- "Invisible" text is displayed until a new font is rendered __(FOIT - flash of invisible text)__

The following tools can help you minimize this:

- [`font-display`](https://web.dev/font-display/) allows you to modify the rendering behavior of custom fonts with values such as `auto`, `swap`, `block`, `fallback` and `optional`. Unfortunately, all of these values (except opti`onal) can cause a re-layout in one of the above ways.  [[MDN]](https://developer.mozilla.org/en-US/docs/Web/CSS/@font-face/font-display)
- The [`Font Loading API`](https://web.dev/optimize-webfont-loading/#the-font-loading-api) can reduce the time it takes to get necessary fonts. [[MDN]](https://developer.mozilla.org/en-US/docs/Web/API/CSS_Font_Loading_API)

As of Chrome 83, I can recommend the following too:

- Using `<link rel=preload>` on the key web fonts: a preloaded font will have a higher chance to meet the first paint, in which case there's no layout shifting.
- Combining `<link rel=preload>` and `font-display: optional`

Read [Prevent layout shifting and flashes of invisible text (FOIT)](https://web.dev/preload-optional-fonts/) by preloading optional fonts for more details.

#### Animation

Prefer `transform` animations to animations of properties that trigger layout changes. ([CSS Triggers](https://csstriggers.com/))

## First Input Delay (FID) [[doc]](https://web.dev/fid/)

How long the page is busy when the user tries to interact with the page for the first time.
