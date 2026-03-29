# CH8: Chunked Encoding

## Chunked Encoding
Up to this point, we've assumed that HTTP messages are parsed and sent byte by byte, but we've still treated the message body as a single block of data. This works great for many use cases, but what happens if we want to send updates bit by bit? Well, remember our fundamental unit of HTTP, the HTTP-message:

```
     HTTP-message   = start-line CRLF
                      *( field-line CRLF )
                      CRLF
                      [ message-body ]
```

Turns out, [ message-body ] can be a bit deceiving... it's a rather flexible field that can contain a variable length of data, known only as its sent by making use of the Transfer-Encoding header rather than the Content-Length header. Here's the format:

```
HTTP/1.1 200 OK
Content-Type: text/plain
Transfer-Encoding: chunked

<n>\r\n
<data of length n>\r\n
<n>\r\n
<data of length n>\r\n
<n>\r\n
<data of length n>\r\n
<n>\r\n
<data of length n>\r\n
... repeat ...
0\r\n
\r\n
```

Where <n> is just a hexadecimal number indicating the size of the chunk in bytes and <data of length n> is the actual data for that chunk. That pattern can be repeated as many times as necessary to send the entire message body. Here's a concrete example with plain text:

```
HTTP/1.1 200 OK
Content-Type: text/plain
Transfer-Encoding: chunked

1E
I could go for a cup of coffee
C
But not Java
12
Never go full Java
0
```

Chunked encoding is most often used for:
<ul>
    <li>
        Streaming large amounts of data (like big files)
    </li>
    <li>
        Real-time updates (like a chat-style application)
    </li>
    <li>
        Sending data of unknown size (like a live feed)
    </li>
</ul>

We need our server to support chunked responses because occasionally our server acts as a proxy for another server. Any requests to the /httpbin/<path>  endpoint will be proxied to the amazing httpbin.org service: a wonderful online tool for testing HTTP stuff. So, for example, this request:

GET localhost:42069/httpbin/stream/100 will trigger a handler on our server that sends a request to https://httpbin.org/stream/100 and then forwards the response back to the client chunk by chunk.
The https://httpbin.org/stream/100 URL streams 100 JSON responses, making it a great way for us to test our chunked response implementation.

## Trailers

Did you think it was a bit odd that we had to write 0\r\n\r\n (two registered nurses) to signal the end of the chunked encoding?

There's actually a good reason for it – you can have additional headers at the end of chunked encoding, called (of course) Trailers. They work the same way that headers do with one catch: you have to specify the names of the trailers in a Trailer header. For example:

HTTP/1.1 200 OK
Content-Type: text/plain
Transfer-Encoding: chunked
Trailer: Lane, Prime, TJ

```
1E
I could go for a cup of coffee
C
But not Java
12
Never go full Java
0
Lane: goober
Prime: chill-guy
TJ: 1-indexer
```

They go between the two \r\n sequences in our original chunked encoding example.

Why Trailers?
Trailers are often used to send information about the message body that can't be known until the message body is fully sent. For example, the hash of the message body... in fact, let's do that.
