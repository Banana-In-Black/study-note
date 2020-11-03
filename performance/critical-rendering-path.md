# Critical Rendering Path

- Constructing the Object Model: Bytes → Characters → Tokens → Nodes → Object Model (DOM & CSSOM)

## The difference between `DOMContentLoaded` and `load` event

- `DOMContentLoaded`: Event fires when the initial HTML document has been completely loaded and parsed, without waiting for stylesheets, images, and subframes to finish loading.

    > <script.defer> will be executed after the document has been parsed, but before firing `DOMContentLoaded`.

- `load`: Event is fired when the whole page has loaded, including all dependent resources such as stylesheets and images.

### Events Alternative: `Document.readState` [[doc]](https://developer.mozilla.org/en-US/docs/Web/API/Document/readyState)

- loading: The document is still loading.
- interactive: The document has finished loading and the document has been parsed but sub-resources such as images, stylesheets and frames are still loading. **The state indicated that the `DOMContentLoaded` event is fired**.
- complete: The document and all sub-resources have finished loading. **The state indicates that the `load` event is fired**.