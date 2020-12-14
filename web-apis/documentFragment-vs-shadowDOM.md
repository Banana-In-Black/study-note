# `DocumentFragment` vs Shadow DOM

## `DocumentFragment` [[MDN]](https://developer.mozilla.org/en-US/docs/Web/API/DocumentFragment)

The `DocumentFragment` interface represents a minimal `document` object that has no parent. It is used as a lightweight version of `Document` that stores a segment of a document structure comprised of nodes just like a standard document. The key difference is due to the fact that the document fragment isn't part of the active document tree structure. Changes made to the fragment don't affect the document (even on reflow) or incur any performance impact when changes are made.

### Usage

A common use for DocumentFragment is to create one, assemble a DOM subtree within it, then append or insert the fragment into the DOM using Node interface methods such as appendChild() or insertBefore(). Doing this moves the fragment's nodes into the DOM, leaving behind an empty DocumentFragment. Because all of the nodes are inserted into the document at once, only one reflow and render is triggered instead of potentially one for each node inserted if they were inserted separately.

## Shadow DOM [[doc]](https://developers.google.com/web/fundamentals/web-components/shadowdom) [[MDN]](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Using_shadow_DOM)

Shadow DOM is designed as a tool for building component-based apps. Therefore, it brings solutions for common problems in web development:

- __Isolated DOM__: A component's DOM is self-contained (e.g. document.querySelector() won't return nodes in the component's shadow DOM).
- __Scoped CSS__: CSS defined inside shadow DOM is scoped to it. Style rules don't leak out and page styles don't bleed in.
- __Composition__: Design a declarative, markup-based API for your component.
- __Simplifies CSS__: Scoped DOM means you can use simple CSS selectors, more generic id/class names, and not worry about naming conflicts.
- __Productivity__: Think of apps in chunks of DOM rather than one large (global) page.
