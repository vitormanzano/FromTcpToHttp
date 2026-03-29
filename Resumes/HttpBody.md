# CH6 - Http Body

A user agent SHOULD send Content-Length in a request...
And "should" has a specific meaning in RFCs per RFC2119:

This word, or the adjective "RECOMMENDED", mean that there may exist valid reasons in particular circumstances to ignore a particular item, but the full implications must be understood and carefully weighed before choosing a different course.

All this to say, for our implementation we're going to assume that if there is no Content-Length header, there is no body present. This is a safe assumption for our purposes, though it might not be true in all cases in the wild.

type Request struct {
	RequestLine RequestLine
	Headers     headers.Headers
	Body        []byte
    // ...
}

Now compare it to the official Golang http.Request struct:

type Request struct {
    Proto    string
    Method   string
    URL      *url.URL
    Header   http.Header
    Body     io.ReadCloser
    // ...
}

There are a few differences:

We have a RequestLine struct, while they've elevated the Method, Target, and Version into their own top-level fields. It's basically the same data, but the layout is a bit different.
We have a Headers struct, they named theirs Header. Again, same data, slightly different layout and naming. I prefer to call them "headers" because that's usually how web developers refer to them, the Go stdlib refers to the entire collection of field lines as a "header"... but I digress.
The most important difference: we have a Body field that just stores the entire body as a []byte in memory, while the Go stdlib has an io.ReadCloser interface. Our solution is perhaps simpler, but it's more limiting to the users of our library... we force them to load the entire body into memory, before interacting with it. The Go stdlib allows the user to read the body in chunks, or even stream it to a file, which is often necessary for large bodies. More on this later.
