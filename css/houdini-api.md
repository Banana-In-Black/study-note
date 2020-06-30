# CSS Houdini

[[doc1]](https://developer.mozilla.org/en-US/docs/Web/Houdini),
[[doc2]](https://www.smashingmagazine.com/2020/03/practical-overview-css-houdini/),
[Browser Support](https://ishoudinireadyyet.com/)

A group of JavaScript APIs give developer the power to extend CSS by hooking into the [rendering](../performance/rendering.md) process of a browser’s rendering engine.

- High-level APIs: Affect rendering process
    + [Paint API](#paint-api)
    + Layout API
    + Animation API
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

Essentially, the API contains functionality allowing developers to create custom values for `paint()`, a CSS `<image>` function.

### Define A Paint Worklet
- Write a paint worklet using the `registerPaint()` function.
- Register the worklet with `CSS.paintWorklet.addModule()`.
- Include the `paint()` CSS function.

```js
registerPaint('paintWorketExample', class {
    // An array of CSS custom properties that the Worklet will keep track of.
    // This array represents dependencies of a paint worklet.
    static get inputProperties() { return ['--myVariable']; }
    // An array of input arguments that can be passed from paint() from inside the CSS.
    static get inputArguments() { return ['<color>']; }
    // Allow or disallow opacity for colors. If set to false, all colors will be displayed with full opacity.
    static get contextOptions() { return {alpha: true}; }

    /**
     * @param ctx: 2D drawing context, almost identical to Canvas API’s 2D drawing context.
     * @param size: An object containing the width and height of the element. Values are determined by the layout rendering process. Canvas size is the same as the actual size of the element.
     * @param properties: Input variables defined in inputProperties
     * @param args: An array of input arguments passed in paint function in CSS
     */
    paint(ctx, size, properties, args) {
        /* ... */
    }
});
```

Exmaple: [Ripple](https://github.com/GoogleChromeLabs/houdini-samples/tree/master/paint-worklet/ripple)