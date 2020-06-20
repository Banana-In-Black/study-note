# Performance

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

## Audit Tools
- [PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/)