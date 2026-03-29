# CH7 - HTTP Response

I Like Bun's Approach
In Bun (JavaScript, I know, sorry), they have a very simple server implementation that looks like this in code:

const server = Bun.serve({
    port: 8080,
    async fetch(req) {
      return new Response("Bun!");
    }
}

It's fairly similar to Go's http.ListenAndServe:

server := &http.Server{
    Addr: ":8080",
    Handler: http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        fmt.Fprintf(w, "Go!")
    }),
}
server.ListenAndServe()

We're going to follow a similar pattern, with a few differences that I'll point out as we get to them.

const port = 42069

```
func main() {
	server, err := server.Serve(port)
	if err != nil {
		log.Fatalf("Error starting server: %v", err)
	}
	defer server.Close()
	log.Println("Server started on port", port)

	sigChan := make(chan os.Signal, 1)
	signal.Notify(sigChan, syscall.SIGINT, syscall.SIGTERM)
	<-sigChan
	log.Println("Server gracefully stopped")
}
```

As you can see, we've settled on a server.Serve function. This is where all the magic happens. It accepts a port and starts handling requests that come in. In a future step we'll allow it to accept a handler function, but for now, we're going to hardcode the response.

Notice the sigChan code. This is a common pattern in Go for gracefully shutting down a server. Because server.Serve returns immediately (it handles requests in the background in goroutines) if we exit main immediately, the server will just stop. We want to wait for a signal (like CTRL+C) before we stop the server

## Content Length
For messages that do include content, the Content-Length field value provides the framing information necessary for determining where the data (and message) ends.

## Response
While there isn't an official list of all the headers that should be in most responses, there are a few that are common enough that we should include them. Namely:

Content-Length (The size of the response body)
Connection (Whether the connection should be kept alive or closed)
Content-Type (The MIME type of the response body)
So, let's build some more of the pieces we'll need to send valid dynamic responses. Remember that HTTP responses follow the same HTTP message format:

```
 HTTP-message   = start-line CRLF
                  *( field-line CRLF )
                  CRLF
                  [ message-body ]
```
The only difference is that the start-line is a status-line instead of a request-line. From RFC 9112:

```
status-line = HTTP-version SP status-code SP [ reason-phrase ]
```

Some interesting tidbits about the reason-phrase, from Section 4,

A client SHOULD ignore the reason-phrase content because it is not a reliable channel for information (it might be translated for a given locale, overwritten by intermediaries, or discarded when the message is forwarded via other versions of HTTP). A server MUST send the space that separates the status-code from the reason-phrase even when the reason-phrase is absent (i.e., the status-line would end with the space).
So, while reason phrases are typically included (and match one to one with the status code), they are not required and should be ignored by clients. Kinda funny.
