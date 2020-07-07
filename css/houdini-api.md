# CSS Houdini

[[doc1]](https://developer.mozilla.org/en-US/docs/Web/Houdini),
[[doc2]](https://www.smashingmagazine.com/2020/03/practical-overview-css-houdini/),
[Browser Support](https://ishoudinireadyyet.com/)

A group of JavaScript APIs give developer the power to extend CSS by hooking into the [rendering](../performance/rendering.md) process of a browser’s rendering engine.

- High-level APIs: Affect rendering process
    + [Paint API](#paint-api)
    + [Layout API](#layout-api)
    + [Animation API](#animation-api)
- Low-level APIs: The foundation of high-level APIs
    + [Typed Object Model API](#typed-om)
    + [Custom Properties & Values API](#custom-prop)
    + [Font Metrics API](#font-metrics)
    + [Worklets](#worklets)

---

## Typed Object Model API [[doc]](https://developers.google.com/web/updates/2018/03/cssom) <a id="typed-om"></a>
CSS now has a proper object-based API for working with values in JavaScript.
- `HTMLElement.attributeStyleMap`: Return `StylePropertyMap` for parsing and modifying inline styles.
- `HTMLElement.computedStyleMap()`: Return `StylePropertyMapReadOnly`.

Assume `el` is a `<div> HTMLElement`:
```js
// CSSUnitValue represents values contain a single unit type.
el.attributeStyleMap.set('margin-top', CSS.px(10));
// equals to the following
// el.attributeStyleMap.set('margin-top', new CSSUnitValue(10, 'px'));
// el.attributeStyleMap.set('margin-top', '10px');
el.computedStyleMap().get('margin-top').value  // 10
el.computedStyleMap().get('margin-top').unit // 'px'

// CSSKeyworldValue for plain text values
el.attributeStyleMap.get('display') // null
el.computedStyleMap().get('display') // CSSKeywordValue {value: "block"}
el.attributeStyleMap.set('display', new CSSKeywordValue('initial'));
// equals to the following
// el.attributeStyleMap.set('display', 'initial');
el.attributeStyleMap.get('display').value // 'initial'
el.attributeStyleMap.get('display').unit // undefined
el.computedStyleMap().get('display') // CSSKeywordValue {value: "inline"}
```

And More...

---

## Custom Properties & Values API [[doc]](https://developer.mozilla.org/en-US/docs/Web/API/CSS_Properties_and_Values_API/guide) <a id="custom-prop"></a>
Allows the registration of CSS custom properties. Either by
- JavaScript
    ```js
    window.CSS.registerProperty({
        name: '--my-prop',
        syntax: '<color>', // How to parse the custom property
                           // Check CSS Values and Units for more options
        inherits: false,
        initialValue: '#c0ffee',
    });
    ```
    > - [`syntax` property spec](https://www.w3.org/TR/css-properties-values-api-1/#syntax-strings)
    > - [CSS Values and Units](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Values_and_units)
- Or CSS
    ```css
    @property --my-prop {
        syntax: '<color>';
        inherits: false;
        initial-value: #c0ffee;
    }
    ```

### Example
Browser doesn’t know how to handle gradient transition, but it knows how to handle color transitions because the custom property is specified as `<color>` type.
```css
.gradientBox { 
    background: linear-gradient(45deg, rgba(255,255,255,1) 0%, var(--colorPrimary) 60%);
    transition: --colorPrimary 0.5s ease;
    /* ... */
}

.gradientBox:hover {
    --colorPrimary: red
    /* ... */
}
```

### Caveat
- Once a property is registered, there's no way to update it, and trying to re-register it with JavaScript will throw an error indicating it's already been defined.
- Unlike standard properties, registered properties aren't validated when they're parsed. Rather, they're validated when they're computed. *(?)*

---

## Font Metrics API <a id="font-metrics"></a>
Allow developers to affect how text elements are being rendered on screen. (ex: multi-line dynamic text truncation)

> The Font Metrics API is still in a very early stage of development, so its specification may change in the future.

---

## Worklets [[doc]](../web-apis/worklet.md) <a id="worklets"></a>
An API for running scripts in various stages of the rendering pipeline independent of the main JavaScript execution environment.
 - Paint Worklet: Using Paint API to programmatically generate an image that responds to computed style changes.
- Animation Worklet: Using Animation API.
- Layout Worklet - Using Layout API.

---

## Paint API [[doc]](https://developer.mozilla.org/en-US/docs/Web/API/CSS_Painting_API/Guide) <a id="paint-api"></a>

Allows developers to use JavaScript functions to draw directly into an element’s background, border, or content such as CSS `background-image`, `border-image`, `mask-image`, etc.

Essentially, the API contains functionality allowing developers to create custom values for `paint()`, a CSS `<image>` function. (Hence it should be accepted by any CSS rule that needs a `<image>` type value like above)

### Define A Paint Worklet
- Write a worklet using the `registerPaint()` function.

```js
registerPaint('paintWorketExample', class {
    // An array of CSS custom properties that the Worklet will keep track of.
    // This array represents dependencies of a paint worklet.
    static get inputProperties() { return ['--myVariable']; }
    // An array of input arguments that can be passed from paint()
    // from inside the CSS.
    static get inputArguments() { return ['<color>']; }
    // Allow or disallow opacity for colors. If set to false,
    // all colors will be displayed with full opacity.
    static get contextOptions() { return {alpha: true}; }

    /**
     * @param ctx 2D drawing context, almost identical to Canvas API’s 2D drawing context.
     * @param size An object containing the width and height of the element.
     *    Values are determined by the layout rendering process.
     *    Canvas size is the same as the actual size of the element.
     * @param properties Input variables defined in inputProperties
     * @param args An array of input arguments passed in paint function in CSS
     */
    paint(ctx, size, properties, args) {
        /* ... */
    }
});
```

- Register the worklet with `CSS.paintWorklet.addModule()`.

```js
CSS.paintWorklet.addModule('path/to/worklet/file.js');
```

- Include the `paint()` CSS function.

```css
.exampleElement {
    /* paintWorkletExample - name of the worklet
       blue - argument passed to a Worklet */
    background-image: paint(paintWorketExample, blue);
}
```

Exmaple: [Ripple](https://github.com/GoogleChromeLabs/houdini-samples/tree/master/paint-worklet/ripple) with [demo](https://googlechromelabs.github.io/houdini-samples/paint-worklet/ripple/)

---
## Animation API [[doc]](https://developers.google.com/web/updates/2018/10/animation-worklet) <a id="animation-api"></a>
Allows for user action to control the flow of animation that runs in a performant, non-blocking way.

### Define A Animation Worklet
- Write a worklet using the `registerAnimator()` function.

```js
registerAnimator('animationWorkletExample', class {
    // Called when a new instance is created. Used for general setup.
    constructor(options) {
        /* ... */
    }
    /**
     * @param currentTime The current time value from the defined timeline
     * @param effect An array of effects that this animation uses
     */
    animate(currentTime, effect) {
        /* ... */
    }
});
```

- Register the worklet with `CSS.animationWorklet.addModule()`.

```js
CSS.animationWorklet.addModule('path/to/worklet/file.js');
```

- Define an effect
    * Keyframe Format [[doc]](https://developer.mozilla.org/en-US/docs/Web/API/Web_Animations_API/Keyframe_Formats): There should be at least two keyframes specified (representing the starting and ending states of the animation sequence). And keyframes without a specified `offset` will be evenly spaced between adjacent keyframes.
    * `EffectTiming` [[doc]](https://developer.mozilla.org/en-US/docs/Web/API/EffectTiming): Describe timing properties for animation effects.

```css
/* CSS version*/

#elementExmaple {
    animation: rotateAndChangeColor infinite 3s linear;
}

@keyframes rotateAndChangeColor {
    0% {
        color: #000;
        transform: rotate(0) translate3D(-50%, -50%, 0);    
    }
    30% {
        color: #431236;
    }
    100% {
        color: #000;
        transform: rotate(360deg) translate3D(-50%, -50%, 0);
    }
}
```
```js
/* To Javascript version */

const effectExample = new KeyframeEffect(
    elementExample,  /* Selected element that's going to be animated */
    [ /* Animation keyframes */
        { transform: 'rotate(0) translate3D(-50%, -50%, 0)', color: '#000' },
        { color: '#431236', offset: 0.3},
        { transform: 'rotate(360deg) translate3D(-50%, -50%, 0)', color: '#000' }
    ],
    { /* Animation options - duration, delay, iterations, etc. */
        duration: 3000,
        iterations: Infinity
    },
```

- Define an animation

```js
/* Create new WorkletAnimation instance and run it */
new WorkletAnimation(
    "animationWorkletExample"  /* Worklet name */
    effectExample,             /* Animation (effect) timeline */
    document.timeline,         /* Input timeline; or feed scroll positions
                                  as a timeline instead: ScrollTimeline */
    {},                        /* Options passed to constructor */
).play();                      /* Play animation */
```

---
## Layout API [[doc]]() <a id="layout-api"></a>
Allows developers to extend the browser’s layout rendering process by defining new layout modes that can be used in `display` CSS property. 

### Define A Layout Worklet
- Write a worklet using the `registerLayout()` function.

```js
registerLayout('exampleLayout', class {
    // An array of CSS custom properties that the Worklet will keep track of
    // that belongs to a Parent Layout element
    static get inputProperties() { return ['--exampleVariable']; }
    // An array of CSS custom properties that the Worklet will keep track of
    // that belong to child elements of a Parent Layout element
    static get childrenInputProperties() { return ['--exampleChildVariable']; }
    static get layoutOptions() {
        return {
            // Can have a pre-defined value of block or normal.
            // Determines if the boxes will be displayed as blocks or inline.
            childDisplay: 'normal',
            // Can have a pre-defined value of block-like or manual.
            // It tells the browser to either pre-calculate the size or not to
            // pre-calculate (unless a size is explicitly set), respectively.
            sizing: 'block-like'
        };
    }
    /**
     * Defines how a box or its content fits into a layout context.
     * @param children Child elements of a Parent Layout element.
     * @param edges Layout Edges of a box.
     * @param styleMap Typed OM styles of a box.
     */
    intrinsicSizes(children, edges, styleMap) {
        /* ... */
    }
    /**
     * @param children Child elements of a Parent Layout element.
     * @param edges Layout Edges of a box.
     * @param constraints Constraints of a Parent Layout.
     * @param styleMap Typed OM styles of a box.
     * @param breakToken Break token used to resume a layout in case of
     *   pagination or printing.
     */
    layout(children, edges, constraints, styleMap, breakToken) {
        /* ... */
    }
});
```

- Register the worklet with `CSS.layoutWorklet.addModule()`.

```js
CSS.layoutWorklet.addModule('path/to/worklet/file.js');
```

-  Use the `layout()` CSS function in `display` CSS property.

```css
.exampleElement {
    display: layout(exampleLayout);
}
```

Example: [Masonry layout](https://github.com/GoogleChromeLabs/houdini-samples/tree/master/layout-worklet/masonry) with [demo](https://googlechromelabs.github.io/houdini-samples/layout-worklet/masonry/)