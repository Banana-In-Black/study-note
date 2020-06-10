# Web Optimization

## [HTTP/2](https://developers.google.com/web/fundamentals/performance/http2/)

### Terminology: 
![Connection](/images/connection.svg)

- _Stream_: A bidirectional flow of bytes within an established connection, which may carry one or more messages.
- _Message_: A complete sequence of frames that map to a logical request or response message.
- _Frame_: The smallest unit of communication in HTTP/2, each containing a frame header, which at a minimum identifies the stream to which the frame belongs.

### Features:

- Binary Framing Layer: HTTP/2 communication is split into smaller messages & frames, each of which is encoded in *binary format*.
- Multiplexed Streams: Multiple streams in flight within the same connection (One TCP connection per origin)

![Multiplexing](/images/multiplexing.svg)

- Stream Prioritization
- Stateful Header Compression (HPACK)

![HeaderCompression](/images/header-compression.svg)

- HTTP/2 Server Push: The ability to send multiple responses for a single client request (through multiple streams).

---

## Critical Rendering Path

### The difference between DOMContentLoaded and load event
- DOMContentLoaded: Event fires when the initial HTML document has been completely loaded and parsed, without waiting for stylesheets, images, and subframes to finish loading.
- load: Event is fired when the whole page has loaded, including all dependent resources such as stylesheets and images.

#### Events Alternative: Document.readState
- loading: The document is still loading.
- interactive: The document has finished loading and the document has been parsed but sub-resources such as images, stylesheets and frames are still loading. **The state indicated that the `DOMContentLoaded` event is fired**.
- complete: The document and all sub-resources have finished loading. **The state indicates that the `load` event is fired**.