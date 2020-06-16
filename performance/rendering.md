# Rendering

## The Pixel Pipeline
![Pipeline](https://developers.google.com/web/fundamentals/performance/rendering/images/intro/frame-full.jpg)

- **Javascript**: It's used to trigger a visual changes for the most of time, but not always. (or though CSS animation, transition, etc.)
- **Style** (calulations): The process of applying CSS rules on matched elements.
- **Layout** <sub>*skippable*</sub>: Caculate the box model (how much space it needs) to determine the position on screen.
- **Paint** <sub>*skippable*</sub>: Draw out essentially every visual part(text, colors, images, and shadows, etc.) of the elements onto multiple layers.
- **Compositing**: Combine multiple layers in correct order.

You won’t always necessarily touch every part of the pipeline on every frame. There're three ways: (The lesser work the better)
- JS / CSS > Styles > Layout > Paint > Composite
- JS / CSS > Styles ---------> Paint > Composite
- JS / CSS > Styles -----------------> Composite

If you want to know which of the three versions above changing any given CSS property will trigger head to [CSS Triggers](https://csstriggers.com/).

## Optimizing Javascript Execution [[doc]](https://developers.google.com/web/fundamentals/performance/rendering/optimize-javascript-execution)
- Avoid `setTimeout()` or `setInterval()` for visual updates; always use `requestAnimationFrame()` instead (mainly for animation).
- Move long-running JavaScript off the main thread to Web Workers.
- Use micro-tasks to make DOM changes over several frames.
- Use Chrome DevTools to assess the impact of JavaScript.