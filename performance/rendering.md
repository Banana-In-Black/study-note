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

## Optimizing Style Caculations [[doc]](https://developers.google.com/web/fundamentals/performance/rendering/reduce-the-scope-and-complexity-of-style-calculations)

Roughly 50% of the time used to calculate the computed style for an element is used to match selectors, and the other half of the time is used for constructing the RenderStyle (computed style representation) from the matched rules.

- Reduce the complexity of your selectors; use a class-centric methodology like [BEM](../css/naming-convention-and-methodologies.md#bem).
- Reduce the number of elements on which style calculation must be calculated.

## Optimizing Layout process [[doc]](https://developers.google.com/web/fundamentals/performance/rendering/avoid-large-complex-layouts-and-layout-thrashing)

- Avoid layout wherever possible; layout is normally scoped to the whole document. and the number of DOM elements will affect performance.
- Assess layout model performance; new Flexbox is typically faster than older Flexbox or float-based layout models.
- **Avoid forced synchronous layouts and layout thrashing**; read style values (where the browser can use the previous frame’s layout values) then make style changes.

```js
// Schedule our function to run at the start of the frame.
requestAnimationFrame(logBoxHeight);

function logBoxHeight() {
    // The browser must first apply the style change
    // and then run layout to get style values
    // ANTI: box.classList.add('super-big');

    // Gets style values before style change
    // to prevent unnecessary layout process
    console.log(box.offsetHeight);

    box.classList.add('super-big');
}
```

```js
/* Anti-pattern: layout thrashing */
function resizeAllParagraphsToMatchBlockWidth() {
    // Puts the browser into a read-write-read-write cycle.
    for (var i = 0; i < paragraphs.length; i++) {
        paragraphs[i].style.width = box.offsetWidth + 'px';
    }
}

/* Fixed */
// Read.
var width = box.offsetWidth;
function resizeAllParagraphsToMatchBlockWidth() {
    for (var i = 0; i < paragraphs.length; i++) {
        // Now write.
        paragraphs[i].style.width = width + 'px';
    }
}
```

## Optimizing Painting [[doc]](https://developers.google.com/web/fundamentals/performance/rendering/simplify-paint-complexity-and-reduce-paint-areas)

- Changing any property apart from `transforms` or `opacity` always triggers paint.
- Paint is often the most expensive part of the pixel pipeline; avoid it where you can.
- Reduce paint areas through layer promotion (using `with-change`, `transform: translateZ(0)`, etc. to create a new layer, but **don't do it without profiling.**) and orchestration<sub>[[1]](#note1)</sub> of animations.

## Stick to Compositor-Only Properties and Manage Layer Count [[doc]](https://developers.google.com/web/fundamentals/performance/rendering/stick-to-compositor-only-properties-and-manage-layer-count)

- Stick to `transform` and `opacity` changes for your animations.
- Promote moving elements with `will-change` or `translateZ`.
- Avoid overusing promotion rules; layers require memory and management.

---
<span id="note1">[1]: 編排</span>
