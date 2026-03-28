# CH4: Request Lines

## L2: Parsing the Request Line

<p>
    Remember how HTTP messages start with a start-line? Well, if it's a request (not a response), then the start-line is called the request-line and has a specific format.
</p>

HTTP-version  = HTTP-name "/" DIGIT "." DIGIT
HTTP-name     = %s"HTTP"
request-line  = method SP request-target SP HTTP-version

## L3: Parsing the stream

Unfortunately parsing code tends to be just one edge case after another. Remember how I said TCP guarantees data to be in order? That's true, but I never said it had to be complete. TCP (and by extension, HTTP) is a streaming protocol, which means we receive data in chunks and should be able to parse it as it comes in.

So, instead of a full HTTP request, we might just get the first few characters, like this:

GE

We need to manage the state of our parser to handle incomplete reads
